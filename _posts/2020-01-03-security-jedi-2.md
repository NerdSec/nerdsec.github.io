---
layout: post
title: "Security Jedi - Chapter 2: Vulnerability Management"
categories: ["Security Jedi"]
tags: ["Big Data", "Vulnerability Management"]
author: Nachiket Vartak & Darshan Parab
---

In a previous blog we have covered the motivation for starting this series. In case, you haven't read it yet, I urge you to first read it [here](/security jedi/2020/01/01/security-jedi-introduction.html) before proceeding.

Today, we will cover the origins of our **Cyber Security Analytics** platform. The first use case for implementing this stack was actually quite different. Vulnerability Management.

## Vulnerability Management Automation

Vulnerabilities are the biggest issue in Cyber Security. There are almost [30-40](https://www.cvedetails.com/browse-by-date.php) vulnerabilities discovered each day. Managing these, in an environment with a few thousand assets can seem quite daunting. There is simply too much data. Many times, a few of these vulnerabilities fall through the cracks of patch management and then are later used in a `Red Team` engagement. Yes, we have all been there.

So, we decided to automate the entire excercise. What if we could put the ownership of all these vulnerabilities on the individual stakeholders? Let them have a view of how horrible the situation is for their assets.

## The Beginning

We started with a quite simple architecture. We used the native [report publishing](https://docs.tenable.com/sccv/Content/PublishingSitesSettings.htm) feature present in Tenable. This allowed us to send the Vulnerability data over `https` to a webserver.

![VM Architecture](/assets/images/architecture.png)

We then used Logstash [http input](https://www.elastic.co/guide/en/logstash/current/plugins-inputs-http.html), to receive this data and then forward it to Elasticsearch. Then it was a simple matter of visualizing the data in Kibana and showing it to the world (just kidding).

## Issues

Although visualizing this data seems quite straight-forward and easy, there are some challenges with this approach:

1. Context

    The data clearly lacks context from a Business perspective. It limited to only vulnerability specific information and not really any information that makes it relevant to my environment.

2. No Tenable Security Center

    Obviously, not everyone will be using `TSC`. Well, then you could be using Nessus, OpenVAS, or Qualys. All of them support either CSV reports or pulling it over an API. More on this later in the blog.

## Elastic + Python

In order to solve the challenges that we discussed earlier, we decided to use the best programming language in the world.

    Python + Elastic image

The log ingestion mechanism still holds true. We receive the data over https in Logstash and dump it in a `csv` file using the [file output plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-file.html).

We then consume these `csv` files using scheduled python jobs. We implemented `Django Celery` to do this in our production, but this is a topic for some other blog in itself. For now, let's assume it was a cron job that ran every `5 minutes`.

### Data Pre-processing

Now that we have the csv files in a folder, we can start by listing the files and processing them.

```python
def VM_fileprocess():
    files = glob.glob('/path/to/vm/*.log')
    files_processed = []
    for file in files:
        # Check if logstash has written the data to the file
        if os.stat(file).st_size > 0:
            fr = open(file, 'r')
            lines = fr.readlines()
            fr.close()
            i = 0
            # Drop the metadata added by logstash
            for line in lines:
                if line.find('"Plugin","Plugin Name"')!=-1:
                    break
                else:
                    i = i+1
            # Write the data to a new file
            if i < len(lines):
                fw = open(file.split('.')[0]+'.csv', 'w')
                fw.writelines(lines[i:-2])
                fw.close()
                files_processed.append(file.split('.')[0]+'.csv')
            # Either delete the original file or archive it
            os.remove(file)
        else:
            pass
    return files_processed
```

The above code is quite straight-forward:

1. We list the files present in the `vm` directory.
2. We then iterate through them and check for the following:
    * File contains scan results received from Tenable SC.
    * Filter out the metadata added by **Logstash**.
    * Write the filtered data to a new file.
3. Delete the old files and return a list of files processed.

We need to do these steps to ensure that when we read the `csv` file in `python`, there are no garbage values.

### Data Processing

Once we have cleaned the `csv` or `log` files, we can start to enrich it and add some context to it. We will leverage the `pandas` library to enrich it.

```python
for file in files_processed:
    df = pd.read_csv(file)
    df.fillna("NA", inplace=True)
    df['_index']= 'vm-tenable'
    df['@timestamp'] = datetime.now()
    # Rename the columns - ECS - https://www.elastic.co/guide/en/ecs/current/index.html
    df.rename(columns={"Plugin":"process.pid","Plugin Name":"process.name","Family":"os.family","Severity":"event.criticality","IP Address":"server.address","Protocol":"network.protocol","Port":"server.port","Exploit?":"vulnerability.exploit.available","MAC Address":"server.mac","DNS Name":"host.hostname","NetBIOS Name":"host.name","Plugin Text":"event.description","Solution":"vulnerability.solution","Risk Factor":"risk_factor","CVE":"vulnerability.cve","First Discovered":"event.start","Last Observed":"event.end","Vuln Publication Date":"vulnerability.pubdate","Patch Publication Date":"vulnerability.patchdate","Exploit Frameworks":"vulnerability.exploit.name","Version":"Version","_index":"_index","_type":"_type","@timestamp":"@timestamp"},inplace=True)
    df.fillna(value={"event.description":"NA"},inplace=True)
```

We begin by iterating through the `processed_files` list and create respective `pandas DataFrames` to be indexed to Elasticsearch. We add a timestamp column to the `DataFrame` to associate a `timestamp` with the VM data, and then rename the columns to conform to ECS.

```python
....
    # Check for credentialized scans
    df["event.credentials"] = df["event.description"].str.contains("Credentialed checks : yes")
    df_creds = df[(df["event.credentials"]==True) & (df["process.pid"]==19506)][["server.address","event.credentials"]]
    # Drop the columns and merge on credential check = yes
    df.drop(["event.credentials"],axis=1,inplace=True)
    df = df.merge(df_creds,on="server.address",how="left")
    df.fillna(value={"event.credentials":False},inplace=True)
```

We then check if the scans were performed using proper credentials. This ensures that the vulnerability data is more accurate and contains detailed vulnerability details.

```python
....
    arr_av = []
    arr_wazuh = []

    unique_ip = df["server.address"].unique()
    # Enrich with information about the agents installed on the servers
    for x in unique_ip:
        query for av information
        query for wazuh/hids information
        query for other agent information
    df_av = pd.DataFrame.from_dict(arr_av)
    df_wazuh = pd.DataFrame.from_dict(arr_wazuh)
    df = pd.merge(df,df_av, on="server.address",how="left")
    df = pd.merge(df,df_wazuh, on="server.address",how="left")
```

We then query for information on the AV or HIDS agents on all our machines in scope. Depending on your environment, this information can be pulled from either a DB, API, or an ES index.

Although optional, this step should not be skipped as it can help you later on to prioritize your remediation priorities and also gives you more context on the the overall health of your environment.

```python
    inventoryusr = open('/run/secrets/invusr').readline().split('\n')[0]
    inventorypw = open('/run/secrets/invpw').readline().split('\n')[0]
    conn = pymssql.connect(server="10.x.x.x", port=3306, user=invusr, password=invpw,database="Inventory")
    stmt = "SELECT IP, Owner, ..., FROM Table_Name"
    df_vmdb = pd.read_sql(stmt,conn)
    df_vmdb.rename(columns = {Columns to be renamed},inplace=True)
    df = pandas.merge(df, df_vmdb, on='src.ip', how='left')
    es = Elasticsearch('cluster information')
    helpers.bulk(es, df.to_dict('records'))
    os.remove(file)
```

And finally, we add the business context to this information. This is the most crucial aspect of the entire exercise. Adding this allows you to visualize your risk based on either the business type or department; whatever is the requirement.

We can also take this a step further by automating the compliance based on a few simple checks, such as the number of open `Critical` or `High` vulnerabilities.

```python
# Get a unique list of all IP having either [Critical, High, or Medium vulnerability].
iplist = df.loc[df['event.severity'].isin(['Critical','High','Medium'])]['ip'].unique().tolist()
df_noncomplied = pandas.DataFrame(iplist, columns=['src.ip'])
# Create a DataFrame and add status of all IP as Non Complied
df_noncomplied['status'] = 'Non Complied'
# Merge with existing data
df = pandas.merge(df, df_noncomplied, on='src.ip', how='left')
# For data not found in the Non Complied dataframe, mark the status as complied.
df['status'].fillna('Complied', inplace=True)
```

Congratulations, you have now automated your entire Vulnerability Management process! No, not really. You still have a herculean task of actually closing all these vulnerabilities/issues. But, now you are in a better position to do it, with a better context.

## Kibana Magic

Till now, we have covered only the boring theory. Time to see the fruits of our labour!

**And, always remember**:

> Within infinite myths lies the eternal truth
>
> Who sees it all?
>
> Varuna has but a thousand eyes,
>
> Indra has a hundred,
>
> You and I, only two.
>
> -Devdutt Patnaik

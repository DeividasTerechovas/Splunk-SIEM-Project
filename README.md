# Splunk-SIEM-Project

We'll be using some SPL from the docs and installing Splunk on a Windows 10 virtual machine.

# Skill Learned
Splunk Installation and Configuration

Windows System Administration

Data Ingestion and Forwarding 

Search Processing Language (SPL)

Log Analysis

Dashboard Creation in Splunk

Security Monitoring and Incident Detection 

Basic SIEM Concepts

# Step 1: Install Splunk Enterprise after downloading it.

The first thing you need to do is go to the Splunk official website and download Splunk Enterprise.

After downloading the installer, take the following actions:

1. Accept the license agreement after opening the installation.
2. Enter your password and username (in this case, DTSOCVM).
3. Press "Next" and accept the installation.

After installation is complete, click "Finish". This will automatically open a new browser tab where you can log in using the username and password you set up earlier.

# Step 2: Install the Universal Forwarder

Installing the Splunk Universal Forwarder is the next step. You can use this program to transfer data to your Splunk Enterprise server from your own PC.

1. Go to the Splunk Universal Forwarder page and download the Universal Forwarder.
2. Accept the license agreement after downloading, then adjust the installation settings.
3. Configure your directory.
4. Enter the IP address of the Splunk Enterprise server (the one on which you previously installed Splunk), then the deployment server's port (8089), and the data reception port (9997).

# Set Up Splunk to Get Information

You must first set up Splunk to receive the forwarded data before proceeding.

1. Select Forwarding and Receiving under Settings.
2. Under receiving, select "Add New" and enter port 9997.
   
Now, go ahead and finish the installation of the Universal Forwarder.

# Step 3: Check Your Forwarding Configuration 

Return to Splunk Enterprice after installing and configuring the Universal Forwarer to make sure everything is configured correctly: 

1. Select Data Summary under Search & Reporting.
2. Your hostname will appear there if the installation went OK. This indicates that data is being successfully sent to your Splunk instance by your Universal Forwarder.

# Step 4: Use Splunk to Incorporate Sample Data

It's time to practice using some sample data now that Splunk is set up and getting data from your Universal Forwarder. The Bots V3 dataset, which contains a range of logs (AWS, Office 365, and streaming logs), will be used.

Follow these steps to add the dataset:

1.Download the Bots V3 dataset.
2.Unzip the dataset and place it in the Splunk apps folder located at: C:\Program Files\Splunk\etc\apps.
3.Restart Splunk to make the new data available.

Once Splunk restarts, you can go to Search & Reporting and search for index="Bots V3" to view the data.

# Step 5: Start Querying and Exploring Data
Now that your sample data is in Splunk, you can start querying it to extract useful information. You can use different search queries based on your needs. For example, you can search the dataset for specific source types like AWS logs, Office 365 data, or streaming logs.

# Step 6: Create a Simple Dashboard
In this section, I’ll show you how to create a simple dashboard in Splunk to answer some key questions. For example, let’s say you need to know how many firewall connections occurred today, and how many were allowed or blocked.

# Creating the Dashboard
In Splunk, go to Dashboards > Create New.
Name the dashboard
Choose Classic for the version.
Add an input time range to allow users to adjust the time period for the data.
Query for Total Connections
Now, let’s start querying for the total number of connections today.

Use a search like:

```
index=firewall 
| stats count by actions
```

This query counts the total number of actions (allowed/blocked).

To display the count, use a Single Value Panel to display the total number of connections.

Query for Allowed and Blocked Connections
To break this down further:

For allowed connections:

```
index=firewall action=allowed 
| stats count
```
For blocked connections:
```
index=firewall action=blocked 
| stats count
```
Add each of these queries to your dashboard in Single Value Panels and choose different colors for each one.

<img src="https://i.imgur.com/siB4zrm.jpeg" width="500">

# Step 7: Monitor Suspicious Activity
Now, let’s say you want to monitor for suspicious scanning activity on your network (e.g., external IPs trying to probe internal systems). To do this:

1. Create a query to track all connections to your internal systems:
```
index=firewall action=allowed OR action=blocked
| stats count by src_ip, dest_ip, dest_port
```
2. Set a threshold (e.g., 100 hits on internal IPs/ports).
3. Finally, apply an IP location lookup to identify the geographical location of suspicious IPs. You can use the geoip function to map the IP addresses to countries.

<img src="https://i.imgur.com/oz95sFN.png" width="500">

# Conclusion

We have installed and set up Splunk, ingested sample data, and built a simple dashboard to monitor firewall connections.

# Splunk-SIEM-Project

# Setting Up Splunk on Your Personal Computer

We will be setting up Splunk on a Windows 10 VM and using some SPL from documentation.

# Step 1: Download and Install Splunk Enterprise

The first thing you need to do is go to the Splunk official website and download Splunk Enterprise.

Once you've downloaded the installer, follow these steps:

1. Open the installer and agree to the license agreement.
2. Set your username (In this instance we used DTSOCVM) and your password.
3. Hit "Next" and let it install.
4. 
After installation is complete, click "Finish". This will automatically open a new browser tab where you can log in using the username and password you set up earlier.

# Step 2: Install the Universal Forwarder

The next step is to install the Splunk Universal Forwarder. This tool will allow you to send data from your personal computer to your Splunk Enterprise server.

1.Download the Universal Forwarder from the Splunk Universal Forwarder page.
2.After downloading, accept the license agreement, and customize your installation options.
3.Set your directory.
4.Input the Splunk Enterprise server's IP address (the one you installed Splunk on earlier), followed by port 8089 for the deployment server and port 9997 for receiving data.

# Configure Splunk to Receive Data

Before you continue, you need to configure Splunk to receive the forwarded data.

1. Go to Settings > Forwarding and Receiving.
2. Click "Add New" under receiving, and input port 9997.
   
Now, go ahead and finish the installation of the Universal Forwarder.

# Step 3: Verify Your Forwarding Setup 

Once the Universal Forwarer is installed and configured, head back to Splunk Enterprice and check if everything is set up proerly: 

1. Navigate to Search & Reporting > Data Summary.
2. If the installation was successfull, you'll see your hostname listed there. This means your Universal Forwarder is successfully sending data to your Splunk instance.

# Step 4: Ingest Sample Data into Splunk

Now that Splunk is installed and receiving data from your Universal Forwarder, it’s time to practice with some sample data. we will be using Bots V3 dataset, which includes a variety of logs (AWS, Office 365, and streaming logs)

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

# Conclusion

We have installed and set up Splunk, ingested sample data, and built a simple dashboard to monitor firewall connections.

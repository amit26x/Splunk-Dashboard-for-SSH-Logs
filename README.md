# Splunk-Dashboard-for-SSH-Logs
In this project, you'll build a Splunk dashboard for monitoring SSH logs. The dashboard will visualize login attempts, failed connections, and brute force attacks. You will create panels for SSH activity summaries and authentication trends. Geo-location analysis will highlight brute force origins. A shared time range filter will allow dynamic data exploration.

**Objective
Lab Set up**
**Task0: Setting up Time Range**
Add Time Range Button
Click on Add Input
Select Time and click on pencil icon
Set Label to Time Range and Token time_range
Again Add Input
Select Submit
**Note:** For all future panel, set the time to time_range for consistency.

**Task1: Authentication Overview Panels
Goal: Give a quick summary of SSH activity.**
Total SSH Events
Click on Add Panel
Under New, choose Single Value
Use Shared Time Picker time_range
Set Content Title to "Total SSH Events"
Enter the Search String as below
**source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
 | stats count AS "Total SSH Events"
Successful Logins**
Click on Add Panel

Under New, choose Single Value

Use Shared Time Picker time_range

Set Content Title to "Successful Logins"

Enter the Search String as below:

source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Successful SSH Login" 
| stats count AS "Successful Logins"
3. Failed Logins
Click on Add Panel
Under New, choose Single Value
Use Shared Time Picker time_range
Set Content Title to "Failed Logins"
Enter the Search String as below:
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Login"
   
4. Connection without Authentication
Click on Add Panel

Under New, choose Single Value

Use Shared Time Picker time_range

Set Content Title to "Invalid User Attempts"

Enter the Search String as below:

index=auth "sshd" "invalid user"
| stats count AS "Invalid User Attempts"

Task2: Login Activity Trends
Goal: Visualize login behavior over time and detect spikes.
1. Failed Logins by username
Click on Add Panel

Under New, choose Bar Chart

Use Shared Time Picker time_range

Set Content Title to "Failed Logins by username"

Enter the Search String as below:

source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Failed SSH Login" | top username
2. Possible Brute Force
Click on Add Panel

Under New, choose Statstics Table

Use Shared Time Picker time_range

Set Content Title to Possible Brute Force b IP Address

Enter the Search String as below:

source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts" | top id.orig_h

Task3: Visualizing Brute Force attack in geo-location
2. Visualizing Brute Force attack with geo-location
Click on Add Panel

Under New, choose Choropleth Map

Use Shared Time Picker time_range

Set Content Title to Brute Force attack with geo-location

Enter the Search String as below:

source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts" 
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"

# Splunk_SPL

***tstats queries:-***

Find list of indexes count

|tstats count where index=* OR index=_* by index

The above query return the list of indexes with total records count in each index

***Develop VendorID filter in the dashboard:***

Here I am building dropdown list for VendorIDs. In order to achieve this I followed below steps

1. Select the dashboard where you want to develop a filter. 
2. Go to Edit 
3. Select dropdown input from '+Add Input'
4. Click on edit button to edit the dropdown input 
5. Lable = 'Select VendorID', enabled Search on change, Token='VendorID_Token', Default = 'All', Initial Value = 'All', Static Options: Name=All, Value=*, Dynamic Options: Content Type = Inline Search, Search String = "index=main VendorID=$VendorID_Token$ | stats count by VendorID", Run Time = All Time, Field For Lable = VendorID, Field for Value = VendorID
6. Apply
7. Save the dashboard

I want to add submit button to this input panel. To achieve that I edited XML code as submitbutton='false' to submitbutton='true'
"fieldset submitButton="false""  --> "fieldset submitButton="true""

To hide filter there is an option called Hide Filter, Which will hide filter from the panel.

***The Filter panel's XML code starts with "Form" whereas the dashboard's (without filter panel) XML code start with "dashboard"***

To interlink filter to the dashboard, add filter_token name in the dashboard search query. Then the dashboard will generate based on filter selections.

Ex: index=main VendorID=$VendorID_Token$ | stats count by VendorID

******SummaryIndex******

Summary index can be helpful to collect existed index data in a new summary index based on schedule search in the background. Summery index is efficient for large vaolume of data. The advantage of summery search is that instead of searching for certain data in a big dataset it will create a small dataset with relevent conditions under summery index. Which gives less time to search/run dashboards and the lattancy would be low. 

Ex: I have a dataset called sales which contains customer, item, vendor, shipping data. My dashboard needs only vendor related information. Instead of searching vendor information in the sales dataset I can create a summary index on vendor data. Which will take less time to run dashboard. 

Create Summary Index:
1. Go to Settings 
2. Select Indexes under Data category
3. Select New Index
4. Give appropriate index name, in my case I am giving vendor_summary_index as my summary index name
5. Select Index Data Type whichever is suitable for the analysis, for me i choose event type datatype
6. Save summary index

Populate Data in Summary Index:
To populate vendor data in vendor_summary_index, I created a report as populate_vendor_summary. I edited schedule based on the data available in my sample dataset. I have data from 07/20/2021 to 07/27/2021. I want to push data from 07/23/2021 to 07/27/2021 on daily basis. To acheive this I scheduled my report as below
1. Schedule: Run Everyday At 11:00 AM
2. Time Range: Custom -7d@d to -6d@d (07/23/2021 12:00 AM to 07/24/2021 12:00 AM)
3. Schedule Priority: Highest
4. Schedule Window: 5 Minutes
5. Trigger Actions:
   a. Output Result to lookup --> FileName: Vendor_Summary_Index.csv --> Append records after every run
   b. Send Email
   
This report will trigger at 11:00 AM CST. Hopefully my report will populate vendor data in vendor_summary_index. Fingers crossed!!!

On Friday, I didn't see data on vendor_summary_index but I could see that the job ran successfully as below.

<img width="1243" alt="image" src="https://user-images.githubusercontent.com/16859646/127785447-0cfdca0f-b441-4923-a714-09efbf356473.png">

I couldn't to able find why I am not seeing data on summary_index eventhough job ran successfully. After long research I found that I didn't enable summary indexing on the report. Now I enabled summary Indexing as below

1. Report: populate_summary_index
2. Enable Summary Indexing: check marked as yes
3. Select Indexing Type: Event
4. Select the summary index: vendor_summary_index

I am not adding fields now. Tomorrow after data pushed to summary_index I'll add adding fileds option. This approach is to understand the difference between adding fileds and without adding fileds on the summari indexing.

And also, I've noticed that the report job is not creating at exactly 11:00AM CST. So I made changes on the report schedule. I removed scheduled window as 'No window'. Hopefully this will help to create/run the job at 11:00AM CST.









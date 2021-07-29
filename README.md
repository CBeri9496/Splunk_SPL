# Splunk_SPL

Here I am writing tstats queries:

Find list of indexes count

|tstats count where index=* OR index=_* by index

The above query results the list of indexes with record counts in each index

Develop VendorID filter in the dashboard:

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

***A filter XML code starts with "Form" whereas a dashboard without filter panel XML code start with "dashboard"***

To interlink filter to the dashboard, add filter_token name in the dashboard search query. Then the dashboard will generate based on filter selections.





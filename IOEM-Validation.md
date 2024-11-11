<h1 align= center>IOEM Validation Effort</h1>
<h2>Objective</h2>
To automate the process of validating the information associated with the ~220 ticket queues that are specified in the Incident Ownership and Escalation Matrix, and the contact points for those groups. Contact points in this effort included Prime Shift contact, Secondary contact, After Hours contact, and the 1st-4th escalation contacts. 
<h2>Tools of this trade</h2>
<p>I decided to use four products/services for this effort: Power Automate, Outlook, Forms, and SharePoint (all Microsoft products).</p>
<ul>
 <li>SharePoint houses a list that contains one item for each aforementioned workload (~220 total) and provides a platform to be able to add/configure “views” in SharePoint (SP). Views are just ways to see data, a lot like filtering and sorting. Technically, the list was created by Microsoft Lists (another application), but I omitted that from this document for the purpose of simplicity, since SP shows and provides everything you need.</li>
 <li>Power Automate is the tool that carries out the actions of manipulating the data from SP, Outlook, Forms, and the list of O365 users. This manipulation allows the flow (series of actions with an end result) to send out emails to one individual for each workload, about 180 emails or so (multiple buckets are owned by the same management chain). In this effort, PA performs two flows</li>
 <li>Outlook is merely a vessel used to ship the emails that PA generates. It is controlled and manipulated by PA in our case.</li>
 <li>Forms is the tool used to aid the effort with all the workloads that were omitted from the initial rollout of emails. It was designed for more control throughout the process. It contains one question, which asks for a bucket name to send an email off for, and that kickstarts the second flow in this effort. 
</li> 
</ul>
<h2>User Flow</h2>
<p>The user will get an email notifying them of the effort, instructions/expectations and a link to their assigned item:</p>
<img src="Assets/IOEM%20Validation/user_email.jpg">
<p>The users will click on the link and will see the following editable fields:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/b54434c160d2e63a7b382e39c876aeb4d465e203/Assets/IOEM%20Validation/IOEM%20write-up11.png" alt="item_view"> 
<p>The Workload Description field provides instructions on how to proceed for the user, and what fields to fill out, and what is expected from each field based off the circumstances surrounding that workload. In a perfect world, every item will look like this:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/b92ecb3050aea09e44f27e6786dcc3be36cc62e9/Assets/IOEM%20Validation/IOEM%20write-up2.png" alt="item_view">
<p>The below image is how IMS can view the status of each item in this list to ensure things are moving forward to 100% validation:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/70c541a64b9e07f6beda274384ab21cc9c918a99/Assets/IOEM%20Validation/IOEM%20write-up3.png" alt="top_half_of_primary_flow">
<p>It displays all the information needed for IMS to keep track of the effort and allows us to reach out to any assignees who are falling behind on their task.</p> 
<h2>Process Flow</h2>
<p>The process flow for this will start with PA and all the data it needs to perform the first (and primary) flow. A list is created beforehand to house all the data. I titled mine IOEM Validation and housed it in the Incident Management SP page.</p> 
<p>All PA flows must have some sort of trigger to execute the actions that follow, so I used the ‘Manually trigger a flow’ trigger, which allows me to kick off the flow at the click of a button. The next action, ‘List rows present in a table’, grabs all the rows (and the data in those rows) from a copy of a recently updated IOEM, and stores it, allowing for retrieval of that data by any other action(s). I then initialize two variables, ‘Invalid Buckets’ and ‘Put Emails in Array’, seen below:</p>
<p align= center><img src="https://github.com/theLiberater-er/PowerPlatform/blob/5d2bb48420883a4ddc4e385f0b8ea57d49142467/Assets/IOEM%20Validation/primary_flow_1.png" alt="top_half_of_flow"></p>
<p>‘Invalid Buckets’ is an array variable that will hold the names of buckets that pass either one of the conditions coded in for the Conditional action. This variable did not have any initial value added to it during its creation. ‘Put Emails in Array’ was not set with an initial value and was designed to take all known emails with each user and put them in an array for filtering later in the flow.</p> 
<p>An ‘Apply to each’ action is automatically added to the rest of the flow. This is due to the next action (‘Get a row’) referencing all the rows in the ‘List rows present in a table’ action. This gets every row in the spreadsheet and has the data in those rows ready to go. While this action and the ‘List rows present in a table’ action seem to do the same thing, they do not. The next action, a Conditional, is an “If” statement, with the following conditions:</p> 
<ul>
<li>If the [value of the] first escalation contact is equal to “Blank”, then the flow will add the bucket name to the ‘Invalid Buckets’ variable.</li> 
<li>If the [value of the] first escalation contact exceeds 20 characters in length, then the flow will add the bucket name to the ‘Invalid Buckets’ variable.</li> 
</ul>
<p>The two actions under ‘True’ are what adds the bucket names to the ‘Invalid Buckets’ variable and displays them for me to make sure those look right. If the row passes as false in the conditional, then it will make a SP item, assign it, and send an email to the assignee notifying them of this validation effort, provides them with instructions, and a link to access the item on SP.</p> 
<p align= center><img src="https://github.com/theLiberater-er/PowerPlatform/blob/3fbca73b62eca39f5ce01adeb491d132970b79f9/Assets/IOEM%20Validation/primary_flow.png" alt="bottom_half_of_flow"></p>
<p>It is also worth noting that during the rollout, it was discovered that any assignee will need to be added to a SP group with Edit or Contribute access to this List within the IMS SP site.</p> 
<h2>SharePoint Group</h2>
<p>Navigate to the Project Management page and click See All:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/a9c870cd03c08245c30612c29b42556419c557de/Assets/IOEM%20Validation/ioem%20write-up12.png" alt="see_all">
<p>Then click the cog, List settings, then Permissions for this list:</p>   
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/ioem%20write-up13.png" alt="list_view"><br>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/IOEM%20write-up14.png" alt="sp_group_settings">
<p>In the image below, click on the IOEM Validation Group. You can also check the permissions of a specific user to aid in troubleshooting, if applicable, by selecting Check Permissions.</p> 
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/IOEM%20write-up15.png" alt="navigating_to_group">
<p>Click on New > Add Users:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/IOEM%20write-up16.png" alt="adding_users_to_group">
<p>When adding users, be sure to un-check the ‘Send an email invitation’ box under Options. This will prevent an email being sent out to each user notifying them that they have been added to this group.</p> 
<p>The last piece of the primary flow is an email that is sent to the appropriate resource with a list of all the invalid buckets:</p>
<img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/IOEM%20write-up17.png"> 
<h2>Secondary Flow</h2>
<p>Below is the flowchart used to design the second flow in this effort, which was designed to address all the buckets that errored out during the initial rollout:</p>
<p align= center><img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/secondary_flow.png" alt="secondary_flow"></p>
<p>This flow is designed to send an email to the assignee of the bucket name that is submitted via a Form:</p>
<p align= center><img src="https://github.com/theLiberater-er/PowerPlatform/blob/795c41fb8f1b022989a987cde8f7f516ee6aba60/Assets/IOEM%20Validation/additional_buckets.png" alt="additional_buckets"></p>
<p>The flow will take the bucket name, grab the assigned person for the item that has that bucket as the title, and send them an email notifying them of assignment, instructions, and a link to the item. Similar to the first flow but configured differently where an email is sent one at a time rather than all at once, like the primary flow. This allows for better error handling in this flow and better quality control.</p> 

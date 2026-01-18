# Introduction
This project explores Robotic Process Automation (RPA) with UiPath to improve productivity and sorting of invoices. 

This is with the guidance of [Kevin Stratvert](https://www.youtube.com/@KevinStratvert) on YouTube so [click here](https://www.youtube.com/watch?v=p0AfPMV3BWw) for his full tutorial!

# Background
Being a recent graduate in Singapore with interest in process improvement and productivity but has no background in automation, this project was created to kick start my hands-on knowledge in RPA with a low code application - UiPath. 

### Tools I used
- **UiPath**: The backbone for my automation.
- **Google Drive**: Selected location to store files for this project (could be changed depending on organisations). 
- **Git & Github**: Version control and sharing, ensuring collaboration and tracking. 
- **Visual Studio Code**: My go-to code editor for database management and executing queries (mainly used to create this readme here for this project).

# New Invoice Upload
1. Start by select "wait for connector event" under "action" in the implementation section. 
2. Select the chosen connector and event to trigger this automation. 
3. Select the parameters: location and filters (if applicable)
4. Add variables to pull information from the files.
5. Upload a test file to the selected location and debug to make sure this is working correctly.

# Adding an RPA Workflow
1. Add a new task and select "start and wait for an RPA workflow" in implementation. "+ Resource" to create a new RPA workflow. 
2. Open data manager on the left of the screen and add new inputs. In this case, the File URL. 
3. Add outputs to recognise what information to extract. In this case; Invoice Date, Invoice Number, Invoice Amount, and Vendor. Back to the agentic process, add the input of FileURL. 
4. To build the RPA workflow, head back to "main.xaml" and add activity. Look for location of data, in this case google drive, hence select "get file or folder" while searching for "drive" and select fileURL. 
5. Extract relevant document data by clicking new task and searching for "extract document data". 
6. *Note: Warning for document understanding is not enabled will show up if this is your first time doing this, select the waffle menu on the top lefthand corner, admin, default tenant, services, add services (top righthand corner), document understanding, and head back to the workflow with the waffle menu and studio*
7. Select the file we want to extract the data from, select file or folder in "input", predefined in "document understanding project", and invoices in "extractor". 
8. Assign the extracted values into variables (from "New Invoice Upload") by adding a new activity and searching for "assign variable value". 
9. Select output variable and corresponding extracted document data. Repeat this step for each variable. 
10. Debug in the agentic process to ensure that all values are extracted and assigned to the variables.

# Build Decision Agent 
1. "Start and wait for agent" in action. Under agent, select "+ Resource".
2.  Define the inputs and outputs for the agent on the left hand side in data manger. For inputs, add InvoiceDate, VendorName, InvoiceNumber, InvoiceAmount (Remember to make all required) and for outputs, remove "content", add Status, and add Reason.
3. Head back to process with project explorer on the left. Map all outputs from the previous step into the inputs. 
4. Configure the agent by filling in the system prompt and user prompt. 

# Test Agent with Evaluation Sets
1. In project explorer, selected "evaluation sets" under agent project.
2. Create new evaluation set with the dropdown on the top righthand corner. 
3. Select add to set and create a sample invoice for under $500. Create another set above $500. 
4. Select evaluate set to compare expected outputs to actual outputs for accuracy. 

# Branch Logic with Gateway
1. Add exclusive gateway.
2. Add task to execute connector activity. Select google drive as connector and desired activity. 
3. Configure the gateway as approved and indicate the condition with C# expression editor. Check the status from the agent by inserting the variables from the agent (vars.status) and set it to "approve".
```vars.status == "approve"``` 

5. Add task for invoices to the reviewed. 
6. For invoices to be reviewed, we would want to create an action app. Create an action app by selecting "+ Resource". Select simple approval template.
7. Edit the contents of the approval app in the agentic process with C# expression editor.

```
"Vendor Name:" + vars.vendorName + "\n" + "Invoice #:" + vars.invoiceNumber + "\n" + "Date:" + vars.invoiceDate + "\n" + "Total: $" + vars.invoiceAmount + "\n" + "Staus:" + vars.status + "\n" + "Reason: " + vars.reason
```
7. Add a gateway, and connecting it to the approved stage. Expand the conditions and label it as "approved" and input the condition with C# expression editor.
``` vars.action == "approve"```
8. Add a new task on the gateway for rejected invoices. Select execute connector activity and the selected connector. Indicate desired activity. 
9. On the gateway, label the rejection and specify the condition with C# expression editor.
```vars.action == "reject"```
11. Set up condition for gateway of approve or review. Set up label for review and condition.
```vars.status == "review"```

# Testing the Full Workflow
1. For action center, turn it on with the waffle menu admin, click on defaulttenant, services, and select actions.
2. Upload the relevant documents onto google drive.
3. Head back to studio and debug.
4. For user task to review, click on "Review" in the execution trail and "Open app task" in details. Assign to self and select approve or reject. 
5. Check if the desired activity is completed. 

# Publishing 
To enable automation, select publish on the top and this process will be automated.

# Closing Thoughts

Being my first project on automation with UiPath, I find the application to be user-friendly. I foresee this tool to be used in many working environments and find is beneficial for everyone in all organisations to pick up such automation capabilities to improve productivity. 

# PegaBalanceTransfer
Developed a Pega app that allows customers to transfer outstanding balances from other credit cards to a UPlus Bank credit card.


## Scenario
U-Plus wants to allow customers to transfer outstanding balances from other credit cards to a U-Plus credit card to encourage customers to consolidate financial services needs with U-Plus Bank. U-Plus offers several balance transfer options, with variations in up-front fees, promotional interest rates, and promotional offer length.
Customers can begin a balance transfer by calling a call center, where a customer service representative (CSR) walks the customer through the balance transfer. When the CSR begins the balance transfer process, the account number is passed to the balance transfer case to present users with the account information.
After providing an account number, customers select one of the available balance transfer offers, identify the amount to transfer from one or more credit cards, and review the terms of the balance transfer offer. The case is then routed to a CSR work queue. A CSR either approves or rejects the transfer. If the transfer is rejected, the CSR identifies the reason for the rejection. After the transfer is approved or rejected, customers receive an email about the decision.
For MLP 1 of the balance transfer project, CSRs interact with the application through the user portal channel. For MLP 2, customers can access the application directly through a mobile app and receive guidance from a web chatbot.
While you want to plan for MLP 1 and MLP 2, you are implementing the functionality described in MLP 1 only. 


### US-001: Balance transfer case life cycle
As a customer, I want to transfer balances to my U-Plus credit card so that I can save money on interest charges.

US-001 Description
Design the Balance Transfer case life cycle to reflect the process that U-Plus customers use to transfer balances to a U-Plus branded credit card. The life cycle should contain the following stages, processes, and steps.

| Stage					| Process       	| Step								| Step type					|
| --------------------- | ----------------- | --------------------------------- | ------------------------- |
| Create        		| Submit request	| Create							| Collect information		|
| 		        		| 					| Select offer						| Collect information		|
| 		        		| 					| Identify transfers				| Collect information		|
| 		        		| 					| Review transfers					| Collect information		|
| Approval				| Approve transfer	| Approve transfer					| Approve/Reject			|
| Fulfillment			| Fulfill request	| Send confirmation					| Send email				|
| Approval Rejection	| Reject request	| Identify reason for rejection		| Collect information		|
| 						| 					| Send rejection email				| Send email				|

Configure the case life cycle with the following personas, channels, and releases.

| Stage | Persona | Channel | Release |
| ----- | ------- | ------- | ------- |
|Create | CSR User | Portal | MLP 1 |
|Create | Customer | User Mobile App | MLP 2 |
|Create | User | Digital Messenger | MLP 2 |
|Approval | CSR User | Portal | MLP 1 |
|Fulfillment |  |  |  |
|Approval Rejection | CSR User | Portal | MLP 1 |



Configure the case life cycle with the following data objects, systems of record (SOR), and releases.

| Stage | Data object | SOR | Release |
| ----- | ----------- | --- | ------- |
|Create | Account | PEGA | MLP 1 |
|Create | Transfer offers | PEGA | MLP 1 |
|Approval |  |  |  |
|Fulfillment |  |  |  |
|Approval Rejection |  |  |  |

Configure the case type to change the status of the case at each stage.

| Stage | Case status on stage entry | Resolution status |
| ----- | -------------------------- | ----------------- |
|Create | Open |  |
|Approval | Pending-Approval |  |
|Fulfillment | Pending-Fulfillment | Resolved-Completed |
|Approval Rejection |  | Resolved-Rejected |

Configure routing to the AcctMgmt:CSR work queue for the Approve transfer step.

Configure a service-level agreement for the Approve transfer step.

|  		| Interval | Urgency | Action |
| ----- | -------- | ------- | ------ |
| Goal | 1 hour | 20 | Notify Assignee by using the default message |
| Deadline | 2 hour | 40 | Notify Assignee by using the default message |



### US-001 Acceptance criteria
The balance transfer case life cycle has a Create stage, which has one process containing a multi-step form with four steps.
The balance transfer case life cycle has an Approval stage, which has one process and one step.
The balance transfer case life cycle has a Fulfillment stage, which has one process and one step.
The balance transfer case life cycle has an Approval Rejection stage, which has one process and two steps.
The Create stage has the CSR persona with the User portal channel tagged with MLP 1.
The Create stage has the Customer persona with a Mobile channel named User Mobile App and a Digital messaging channel named User Digital Messenger tagged with MLP 2.
The Create stage has the Account and Transfer offers data objects tagged with MLP 1.
The Approval and Approval Rejection stages have the CSR persona with the User portal channel tagged with MLP 1.
A CSR accesses the case from a work queue to perform the Approve transfer step.
The case status updates at the start of the Create, Approval, and Fulfillment stages and resolves at the end of the Fulfillment and Approval Rejection stages.
The Approve transfer step has a completion goal and a completion deadline of one and two hours, respectively, increasing the urgency and notifying the assignee by using a default message.


### US-002: Create view
As a customer, I want to select one of my accounts so that I can transfer balances with as few errors as possible.

### US-002 Description
Configure the Create step with a data reference field that is a single record and is sourced from the preconfigured Account data object. The view should display the following fields, which are already defined for the data object:

- Credit limit
- Current balance
- First name
- Last name
Configure the data reference field as a Search box that searches the account number field.

### US-002 Acceptance criteria
The user can select an account number from a search box in the Create view.
When an account number is selected, the Create view displays four fields: Credit limit, Current balance, First name, and Last name.


### US-003: Select Offer view
As a customer, I want to review all the available balance transfer offers so that I can select the one that best matches my needs.

### US-003 Description
Configure the draft data object named Transfer offers to add the following fields:

| Field name | Field type |
| ---------- | ---------- |
| Title | Text (single line) |
| Description | Text (paragraph) |
| Interest rate | Percentage |
| Transfer fee | Percentage |
| Minimum payment rate | Percentage |
| APR after promotional period | Percentage |
| Length of offer | Text (single line) |

Using the values in the following table, add three records to the data object to identify the offers that are available to customers:
| Title					| Description | Interest rate | Transfer fee | Minimum payment rate | APR after promotional period | Length of offer |
| --------------------- | ----------- | ------------- | ------------ |--------------------- | ---------------------------- | --------------- |
| Low interest rate transfer | Low interest rate on transferred balances for twelve billing cycles. | .03 | .04 | .02 | .13 | 12 billing cycles |
| No fee transfer | Balance transfer with no transfer fee. Promotional interest rate is applied for nine billing cycles following the transfer. | .06 | 0 | .02 | .15 | 9 billing cycles |
| Zero interest transfer | No interest on transferred balances for six billing cycles. | 0 | .04 | .03 | .16 | 6 billing cycles |


Configure the Select offer view with a single record, data reference field named Transfer offers. Configure the data reference field to source data from the Transfer offers data object and display the names of available offers in a drop-down list. When the user selects an offer by Title, the view displays the following fields:

- Description
- Interest rate
- Transfer fee
- Minimum payment rate
- APR after promotional period
- Length of offer

### US-003 Acceptance criteria
The Transfer offers data object has fields for Title, Description, Interest rate, Transfer fee, Minimum payment rate, APR after promotional period, and Length of offer.
The Transfer offers data object has three records for Low interest rate transfer, No fee transfer, and Zero interest transfer.
The user can select an offer by Title from a drop-down list in the Select offer view.
When an offer is selected, the Select offer view displays six fields: Description, Interest rate, Transfer fee, Minimum payment rate, APR after promotional period, and Length of offer.



### US-004: Identify Transfers view
As a customer, I want to identify the balances that I want to transfer from other accounts so that I can reduce the amount of interest I pay on outstanding debts.

### US-004 Description
Configure the Identify transfers view with a list of records, Embedded data field named Transfers. Include the following fields in the Transfers data object to represent transfers authorized by customers.

| Field name | Field type | Options		|
| ---------- | ---------- | ----------- |
| Account number | Text (single line) | Required |
| Amount | Currency | Required |
| Lender | Text (single line) | Required |


### US-004 Acceptance criteria
The Transfers Embedded data field has three required fields: Account number, Amount, and Lender.
The user can add and delete transfers in the Identify transfers view.



### US-005: Review Transfers view
As a customer, I want to review my transfers so that I authorize transfers in the correct amount to the correct lender.

### US-005 Description
Configure the Review transfers view to display a list of transfers authorized by the customer. Customers may not update the details of each transfer.

Configure the Review transfers view with four calculated currency fields.

| Field name | Expression |
| ---------- | ---------- |
| Total transfer amount | Sum of the amounts in the Transfers data fields |
| Transfer fee charged | Total transfer amount * Transfer fee |
| Total cost of transfer | Transfer fee charged + Total transfer amount |
| Minimum payment on transferred amounts | Total cost of transfer * Minimum payment rate |

### US-005 Acceptance criteria
The Review transfers view displays a read-only list of authorized transfers.
The Review transfers view displays four calculated currency fields: Total transfer amount, Transfer fee charged, Total cost of transfer, and Minimum payment on transferred amounts.
The Total transfer amount field displays the sum of the Amount values added by the user.
The Transfer fee charged field displays the value of the Total transfer amount multiplied by the Transfer fee.
The Total cost of transfer field displays the sum of the Transfer fee charged and Total transfer amount.
The Minimum payment on transferred amounts field displays the value of Total cost of transfer multiplied by the Minimum payment rate.


### US-006: Approval view
As a CSR, I want to review the transfer and customer account information so that I can ensure that the requested transfers comply with lending policies at U-Plus.

### US-006 Description
Configure the view for the Approve transfer step to display the fields listed in the following table, organized into the three views identified. Entries without a listed field type are reused fields that are already defined in the data model.

*Tip:* Create a new Form view for each set of details, and then add the views to the main view for the step.
| View 		 | Field name | Field type | Additional configurations |
| ---------- | ---------- | ---------- | ------------------------- |
| Account details | Account number |  |  |
| Account details | Credit limit |  |  |
| Account details | Current balance |  |  |
| Account details | Available credit |  |  |
| Account details | Maximum transfer allowed |  |  |
| Transfer details | Total cost of transfer | Currency | Calculated as maximum transfer allowed - total cost of transfer |
| Transfer details | Remaining transfer balance available | Currency | Calculated as maximum transfer allowed - total cost of transfer |
| Transfer details | Remaining available credit | Currency | Calculated as available credit - total cost of transfer |
| Transfer details | Credit utilization | Percentage | Calculated as (current balance + total cost of transfer) / credit limit |
| Approval details | Credit score | Integer | Required |
| Approval details | Notes | Text (paragraph) | Optional |


### US-006 Acceptance criteria
The Approve transfer view displays eleven fields, separated into three groupings: Account details, Transfer details, and Approval details.
The Account details grouping contains five fields: Account number, Credit limit, Current balance, Available credit, and Maximum transfer allowed.
The Transfer details grouping contains four calculated fields: Total cost of transfer, Remaining transfer balance available, Remaining available credit, and Credit utilization.
The Remaining transfer balance available currency field displays the value of the Maximum transfer allowed minus the Total cost of transfer.
The Remaining available credit currency field displays the value of the Available credit minus the Total cost of transfer.
The Credit utilization percentage field displays the sum of the Current balance and the Total cost of transfer, divided by the Credit limit.
The Approval details grouping contains two editable fields: Credit score and Notes.


### US-007: Approval email
As a customer, I want to receive a notification when my requested transfer is approved so that I can manage payments to the specified accounts and keep those accounts in good standing.

### US-007 Description
Configure the case type to create the Owner participant automatically when the case begins.

When a requested transfer is approved, send an email to the case owner with the subject line Balance transfer approved.

Message content
Dear [first name] [last name], 

We have approved your balance transfer request.

Balance transfer amount: [total cost of transfer] 
Selected offer: [title] 
Please note the following terms for your balance transfer: 

Interest rate during the promotional period: [interest rate] 
Duration of promotional period: [length of offer] 
Interest rate on remaining balance after your promotional rate expires: [APR after promotional period] 
Please review your account terms and conditions for important information about how we apply payments if you maintain a balance due to other card activity.

Thank you for banking with U-Plus.

*Note:* Items in square brackets are property references. You can click the Insert property icon to select a property from a list. If the Insert property icon is not visible, log out and log back in.

### US-007 Acceptance criteria
The case type automatically creates the Owner participant when the case begins.
When a requested transfer is approved, the case owner receives an email with the subject line Balance transfer approved.
The approval email message contains the customer's full name, the total transfer cost, the selected offer, the interest rate, the offer length, and the APR after the promotional period.



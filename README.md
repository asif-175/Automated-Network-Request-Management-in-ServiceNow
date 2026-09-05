# Automated Network Request Management in ServiceNow

## 📌 Project Overview

**Automated Network Request Management in ServiceNow** is an IT service automation project developed to streamline the lifecycle of network-related service requests.

The system allows users to submit network requests through the ServiceNow Service Catalog. The submitted request is processed through automated workflow logic using Flow Designer, including data collection, backend record creation, approval processing, notifications, and record updates.

The project aims to reduce manual intervention, improve request visibility, standardize network request processing, and maintain an auditable request lifecycle.

---

## 🎯 Problem Statement

Traditional network request handling can involve manual request submission, approval coordination, communication between requesters and IT teams, and manual status updates.

This can result in:

- Delayed request processing
- Manual approval tracking
- Inconsistent request information
- Limited visibility into request status
- Increased administrative effort
- Difficulty maintaining an audit trail

This project addresses these challenges by implementing an automated network request management process in ServiceNow.

---

## 🎯 Project Objectives

The main objectives of this project are:

- Create a standardized Network Request catalog item.
- Capture required network and requester information.
- Provide a structured request submission form.
- Dynamically control fields based on user selections.
- Automate request processing using Flow Designer.
- Implement approval processing.
- Automatically create backend request records.
- Update backend records based on workflow outcomes.
- Send notifications during important request stages.
- Support network team request processing.
- Improve request visibility and auditability.

---

## 🏗️ Project Workflow

The implemented request processing workflow is:

```text
Requester
    ↓
Service Catalog
    ↓
Network Request
    ↓
Catalog Variables
    ↓
Flow Designer
    ↓
Get Catalog Variables
    ↓
Create Database Tables Record
    ↓
Send Email
    ↓
Ask for Approval
    ↓
Approval Decision
    ↓
 ┌─────────────────┐
 │                 │
Approved         Rejected
 │                 │
 ↓                 ↓
Update Record   Update Record
 │
 ↓
Request Processing
 │
 ↓
Request Completion
```

---

## 🛠️ Technology Stack

| Technology / ServiceNow Feature | Purpose |
|---|---|
| ServiceNow | IT Service Management platform |
| Service Catalog | Network request submission |
| Catalog Item | Provides the Network Request form |
| Catalog Variables | Captures request information |
| Variable Sets | Provides reusable requester information |
| UI Policies | Controls dynamic form behavior |
| Custom Table | Stores network request information |
| Flow Designer | Automates request processing |
| Approval Engine | Handles request approval |
| Email Notifications | Communicates important request updates |

---

# 🔧 ServiceNow Configuration

## 1. Custom Database Table

A custom table named **Database Tables** was configured to store network request information generated through the automated process.

### Database Tables

| Property | Value |
|---|---|
| Table Label | Database Tables |
| Purpose | Stores network request information |
| Auto Number Prefix | ANR |
| Number of Digits | 7 |

The auto-number configuration generates a unique identifier for each request.

### Example Request Numbers

```text
ANR0001000
ANR0001001
ANR0001002
```

The generated request number provides a unique reference for identifying and tracking each network request record.

---

## 2. Database Table Fields

The **Database Tables** record contains the information required to manage the network request.

The configured fields cover information related to:

- Request identification
- Requester
- Contact information
- Network request details
- Address
- Device information
- Connection type
- Assignment
- Approval status
- Work or processing status

The Database Tables record acts as the backend record associated with the request submitted through the Service Catalog.

---

## 3. Approval Request Related List

An **Approval Request** related list was configured on the Database Tables form.

This allows approval information associated with a network request to be viewed from the corresponding database record.

Approval information can include:

- Approval state
- Approver
- Approval outcome

The related list provides visibility into the approval stage of the request and supports request tracking.

---

# 🛒 Service Catalog

## Network Request Catalog Item

A Service Catalog item named **Network Request** was created to allow users to submit network-related requests.

### Catalog Item Configuration

| Property | Value |
|---|---|
| Name | Network Request |
| Catalog | Service Catalog |
| Category | Network |
| Short Description | Network request Management |

The catalog item provides a structured interface for collecting the information required to process a network request.

---

# 📝 Catalog Variables

The Network Request catalog item contains variables for collecting request and network-related information.

| Variable | Type | Purpose |
|---|---|---|
| Connection Type | Choice | Captures the type of connection request |
| Relocated Address | Single Line Text | Captures relocation address |
| Device Type | Choice | Captures device category |
| Address | Single Line Text | Captures address |
| Device Details | Single Line Text | Captures device information |
| Other Details | Single Line Text | Captures additional information |
| Requested For | Reference | Identifies the requested user |

### Device Type Choices

The configured Device Type choices include:

- Laptop
- Mobiles
- Others

The catalog variables ensure that the required request information is collected in a standardized format before the request enters the automated workflow.

---

# 👤 Variable Set

A reusable variable set named **Network Requester Information** was configured for the Network Request catalog item.

The variable set contains requester-related information.

| Variable | Type | Purpose |
|---|---|---|
| Opened on behalf of | Reference | Selects the user |
| Email ID | Single Line Text | Stores user email |
| User Name | Single Line Text | Stores user name |
| Phone Number | Single Line Text | Stores user phone number |
| Proof of Document | Attachment | Allows supporting document upload |

The requester information variables can be populated based on the selected ServiceNow user record.

The auto-population relationship can be represented as:

```text
Opened on behalf of
        ↓
ServiceNow User Record
        ↓
 ┌──────┼─────────┐
 ↓      ↓         ↓
Email  Name      Phone
```

This reduces manual data entry and improves the consistency of requester information.

---

# ⚙️ Dynamic Form Behavior

UI Policies were configured to provide dynamic behavior on the Network Request catalog form.

The UI Policies are used to:

- Dynamically show or hide fields.
- Control whether specific fields are mandatory.
- Improve the user experience.
- Prevent unnecessary fields from being displayed.
- Ensure that users provide relevant information based on their selections.

When the configured condition is satisfied, the corresponding field can be displayed and made mandatory.

When the condition is not satisfied, the field can remain hidden and does not require user input.

---

# 🔄 Flow Designer Automation

The core automation of the project is implemented using **ServiceNow Flow Designer**.

### Flow Name

**Network Request**

### Trigger

The flow is triggered when the **Network Request** catalog item is submitted.

The submitted catalog request provides the input for the automated workflow.

---

## Flow Steps

The main flow consists of the following activities:

### 1. Service Catalog Trigger

The flow starts when a user submits the Network Request catalog item.

The catalog submission acts as the starting point for the automated request processing.

---

### 2. Get Catalog Variables

The **Get Catalog Variables** action retrieves the variables submitted through the Network Request catalog item.

The retrieved values can be used as data pills in subsequent Flow Designer actions.

The configured variables include:

- Connection Type
- Relocated Address
- Device Type
- Address
- Device Details
- Other Details
- Requested For
- Opened on behalf of
- Email ID
- User Name
- Phone Number
- Proof of Document

This action connects the user-facing catalog form with the backend automation process.

---

### 3. Create Database Tables Record

The **Create Record** action creates the corresponding record in the **Database Tables** table.

The information collected through the catalog item is mapped to the appropriate fields of the backend record.

The process can be represented as:

```text
Network Request Catalog Item
            ↓
    Get Catalog Variables
            ↓
       Create Record
            ↓
      Database Tables
```

This creates a backend record corresponding to the submitted network request.

---

### 4. Send Email

The **Send Email** action is used to communicate important request information to relevant users during the workflow.

Email communication is used for important request stages such as:

- Request processing
- Approval requirement
- Approval outcome
- Other configured request updates

This helps keep requesters and responsible team members informed about the progress of the request.

---

### 5. Ask for Approval

The **Ask for Approval** action handles the approval stage of the workflow.

The approval process is performed on the corresponding Database Tables record.

The approval result is then used by the flow's decision logic.

The possible approval outcomes include:

- Approved
- Rejected

---

### 6. Approval Decision

The flow evaluates the result returned by the approval process.

```text
Ask for Approval
       ↓
   ┌───┴────┐
   ↓        ↓
Approved  Rejected
   ↓        ↓
Update    Update
Record    Record
```

The approval result determines the appropriate path for updating the backend record.

---

### 7. Update Database Tables Record

The **Update Record** action updates the Database Tables record according to the approval outcome.

For an approved request, the record is updated to reflect the approval and continue through the configured request processing.

For a rejected request, the record is updated to reflect the rejection.

This ensures that the backend record represents the current outcome of the request.

---

# 👥 User and Group Management

The project uses ServiceNow users and groups to support request processing.

The main participants in the process are:

- Requesters
- Approvers
- Network Team
- IT Administrators

A **Network Team** group was configured to support network request processing.

Reference fields such as **Assignment Group** and **Assigned To** use valid ServiceNow group and user records.

This allows network requests to be associated with the appropriate team or assigned user.

---

# 🔐 Roles and Access

The project follows a role-based approach for different participants involved in network request processing.

| User Type | Responsibility |
|---|---|
| Requester | Submit network requests |
| Approver | Review and approve or reject requests |
| Network Team | Process network requests |
| Administrator | Configure and monitor the system |

This separation of responsibilities helps maintain an organized request management process.

---

# 📧 Notifications

Email notifications are used to communicate important information during the request lifecycle.

The notification flow can be represented as:

```text
Request Submitted
       ↓
Approval Required
       ↓
Approved / Rejected
       ↓
Request Processing
       ↓
Request Completed
```

Notifications help reduce manual follow-up and improve visibility into request progress.

---

# 📊 Request Lifecycle

The request progresses through different stages during its lifecycle.

The general request lifecycle is:

```text
New
 ↓
Awaiting Approval
 ↓
In Progress
 ↓
Completed
```

If the approval request is rejected, the request follows the rejection path:

```text
New
 ↓
Awaiting Approval
 ↓
Rejected
```

The request status allows administrators and relevant team members to track the progress and outcome of each request.

---

# 🧪 Testing

The system was tested by submitting network requests and verifying the behavior of the configured catalog item, dynamic form behavior, workflow, approval process, backend record creation, and notifications.

### Test Cases

| Test ID | Test Scenario | Expected Result |
|---|---|---|
| TC01 | Submit a valid Network Request | Request is submitted successfully |
| TC02 | Select different connection options | Relevant fields behave correctly |
| TC03 | Select a requester | User information is populated |
| TC04 | Submit request for approval | Approval process is initiated |
| TC05 | Approve a request | Database record is updated |
| TC06 | Reject a request | Database record reflects rejection |
| TC07 | Verify backend record | Corresponding Database Tables record is created |
| TC08 | Verify notification | Configured email is generated |
| TC09 | Verify request status | Status reflects the current processing stage |
| TC10 | Verify assignment | Request is associated with the appropriate team or user |

The detailed test results and supporting screenshots are included in the project documentation.

---

# 📸 Screenshots

Screenshots demonstrating the implementation are organized in the `Screenshots` directory.

The screenshots cover important parts of the project, including:

- ServiceNow project setup
- Database Tables configuration
- Database table fields
- Auto-number configuration
- Approval Request related list
- Network Request catalog item
- Catalog variables
- Variable Set
- Auto-populated requester information
- UI Policies
- Flow Designer
- Get Catalog Variables
- Create Record
- Send Email
- Ask for Approval
- Update Record
- Network Request submission
- Generated request number
- Approval results
- Backend Database Tables record
- Testing results

---

# 📁 Repository Structure

```text
automated-network-request-management-servicenow/
│
├── README.md
│
├── Documentation/
│   └── Project_Documentation.pdf
│
├── Screenshots/
│   ├── 01_Project_Setup/
│   ├── 02_Database_Tables/
│   ├── 03_Catalog_Item/
│   ├── 04_Variables/
│   ├── 05_Variable_Set/
│   ├── 06_UI_Policies/
│   ├── 07_Flow_Designer/
│   ├── 08_Approvals/
│   ├── 09_Notifications/
│   └── 10_Testing/
│
├── Configuration/
│   ├── Tables/
│   ├── Catalog/
│   ├── Flows/
│   └── Security/
│
└── Test_Data/
    └── test_cases.md
```

---

# 💡 Key Benefits

The implemented solution provides the following benefits:

- Reduced manual intervention
- Standardized network request submission
- Automated request processing
- Improved approval management
- Better request visibility
- Consistent data collection
- Faster communication
- Improved request tracking
- Better auditability
- Maintainable workflow automation

---

# 🚧 Challenges Faced

During the implementation of the project, several ServiceNow configuration and workflow challenges were encountered.

### 1. Reference Field Configuration

Fields such as **Assignment Group** and **Assigned To** are reference fields. Therefore, valid ServiceNow group and user records must be selected instead of entering arbitrary text.

### 2. Auto-Population

Requester information required configuring dependent variables and reference-field dot-walking so that information such as name, email, and phone could be retrieved from the selected ServiceNow user record.

### 3. Dynamic Field Visibility

UI Policies were configured to dynamically control the visibility and mandatory behavior of catalog variables based on user selections.

### 4. Flow Designer Record References

Reference fields used within Flow Designer require appropriate ServiceNow record references. Lookup logic can be used when a specific group or record needs to be selected during the flow configuration.

### 5. Flow Execution and Testing

The Flow Designer workflow was tested to verify that catalog submission triggered the configured automation, created the backend Database Tables record, processed approval, and updated the record according to the approval outcome.

---

# 📚 Learning Outcomes

This project provided practical hands-on experience with:

- ServiceNow Administration
- ServiceNow Development
- Service Catalog
- Catalog Items
- Catalog Variables
- Variable Sets
- Reference Fields
- Custom Tables
- Auto Number Configuration
- UI Policies
- Flow Designer
- Approval Workflows
- Email Notifications
- User and Group Management
- Request Lifecycle Management
- Workflow Automation
- Testing and Troubleshooting
- ServiceNow Form Configuration

---

# 🚀 Future Enhancements

The project can be further enhanced with:

- SLA-based request tracking
- Advanced approval routing
- Automated fulfillment task creation
- Network configuration integrations
- ServiceNow reports and dashboards
- Advanced role-based access control
- Integration with external network management systems
- Automated network configuration verification
- Request analytics and performance monitoring
- Enhanced audit and compliance reporting

---

# 🎓 Project Context

This project was developed as part of the **SmartBridge ServiceNow Hands-on Project**.

The project demonstrates practical implementation of ServiceNow workflow automation by combining Service Catalog, Catalog Variables, Variable Sets, UI Policies, custom tables, approvals, email notifications, and Flow Designer.

The implementation focuses on automating the network request lifecycle from request submission through backend record creation, approval processing, request updates, and completion.

---

# 👨‍💻 Project Information

| Detail | Information |
|---|---|
| Project Title | Automated Network Request Management in ServiceNow |
| Platform | ServiceNow |
| Automation Tool | Flow Designer |
| Service Catalog Item | Network Request |
| Backend Table | Database Tables |
| Auto Number Prefix | ANR |
| Auto Number Digits | 7 |
| Project Type | ServiceNow IT Service Management Automation |
| Training / Program | SmartBridge ServiceNow Hands-on Project |
| Status | Completed |

---

# 📄 Documentation

Detailed project documentation containing implementation steps, configuration details, screenshots, testing procedures, challenges, and learning outcomes is included in the `Documentation` directory.

The documentation provides a detailed explanation of the project implementation and configuration.

---

# 📌 Conclusion

The **Automated Network Request Management in ServiceNow** project demonstrates how ServiceNow can be used to automate and standardize network request processing.

By combining Service Catalog, Catalog Variables, Variable Sets, dynamic form behavior, custom Database Tables, approval processing, email notifications, and Flow Designer automation, the solution provides a structured approach for managing network requests.

The automated workflow reduces manual intervention, improves request visibility, supports approval management, and provides a consistent request processing lifecycle.

The project also demonstrates practical skills in ServiceNow administration, development, workflow automation, configuration, testing, troubleshooting, and IT service management.

---

# ⭐ Project Status

**Completed ✅**

The **Automated Network Request Management in ServiceNow** project has been implemented and tested in a ServiceNow development environment.

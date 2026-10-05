# EventForce Management System – Salesforce

## 📌 Project Overview

**EventForce Management System** is a Salesforce-based CRM solution designed to digitize and automate event management operations for **Event Planner Solutions**.

The system provides a centralized platform for managing:

* Events
* Clients
* Vendors
* Venues
* Feedback
* Event–Vendor assignments
* Event cancellation approvals
* Automated client reminders
* Venue availability
* Reports and dashboards
* User access and security

The solution combines **Salesforce declarative automation, Apex programming, security configuration, reports, and dashboards** to create a complete event-management CRM platform.

---

## 🎯 Objectives

The primary objectives of the EventForce Management System are:

* Centralize event management data in Salesforce.
* Manage client bookings and event scheduling efficiently.
* Coordinate vendors and event services.
* Manage venue reservations and availability.
* Automate event reminders and cancellation workflows.
* Prevent duplicate venue bookings.
* Collect and manage client feedback.
* Improve data accuracy using validation rules and formula fields.
* Provide role-based access and data security.
* Generate reports and dashboards for business insights.
* Reduce manual processes and improve operational efficiency.

### Expected Business Benefits

* **60% reduction** in manual processes.
* **40% improvement** in booking efficiency.
* Improved client communication and satisfaction.
* Better collaboration between event coordinators, vendors, and clients.
* Data-driven decision making through reports and dashboards.

---

# 🏗️ Salesforce Architecture

## Custom Objects

The system uses the following custom objects:

| Object           | Purpose                                         |
| ---------------- | ----------------------------------------------- |
| `Event__c`       | Stores event information and scheduling details |
| `Client__c`      | Stores client information                       |
| `Vendor__c`      | Stores vendor and service information           |
| `Venue__c`       | Stores venue information and availability       |
| `Feedback__c`    | Stores client feedback and ratings              |
| `EventVendor__c` | Junction object connecting Events and Vendors   |

---

## 🔗 Relationships

| Relationship         | Type          |
| -------------------- | ------------- |
| Event → Client       | Lookup        |
| Event → Venue        | Lookup        |
| Feedback → Event     | Lookup        |
| Feedback → Client    | Lookup        |
| EventVendor → Event  | Master-Detail |
| EventVendor → Vendor | Master-Detail |

The `EventVendor__c` junction object implements the **Many-to-Many relationship between Events and Vendors**.

```text
                 ┌──────────────┐
                 │    Client    │
                 └──────┬───────┘
                        │
                      Lookup
                        │
                 ┌──────▼───────┐
                 │     Event    │
                 └───┬──────┬───┘
                     │      │
                  Lookup   Master
                     │      │
              ┌──────▼─┐  ┌─▼─────────────┐
              │  Venue │  │ Event Vendor   │
              └────────┘  └──────┬─────────┘
                                  │
                               Master
                                  │
                           ┌──────▼──────┐
                           │   Vendor    │
                           └─────────────┘

                 ┌──────────────┐
                 │   Feedback   │
                 └──────┬───────┘
                        │
                Lookup to Event
                        │
                     Event
```

---

# 🚀 Project Phases

## Phase 1 – Requirement Analysis & Planning

The first phase focused on identifying business requirements and designing the Salesforce solution.

### Key Requirements

* Custom Salesforce objects
* Relationship modeling
* Event scheduling
* Client management
* Vendor coordination
* Venue management
* Feedback collection
* Automated reminders
* Cancellation approval process
* Validation rules
* Formula fields
* Apex automation
* Role-based security
* Reports and dashboards

### Stakeholders

* **Clients** – Request event services and provide feedback.
* **Event Coordinators** – Manage events, clients, and approvals.
* **Vendors** – Provide event-related services.
* **Venue Managers** – Manage venue availability.
* **Event Administrators** – Manage security and system operations.

---

# ⚙️ Phase 2 – Backend Development & Configuration

This phase implemented the core Salesforce functionality.

### Technologies and Features

* Salesforce Custom Objects
* Custom Fields
* Lookup Relationships
* Master-Detail Relationships
* Junction Objects
* Validation Rules
* Formula Fields
* Approval Processes
* Record-Triggered Flows
* Apex Classes
* Apex Triggers
* Batch Apex
* Scheduled Apex
* Profiles
* Roles
* Permission Sets
* Sharing Rules

---

## 📦 Event Object

### Main Fields

* Event Name
* Event Date
* Event Type
* Event Status
* Event Budget
* Client
* Venue

### Event Types

* Wedding
* Corporate
* Birthday
* Anniversary
* Festival
* Concert
* Other

### Event Status

* Planned
* Confirmed
* Completed
* Pending Cancellation
* Canceled
* Rejected

### Event Budget Formula

The event budget is automatically calculated based on the event type.

| Event Type  |  Budget |
| ----------- | ------: |
| Wedding     | ₹50,000 |
| Corporate   | ₹30,000 |
| Birthday    | ₹10,000 |
| Anniversary | ₹20,000 |
| Festival    | ₹60,000 |
| Concert     | ₹40,000 |
| Other       | ₹15,000 |

---

# 👥 Client Management

The `Client__c` object stores:

* Client Name
* Email
* Phone
* Address
* Country
* City

A validation rule ensures that clients enter a valid email address.

### Validation Rule

**Rule:** `Email_Valid_Address`

**Purpose:** Prevent invalid email addresses from being saved.

---

# 🤝 Vendor Management

The `Vendor__c` object manages event service providers.

### Vendor Information

* Vendor Name
* Email
* Phone
* Service Type
* Status

### Vendor Services

* Catering
* Decor
* Photography
* Videography
* Lighting
* Stage Setup
* Makeup Artist
* DJ/Music
* Transportation
* Hosting/Anchor

### Vendor Status

* Available
* Booked
* Cancelled

---

# 🏢 Venue Management

The `Venue__c` object stores:

* Venue Name
* Address
* Location URL
* Capacity
* Availability Status

### Availability Status

* Available
* Reserved
* Not Available

---

# ⭐ Feedback Management

The `Feedback__c` object collects client feedback.

### Fields

* Feedback ID
* Rating
* Comments
* Event
* Client

### Rating

Ratings are provided from **1 to 5**.

---

# 🔄 Event–Vendor Many-to-Many Relationship

Salesforce does not directly support a Many-to-Many relationship between two objects.

Therefore, the project uses:

**EventVendor__c – Junction Object**

```text
Event
  │
  │ Master-Detail
  ▼
EventVendor
  ▲
  │ Master-Detail
  │
Vendor
```

This allows one event to have multiple vendors and one vendor to participate in multiple events.

---

# 🔍 Lookup Filter

A lookup filter was implemented for Feedback.

### Scenario

When a client is selected while creating Feedback, the Event lookup only displays events belonging to that selected client.

For example:

```text
Client: Ramesh

Available Events:
✓ Ramesh Wedding
✓ Ramesh Birthday

Other Client Events:
✗ Not displayed
```

This improves data accuracy and user experience.

---

# ✅ Validation Rules

Validation rules prevent incorrect data from being saved.

### Client Email Validation

**Rule Name:**

```text
Email_Valid_Address
```

**Purpose:**

```text
Validate the client's email address before saving.
```

If an invalid email is entered, the system displays:

```text
Please Enter Valid Email Address
```

---

# 📩 Approval Process

## Event Cancellation Approval

An approval process was created to handle event cancellation requests.

### Process

```text
Event Status
     ↓
Pending Cancellation
     ↓
Approval Request
     ↓
Manager Review
     ↓
 ┌───────────────┐
 │               │
Approved       Rejected
 │               │
 ▼               ▼
Canceled       Rejected
```

### Approval Features

* Email notification to Event Coordinator
* Manager approval
* Event status update
* Client notification
* Event owner notification
* Rejection notification

### Email Templates

* Event Cancellation Request Notification
* Email to Client After Cancellation
* Approval Notification to Event Owner
* Rejection Notification to Event Owner

---

# 🔔 Automated 3-Day Event Reminder

A **Record-Triggered Flow** was created to automatically remind clients three days before a confirmed event.

### Flow

```text
Event Created / Updated
          ↓
Status = Confirmed
          ↓
Event Date
          ↓
3 Days Before Event
          ↓
Send Email
          ↓
Client + Event Owner
```

### Flow Name

```text
Client Reminder - 3 Days Before
```

The email contains:

* Event Name
* Event Date
* Venue
* Event Type
* Reminder message

---

# 💻 Apex Development

## Venue Status Automation

### Apex Class

```text
VenueStatusHelper
```

### Trigger

```text
EventTrigger13
```

### Function

When an event becomes **Confirmed**, the related venue is marked:

```text
Reserved
```

When an event becomes **Canceled**, the venue becomes:

```text
Available
```

### Logic

```text
Event Confirmed
      ↓
Venue = Reserved

Event Canceled
      ↓
Venue = Available
```

---

# 🚫 Prevent Double Booking

An Apex Trigger prevents two events from being scheduled at the same venue on the same date.

### Trigger

```text
PreventDoubleBooking
```

### Example

```text
Venue: Grand Hall
Date: 15-Nov-2026

Event 1 → Allowed

Event 2 → Same Venue + Same Date
       ↓
Booking Prevented
       ↓
"This Venue is already booked on this date."
```

This protects the system from duplicate venue reservations.

---

# ⏰ Batch & Scheduled Apex

The project uses asynchronous Apex to automatically complete past events.

## Batch Class

```text
BatchCompleteEvents
```

### Purpose

Find events where:

```text
Event Date < TODAY
```

and the status is not already:

```text
Completed
```

Then update the status to:

```text
Completed
```

---

## Scheduled Class

```text
ScheduleCompleteEvents
```

The scheduled class executes the batch process automatically.

### Schedule

```text
Daily Event Completion
Time: 08:00 PM
```

This provides automated daily event-status maintenance.

---

# 🎨 Phase 3 – UI/UX Development & Customization

A custom Salesforce Lightning application was created to provide a centralized workspace.

## Lightning App

### App Name

```text
Event Planner
```

### Navigation Items

* Events
* Clients
* Vendors
* Venues
* Feedback
* Reports
* Dashboards

The Lightning App provides a single interface for managing EventForce operations.

---

# 📊 Reports

A dedicated report folder was created:

```text
EventForce Report
```

## Report: Upcoming Events by Month

The report provides:

* Upcoming events
* Event dates
* Event types
* Clients
* Venues
* Event budgets
* Monthly event counts
* Monthly budget totals

### Filters

```text
Event Date >= TODAY
Event Status != Canceled
```

Events are grouped by:

```text
Calendar Month
```

---

# 📈 Dashboard

## EventForce Operations Dashboard

The dashboard provides a visual summary of upcoming event operations.

### Dashboard Component

```text
Upcoming Events by Month
```

### Visualization

```text
Donut Chart
```

The dashboard helps administrators and coordinators quickly understand upcoming event activity.

---

# 🔐 Phase 4 – Data Migration, Testing & Security

This phase focused on data loading, user management, security, and testing.

## Data Migration

Sample records were prepared and imported using the Salesforce **Data Import Wizard**.

Data included:

* Events
* Clients
* Vendors
* Venues
* Feedback

Data validation was performed before importing records to reduce duplicates and maintain data consistency.

---

# 👤 Profiles

The following custom profiles were created:

| Profile           | Purpose                                           |
| ----------------- | ------------------------------------------------- |
| Event Admin       | Full system administration                        |
| Event Coordinator | Manage events and client-related operations       |
| Vendor Manager    | Manage vendors                                    |
| Client Profile    | Access own event information and provide feedback |

---

# 🌳 Role Hierarchy

The role hierarchy was designed as:

```text
CEO
 │
 └── Event Admin
       │
       ├── Event Coordinator
       │
       ├── Vendor Manager
       │
       └── Client
```

Roles help control record visibility and reporting hierarchy.

---

# 🔑 Permission Set

## Feedback Manager

A permission set named:

```text
Feedback Manager
```

was created.

It provides additional permissions for the Feedback object without changing the user's profile.

The permission set was assigned to the Event Coordinator user.

---

# 🛡️ Sharing & Security

## Organization-Wide Defaults

The Event object was configured with:

```text
Internal Access: Private
External Access: Private
```

This provides a secure baseline for event records.

---

## Sharing Rule

### Rule Name

```text
Event_Sharing_For_Vendors
```

### Sharing Logic

```text
Event Coordinator
       ↓
Event Records
       ↓
Vendor Manager
       ↓
Read Only
```

This allows Vendor Managers to view relevant event records without giving them unnecessary editing access.

---

# 🔒 Security Model

The project uses multiple Salesforce security mechanisms:

* Profiles
* Roles
* Permission Sets
* Organization-Wide Defaults
* Sharing Rules
* Object Permissions
* Field Permissions
* Record-Level Access

Together, these features provide controlled access to EventForce data.

---

# 🚀 Phase 5 – Deployment, Documentation & Maintenance

The complete solution was developed and tested in a **Salesforce Developer Edition Org**.

Actual production deployment was not performed because Developer Edition is a standalone environment.

For enterprise deployment, the solution can be migrated using:

* Salesforce Sandboxes
* Change Sets
* Salesforce DX
* GitHub
* CI/CD pipelines
* DevOps tools

---

# 🧪 Testing

The system was tested for:

* Object creation
* Record creation
* Validation rules
* Lookup relationships
* Lookup filters
* Event cancellation approval
* Email notifications
* 3-day reminders
* Venue availability updates
* Duplicate venue booking prevention
* Batch Apex
* Scheduled Apex
* User permissions
* Role hierarchy
* Sharing rules
* Reports
* Dashboards

---

# 🔧 Maintenance & Troubleshooting

Regular monitoring is required for:

* Salesforce Flows
* Apex Triggers
* Apex Classes
* Scheduled Jobs
* Approval Processes
* User permissions
* Sharing settings
* Validation rules

Common troubleshooting areas include:

* User access errors
* Permission conflicts
* Failed flows
* Apex errors
* Incorrect sharing configuration
* Incorrect validation logic
* Data inconsistencies

---

# 🛠️ Technologies Used

| Technology           | Usage                     |
| -------------------- | ------------------------- |
| Salesforce CRM       | Main development platform |
| Salesforce Lightning | User interface            |
| Custom Objects       | Data management           |
| Salesforce Flow      | Business automation       |
| Apex                 | Advanced business logic   |
| Apex Triggers        | Record-level automation   |
| Batch Apex           | Bulk processing           |
| Scheduled Apex       | Automated scheduling      |
| Validation Rules     | Data validation           |
| Formula Fields       | Automated calculations    |
| Approval Processes   | Cancellation approvals    |
| Reports              | Business analysis         |
| Dashboards           | Data visualization        |
| Profiles & Roles     | Security                  |
| Permission Sets      | Additional access         |
| Sharing Rules        | Record-level sharing      |

---

# 📁 Main Salesforce Components

```text
EventForce Management System
│
├── Custom Objects
│   ├── Event__c
│   ├── Client__c
│   ├── Vendor__c
│   ├── Venue__c
│   ├── Feedback__c
│   └── EventVendor__c
│
├── Automation
│   ├── Client Reminder - 3 Days Before
│   └── Event Cancellation Approval
│
├── Apex
│   ├── VenueStatusHelper
│   ├── EventTrigger13
│   ├── PreventDoubleBooking
│   ├── BatchCompleteEvents
│   └── ScheduleCompleteEvents
│
├── Security
│   ├── Event Admin
│   ├── Event Coordinator
│   ├── Vendor Manager
│   ├── Client Profile
│   ├── Feedback Manager
│   └── Event Sharing Rule
│
├── Reports
│   └── Upcoming Events by Month
│
└── Dashboard
    └── EventForce Operations Dashboard
```

---

# 🎥 Project Demo Flow

The recommended project demonstration sequence is:

```text
1. Introduction
       ↓
2. Event Planner Lightning App
       ↓
3. Custom Objects
       ↓
4. Record Creation
       ↓
5. Validation Rules
       ↓
6. Automated Reminder Flow
       ↓
7. Cancellation Approval
       ↓
8. Apex Automation
       ↓
9. Security Configuration
       ↓
10. Reports & Dashboard
       ↓
11. Testing / Debugging
       ↓
12. Conclusion
```

---

# 📚 Learning Outcomes

Through this project, the following Salesforce concepts were implemented and practiced:

* Salesforce Developer Org setup
* Custom object creation
* Custom fields
* Lookup relationships
* Master-Detail relationships
* Many-to-Many relationships
* Junction objects
* Lookup filters
* Validation rules
* Formula fields
* Salesforce Flows
* Approval Processes
* Email Templates
* Apex Classes
* Apex Triggers
* Batch Apex
* Scheduled Apex
* Lightning App development
* Reports and dashboards
* Profiles
* Roles
* Permission Sets
* Organization-Wide Defaults
* Sharing Rules
* Data migration
* Testing and troubleshooting
* Salesforce deployment concepts

---

# 🔮 Future Enhancements

The EventForce Management System can be further enhanced with:

* Salesforce Experience Cloud for clients
* AI-powered event recommendations
* Automated vendor recommendations
* Online event booking and payments
* WhatsApp/SMS notifications
* Advanced analytics
* Mobile application integration
* AI chatbot for client support
* Real-time event tracking
* Automated vendor performance scoring
* Calendar integration
* Advanced approval workflows

---

# 📌 Conclusion

The **EventForce Management System** demonstrates how Salesforce can be used to build a complete CRM solution for event planning and management.

The project integrates **custom data modeling, Salesforce Flow automation, approval processes, Apex programming, security configurations, reports, and dashboards** into a single platform.

The system helps event planners manage clients, vendors, venues, events, and feedback while reducing manual work and improving operational visibility.

Although the project was implemented in a Salesforce Developer Edition environment for academic purposes, its architecture follows real-world Salesforce CRM implementation practices and can be extended for enterprise-level deployment.

---

## 👨‍💻 Project

**Project Name:** EventForce Management System
**Platform:** Salesforce
**Project Type:** CRM / Event Management
**Environment:** Salesforce Developer Edition
**Primary Technologies:** Salesforce, Apex, Flow, Lightning, Reports & Dashboards

---

## 🙏 Acknowledgement

This project was developed as an academic Salesforce implementation project to understand CRM development, Salesforce automation, Apex programming, security configuration, reporting, and deployment concepts.

**Thank You!**

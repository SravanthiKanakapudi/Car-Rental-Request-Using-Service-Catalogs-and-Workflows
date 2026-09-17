# Car Rental Request Automation in ServiceNow

## Project Overview

The **Car Rental Request Automation** project is developed using **ServiceNow Service Catalog and Flow Designer** to automate the process of requesting, approving, assigning, and tracking rental cars.

The system allows users to submit a car rental request through a Service Catalog item. The request is automatically routed for approval, car availability is checked, and a fulfillment task is created for the appropriate team.

## Technologies Used

- ServiceNow
- Service Catalog
- Flow Designer
- Business Rules
- JavaScript
- Catalog Tasks
- Notifications
- Update Sets
- ServiceNow Developer Instance (PDI)

## Requirements

- ServiceNow Developer Instance (PDI)
- Service Catalog access
- Flow Designer
- Administrator/System Administrator role
- Basic knowledge of ServiceNow
- Basic JavaScript knowledge

## Service Catalog Item

### Car Rental Request

The Service Catalog item contains the following variables:

- Car Type
- Pickup Location
- Drop Location
- Requested Date
- Justification

## Workflow

The automation follows these steps:

1. User submits a Car Rental Request.
2. The request is created in ServiceNow.
3. The request is sent for manager approval.
4. The system checks car availability.
5. If a suitable car is available, a fulfillment task is created.
6. The task is assigned to the Car Rental/Fleet Team.
7. Notifications are sent to the requester.
8. The request is completed after the car is assigned.

## Main Components

### 1. Service Catalog

A Service Catalog item named **Car Rental Request** is created to collect rental details from the user.

### 2. Flow Designer

Flow Designer automates:

- Request processing
- Approval
- Car availability checking
- Fulfillment task creation
- Notifications

### 3. Business Rule

A Business Rule is used to prevent a car from being assigned when its availability status is not **Available**.

### 4. Catalog Task

A Catalog Task is created for the fulfillment team with details such as:

- Requested Date
- Pickup Location
- Drop Location
- Car Type

## Notifications

The system can send notifications for:

- Request Submitted
- Request Approved
- Request Rejected
- Car Assigned
- Request Completed

## Testing

The project can be tested using the following scenarios:

| Test Case | Expected Result |
|---|---|
| Successful request | Request is approved and fulfillment task is created |
| Manager rejects request | Request is rejected |
| No car available | Car assignment is prevented |
| Missing mandatory field | Request cannot be submitted |

## Project Objective

The main objective of this project is to reduce manual effort by automating the car rental request process and providing a centralized system for request submission, approval, fulfillment, and tracking.

## Deployment

The ServiceNow configurations can be packaged using an **Update Set** and transferred from one ServiceNow instance to another.

**##FlowChart**

                    ┌──────────────┐
                    │    START     │
                    └──────┬───────┘
                           ↓
             ┌──────────────────────────┐
             │ User submits Car Rental  │
             │ Request (Service Catalog)│
             └────────────┬─────────────┘
                          ↓
             ┌──────────────────────────┐
             │ Request / RITM Created   │
             └────────────┬─────────────┘
                          ↓
                ┌───────────────────┐
                │ Manager Approval? │
                └───────┬─────┬─────┘
                        │     │
                  YES   │     │   NO
                        ↓     ↓
        ┌──────────────────┐  ┌─────────────────┐
        │ Check Car        │  │ Request         │
        │ Availability     │  │ Rejected        │
        └────────┬─────────┘  └────────┬────────┘
                 ↓                     ↓
        ┌──────────────────┐    ┌─────────────────┐
        │ Car Available?   │    │ Send Rejection  │
        └───────┬─────┬────┘    │ Notification    │
                │     │         └────────┬────────┘
           YES  │     │ NO              ↓
                ↓     ↓              ┌───────┐
     ┌──────────────┐ ┌────────────┐ │  END  │
     │ Create       │ │ No Car     │ └───────┘
     │ Catalog Task │ │ Available  │
     │ (sc_task)    │ └─────┬──────┘
     └──────┬───────┘       ↓
            ↓        ┌─────────────────┐
     ┌──────────────┐│ Send No Car     │
     │ Assign Task  ││ Available       │
     │ to Fleet/    ││ Notification    │
     │ Car Rental   │└────────┬────────┘
     │ Team         │         ↓
     └──────┬───────┘      ┌───────┐
            ↓              │  END  │
     ┌──────────────┐      └───────┘
     │ Assign Car   │
     └──────┬───────┘
            ↓
     ┌──────────────────┐
     │ Send Assignment  │
     │ Notification     │
     └────────┬─────────┘
              ↓
     ┌──────────────────┐
     │ Mark Request     │
     │ as Completed     │
     └────────┬─────────┘
              ↓
          ┌───────┐
          │  END  │
          └───────┘

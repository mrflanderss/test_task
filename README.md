
# Release Notes (task #3)

We present to you subscription plans. By purchasing them your clients will be able to manage their limits within our system.

You can offer your users both predefined plans and your own, which you can create and customize using the [Subscription Plan API](https://app.swaggerhub.com/apis/ALEXEY0127/subscription-plan_api/1.0.0).

We've also updated the User model. Now, the response to the [GET api/external/user](https://app.swaggerhub.com/apis/ALEXEY0127/subscription-plan_api/1.0.0) request includes a new *sub_status* parameter. This parameter provides information about the subscription status.


# Subscription Plan API (task #1)

## Introduction

The Subscription Plan API provides a way for users to manage their limits in the System. Subscription plans are connected with specific user types. When a user purchases a subscription, we change their type on our side.

With the Subscription Plan API, providers can manage subscription plans of their users: 

- Retrieve all available plans.
- Add new subscription plans.
- Update existing plans.
- Purchase a subscription plan.

## How it works

Now, users in the System have predefined types with varying limits. Below are the available user types in the System:


|Type name     |Specifics and limits                       |Possible changes                         |
|----------------|-------------------------------|-----------------------------|
|`signing`           |Service type. Assigned while the subscription plan is being processed. The System doesn't return this status to partners via the API.|`company`, `advanced`             |
|`guest`           |KYC not completed. Sessions are limited to 20 minutes, with up to 5 operations per 24 hours and 20 in 7 days.|`basic`            |
|`basic`           |KYC completed. Up to 20 operations per 24 hours and no more than 50 in 7 days.|`advanced`, `guest`            |`advanced`, `guest`
|`company`         |No limits|`advanced`|
|`advanced`        |No limits|`basic`, `company`|
|`admin`        |Type for System administrators. No limits|-|

Providers can create custom types if the predefined ones don't meet their needs.

### Manage subscription plans: add, update, and retrieve

Some text here


### Purchase a subscription plan

To allow a user to pay for a subscription plan, the following process applies:

Step 1.  **User requests available plans:**

The user sends a request to the provider asking for the available subscription plans.

Step 2.  **Provider retrieves plans from the system:**

The provider sends a GET request to the System's API endpoint (api/external/user) to fetch a list of available plans.

Step 3.  **System responds with available plans:**

The system returns the list of available plans to the provider.

Step 4.  **Provider shows the plans to the user:**

The provider presents the list of plans to the user.

Step 5.  **User purchases a subscription plan:**

The user selects a plan and sends a request to buy it.

Step 6.  **Provider submits the purchase to the system:**

The provider sends a POST request to the system's API endpoint (api/partner/buy) to initiate the purchase.

Step 7.  **System updates the user's status:**

The system temporarily changes the user's status to `signing`.

Step 8.  **System redirects the user to the payment page:**

The system provides the user with a payment link to complete the transaction.

Step 9.  **Payment outcome:**

• Option A.  **If the payment is approved:**

The system updates the user's status to the new, upgraded status.

• Option B. **If the payment is rejected:**

The system reverts the user's status to the previous state.

Step 10.  **System sends a callback to the provider:**

The system sends the updated user object, including the current status, back to the provider.

Step 11.  **Provider informs the user:**

The provider notifies the user about their current subscription plan and status.

### Sequence Diagram

```mermaid
sequenceDiagram
User -> Provider: Ask for available plans
Provider -> System: GET api/external/user
System --> Provider: Return list of available plans
Provider --> User: Show available plans
User -> Provider: Buy subscription plan
Provider -> System: POST api/partner/buy
System -> System: Change user status to signing
System --> User: Redirect to payment page
alt Payment approved
System -> System: Change user status to a upgraded one
else  Payment rejected
System -> System: Return previous status of the user
end
System --> Provider: Send callback with user object
Provider --> User: Inform user about their current plan
```

## Base URL

`https://system.com/v1`

---

## Authentication 

Authentication depends on the request type:

* Authentication doesn't apply for **GET** requests. Make sure you send requests using the System's VPN.

* Use [HTTP Basic auth](https://en.wikipedia.org/wiki/Basic_access_authentication) for **POST** requests. Ask your technical lead to obtain username and password. Specify the obtained credentials in the *Authorization* parameter of the request header.

    ```curl
    Authorization: Basic YXBkdXJlcjphcGlrbC9k 
    ```
## Rate limits

In progress

## Incoming notifications

In progress

---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://app.planhat.com'>Login to get your API Key</a>
  - <a href='https://www.planhat.com'>Customer Success API from Planhat</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Planhat API! It will help you interact with Planhat whenever you need something not covered by our standard integration.

The API has a few "open" end point that can be used to push data without the need to authenticate. This is ideal if all you need is to send some server side metrics,
user activities, call logs etc. If you need more that that your API keys will give you access to the full API which will let you do almost anything related to you data in Planhat.

There is a default limit of approximately 200 API calls / minute. Should you send more requests you'll receive a "429 Too many requests" response. The limit of 200 calls is reset every 60-120 seconds, and each response includes information about your limit and how many calls you have left until next reset.

`X-Rate-Limit-Limit: 200`
`X-Rate-Limit-Remaining: 192`

If you need more capacity let us know.

In addition to the API, you can upload data in xlsx-format in Planhat. We have prepared a couple of template for this, and you can read more about it [Here](https://www.planhat.com/get-started/).

# Authentication
```shell
# With shell, you can just pass the correct header with each request
curl https://api.planhat.com/myprofile
  -H "Authorization: Bearer [PERSONAL_ACCESS_TOKEN]"
```

> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.

Most end-points require authentication by a Personal Access Token, managed from the Settings section in Planhat. Personal Access Tokens are static tokens created by you from within the App. Once a token is created, it will last forever. To disable access to a token, simply remove it.

Access tokens are personal, meaning that whatever automatic operation you perform with this token, will appear as if the user in question did it. Consider creating a separate API user and generate tokens for that user to keep things separated.

The Token should be placed in the Authorization header in ONE of the following ways:

`Authorization: Bearer [PERSONAL_ACCESS_TOKEN]`

OR

`Authorization: Basic [base64encode(PERSONAL_ACCESS_TOKEN:)]`

<aside class="notice">
Note the colon after the token if you go with the Basic approach. It’s required even though there is no password.
</aside>

# Companies

```json
[
{
  "name": "Google",
  "slug": "google",
  "owner": "580549e00000000000000000",
  "coOwner": "580549e00000000000000000",
  "externalId": "12345",
  "phase": "onboarding",
  "tags":["Gold", "East Coast"],
  "custom": {
    "referenceOk": true
  }
}
]
```

Companies, sometimes referred to as "Accounts", are your customers. It also covers companies that previously were your customer and potentially new customers (prospects).
Whatever the status of the company it's the same object and end-point.

`POST: https://api.planhat.com/companies`

`GET: https://api.planhat.com/companies/:id`

`PUT: https://api.planhat.com/companies/:id`

`DELETE: https://api.planhat.com/companies/:id`


### Properties

Property | Description
--------- | -----------
name* | Name of the Customer.
slug | Provide a custom slug in case you plan on adding multiple customers with the exact same name. Could be our own customer id, or an increasing counter, anything as long as it's unique. If your customer names are unique then you don't have to provide this, Planhat will create one for you.
owner | The the planhat user id of the Account Manager / Success Manager to whom this customer belongs. See section [Team (Planhat Users)] below for info about how to get the user ids.
coOwner | A potential co-pilot, in case a single owner isn't enough. See section [Team (Planhat Users)] below for info about how to get the user ids.
externalId | The customer id in your own system (will help us match other data you send over such as user activities, custom metrics etc). Not strictly required but if you've read this far it's most likely something you need.
phase | Each customer can be assigned a lifecycle phase in Planhat. It's a text string that should match the name of one of the phases in Planhat. If the name doesn't match any of the phases in Planhat it will still save.
tags | Array of strings to tag your customers. There is no separate end-point for tags so if you're going to update some of the tags for a given customer you'd have to update the whole array.
custom | A flexible object with custom data, one level. Add any custom data points on the form. Use short and human friendly names as keys. For example "Plan" or "Subscription Plan" is better than "SUBSCRIPTION_PLAN", as this is how it will be displayed in Planhat. Date, Strings, Boolean, Number are allowed types.




### Company lookup

When you need a lightweight list of all companies in Planhat to match against your own id's etc.

`GET: https://api.planhat.com/leancompanies?externalId=yourid123`



# Team

If you would like to add the owner property while creating new companies and you have the user's email but not the planhat id, then this end-point will help:

`GET: https://api.planhat.com/users?email=yourcolleaguesname@yourcompany.com`


# Contacts

These are people working at your customers, business contacts as well as your actual end users.


`POST:  https://api.planhat.com/endusers`

`GET:  https://api.planhat.com/endusers/:id`

`PUT:  https://api.planhat.com/endusers/:id`

`DELETE:  https://api.planhat.com/endusers/:id`

The GET request can be filtered to only list users at a specific company.

`/endusers?c=55242d576b8275de65f567e8`

Property | Description
--------- | -----------
companyId* | Planhat company id to decide which company profile a certain end-user belongs to.
companyName | Name of the company the user belongs to. Only used to display which company a certain user belongs to in the aggregated contact/end-user lists. There is no validation that this name actually matches the company name of the companyId provided. To link a user to a certain company profile, only the companyId parameter will be considered.
createDate | Date the contact was created, defaults to time of creation if not specified.
externalId | The user id in your own database, or some other external system
position | The role or position of this contact/user. e.g, "CFO"
phone | You pick the telephone number format, any string will do.
email | Email address of the user/contact
featured | Boolean marker to single out contacts of special importance, typically set this to true if this is a user your sales team sometimes speak with, have the phone number to etc. Set to false if this is "just another user".
tags | Array of Strings to tag each contact


# Calls

```shell
# With the open end-point no auth required
curl -X POST \
-H "Content-Type: application/json" \
-d '{"nr": "+4670123456", "externalUserId": "max123", "start": "2016-10-17T14:00:00.000Z", "end": "2016-10-17T14:02:00.000Z", "waiting": 10}' \
https://api.planhat.com/199627f7-0d65-52cd-89b9-45b5ff561bem/calls
```
> Remember to replace the sample tenant id above `199627f7-0d65-52cd-89b9-45b5ff561bem` with your own.

```shell
# You call also the authenticated end point to add calls
curl -X POST \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"nr": "+4670123456", "externalUserId": "max123", "start": "2016-10-17T14:00:00.000Z", "end": "2016-10-17T14:02:00.000Z", "waiting": 10}' \
https://api.planhat.com/calls
```
> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.

When you're looking for a custom connection of your VOIP/phone solution to Planhat this is the end point.


`POST:  https://api.planhat.com/calls`

`POST:  https://api.planhat.com/[YOUR_TENANT_UUIS]/calls` (open, no auth header)

`GET:  https://api.planhat.com/calls/:id`

`PUT:  https://api.planhat.com/calls/:id`

`DELETE:  https://api.planhat.com/calls/:id`


Property | Description
--------- | -----------
nr* | Phone number of the contact you called, this will be used to match against customer and enduser in Planhat.
externalUserId* | UserId in your on system, used to find a matching "agent" / team member
start | When call started
end | When the call ended
waiting | In seconds, the waiting time before the "talk" started (waiting + talk_duration = end - start)





# Other end-points

There are plenty of end-points to do pretty much anything you'd like. Let us know what you need and will provide you with the relevant details.

# Companies

```shell
curl -X POST \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"name": "Google", "externalId": "12345abcd", "phase": "onboarding", "tags": ["Gold", "East Coast"], "custom": {"referenceOk": true}}' \
https://api.planhat.com/companies
```
> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.


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

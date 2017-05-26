# Tickets

This end point doesn't have the normal GET, POST, PUT, DELETE.
Create and update operations are combined into an "upsert" (PUT) request.
The PUT body can contain either a single JSON object, or an array of objects (tickets)

sourceId is used to determine if this is an update or create operation.

<aside class="notice">
Note that if the email of a ticket doesn't match any user in Planhat, but there is enough information to create a new user,
then Planhat will create a new user, link it to the corresponding customer, and then create the ticket with a link to that customer.
</aside>

```shell

curl -X PUT \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"source": "HomeGrownSupport", "sourceId": "123", "email": "max@customer.com", "createdAt": "2016-10-17T14:00:00.000Z", "updatedAt": "2016-10-17T14:02:00.000Z", "status": "resolved"}' \
https://api.planhat.com/tickets
```
> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.



`PUT: /tickets`

`GET: /tickets`

`DELETE: /tickets/:id`


Property | Description
--------- | -----------
source | The name of the system this ticket originates from. Typically "Zendesk", "Desk" etc, but since you're reading these docs you may the tickets in some other tool.
sourceId* | Required. Id of the ticket in the source system
email* | Required. Email of the person who submitted the ticket. Items without email will be silently dropped.
companyExternalId | The External Company Id, see the section "Company lookup" for a way to convert between different customer id's. Not required but can help automatically create a new user if it doesn't already exist, or if there is no external user related to the ticket but you still want it linked to a customer.
title | If the ticket has a title or main subject.
description | Description of the ticket
url | Url to where more information can be found about this ticket.
tags | Array of Strings
type | the type of the ticket. Use as you like, but typically it could be: "bug", "feature request", "training" etc.
severity | String describing the severity, no restrictions on the scale apart from that.
product | Name of the product to which the ticket relates
timeSpent | Time spend on this ticket, measured in number of minutes, can be a decimal.
createdAt | Date
updatedAt | Date
status | String, you pick any status options you like. Typically "new", "penidng", "open", "resolved", "closed" or something similar.

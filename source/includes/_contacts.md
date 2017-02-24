
# Contacts

These are people working at your customers, business contacts as well as your actual end users.


`POST: /endusers`

`GET: /endusers/:id`

`PUT: /endusers/:id`

`DELETE: /endusers/:id`

The GET request can be filtered to only list users at a specific company.

`/endusers?c=55242d576b8275de65f567e8`

Property | Description
--------- | -----------
companyId* | Planhat company id to decide which company profile a certain end-user belongs to.
companyName | Name of the company the user belongs to. Only used to display which company a certain user belongs to in the aggregated contact/end-user lists. There is no validation that this name actually matches the company name of the companyId provided. To link a user to a certain company profile, only the companyId parameter will be considered.
createDate | Date the contact was created, defaults to time of creation if not specified.
externalId | The user id in your own database, or some other external system
name | The user's full name
position | The role or position of this contact/user. e.g, "CFO"
phone | You pick the telephone number format, any string will do.
email | Email address of the user/contact
featured | Boolean marker to single out contacts of special importance, typically set this to true if this is a user your sales team sometimes speak with, have the phone number to etc. Set to false if this is "just another user".
tags | Array of Strings to tag each contact

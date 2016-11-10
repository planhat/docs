# Calls

```shell
# With the open end-point no auth required
curl -X POST \
-H "Content-Type: application/json" \
-d '{"nr": "+4670123456", "userId": "max123", "agentId": "987qwe", "start": "2016-10-17T14:00:00.000Z", "end": "2016-10-17T14:02:00.000Z", "waiting": 10}' \
https://api.planhat.com/199627f7-0d65-52cd-89b9-45b5ff561bem/calls
```
> Remember to replace the sample tenant id above `199627f7-0d65-52cd-89b9-45b5ff561bem` with your own.

```shell
# You call also use the authenticated end point to create calls
curl -X POST \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"nr": "+4670123456", "userId": "max123", "agentId": "987qwe", "start": "2016-10-17T14:00:00.000Z", "end": "2016-10-17T14:02:00.000Z", "waiting": 10}' \
https://api.planhat.com/calls
```
> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.

When you're looking for a custom connection of your VOIP/phone solution to Planhat this is the end point.


`POST:  https://api.planhat.com/calls`

`POST:  https://api.planhat.com/[YOUR_TENANT_ID]/calls` (open, no auth header)

`GET:  https://api.planhat.com/calls/:id`

`PUT:  https://api.planhat.com/calls/:id`

`DELETE:  https://api.planhat.com/calls/:id`


Property | Description
--------- | -----------
nr* | Phone number of the contact you called, this can be used to match against customer and enduser in Planhat, if the userId of the contact is not provided.
userId* | Identify the user (contact) to whom you were talking by the userId in you'r own system (externaId)
phUserId* | Identify the user (contact) to whom you were talking by the userId in in Planhat
agentId* | The agent's (your team member's) user id in your on system, used to find a matching "agent" / team member
phAgentId* | The agent's (your team member's) user id in Planhat, used to find a matching "agent" / team member
start | When call started
end | When the call ended
waiting | In seconds, the waiting time before the "talk" started (waiting + talk_duration = end - start)
direction | "in" for inbound or "out" for outbound
record | link/uri to recording, if available

Preferably identify the customer contact by id (userId or phUserId), relying on the phone number is slightly less robust but will also work.

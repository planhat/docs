# NPS

```shell
# You call also use the authenticated end point to create calls
curl -X PUT \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"email": "john.smith@gmail.com", "score": 9, "comment": "Excellent UX", "dateSent": "2017-06-07T04:36:44Z", "dateAnswered": "2017-06-08T15:25:44Z", "source": "custom-nps.com", "sourceId": "4651"}' \
https://api.planhat.com/nps
```

> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.


A contact in Planhat can be linked to multiple NPS survey responses, but only the last one will be used to get a score on company level.


`GET: /nps`

`PUT: /nps`

`DELETE: /nps/:id`


### Properties

Property | Type | Description
--------- | ----------- | -----------
email* | String |  Email of the contact that has been surveyed
score* | Number |  Feedback score that the user submit in his/her survey (0-10)
comment | Sting |  The comment when the user gives an score in survey
dateSent | Date |  ISO date when the survey was sent
dateAnswered | Date |  ISO date when the survey was answered
source | String | The name of the system that NPS scores come from. For example: “custom-nps.com”.
sourceId | String | The id of the NPS score (response) in your system. This id is used as a matching key for update if already there’s an score with this sourceId

# Revenue - Monthly

If each of your customers only has one subscription plan at a time, and renewing monthly,
then this is the easiest way to manage your subscription/license data through the API.
A single end-point to handle new subscriptions, upgrades, downgrades and churn.

```shell
# You call also use the authenticated end point to create calls
curl -X PUT \
--user "[PERSONAL_ACCESS_TOKEN]"
-H "Content-Type: application/json" \
-d '{"action": "newMrr", "data": [{"cId": "customer24", "value": 200, "currency": "SEK", "date": "2017-03-02T14:00:00.000Z"}]}' \
https://api.planhat.com/licenses/actions
```

> Replace `[PERSONAL_ACCESS_TOKEN]` with your API key.




`PUT: /licenses/actions`



### Properties

Property | Type | Description
--------- | ----------- | -----------
action* | String | You should set this value to: "newMrr"
data* | String | This must be an array of updates (even if it's only one), each element containing the four properties below
data.element cId* | String | External id of the company.
data.element date* | Date | ISO date indication when the the new mrr takes effect (at the latest)
data.element currency* | String | 3 letter currency code, e.g, "EUR" or "USD"
data.element value* | Number | the new mrr value


Typically you'll have some automated process to charge the subscription fee from your customer's credit cards.
So when a customer pays for the first time you send the value and the date when the subscription started, mostly it will be the current date.

From this point on you don't need to make any updates to planhat unless there is a chance.
Planhat will automatically renew the subscription for another month at the end of each period.

When there is an update, send the new mrr value and a date that either matches the exact start date of the period with the new value, or any other date within that period.
For each of the updates received, Planhat will look for (and update) the first subscription period that has an end-date less than (but not equal to) the date provided.
When the value of a subscription period is updated, all the subsequent periods (if any exist) will also be updated with the same value.

To cancel a subscription, send a value of 0, that will remove the first subscription that has an end-date less than (but not equal to) the date provided.
The preceding subscription period will keep it's value but get the status "lost".

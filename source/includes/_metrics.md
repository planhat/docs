
# Metrics
```shell
curl -X POST \
-H "Content-Type: application/json" \
-d '[{"dimensionId": "flightTime", "value": 1, "companyExternalId": "airbus"}, {"dimensionId": "debtRatio", "value": 5.2, "companyExternalId": "boing"}]' \
https://api.planhat.com/[YOUR_TENANT_TOKEN]/dimensiondata
```
While User Activity says a lot about user engagement, it doesn't neccesarily reflect the value created. Dimension Data is a set of metrics to understand how well you customers are doing, and the value they get out of your service.

Depending on what metrics you pick you might either want to send it as it happens or once a day. Number of user logins for example. You could either send an post everytime a user logs in, or you could save logins at your end and once a day send the total count for the day to planhat. Typically the more granular data you send to planhat, the more options you'll have to later crunch that data.

Note: The end point accepts either a single object or an array, which can be useful if you run a batch update with some intervall, or if you initially would like to load historical data.

`POST: /[YOUR_TENANT_TOKEN]/dimensiondata`

Property | Description
--------- | -----------
dimensionId* | Any string without spaces or special characters. If you're sending "Share of Active Users" a good dimensionId might be "activeusershare". It's not displayed in Planhat but will be used when building Health Metrics in Planhat.
value* | The raw (number) value you would like to set.
companyExternalId | This is the company id in your systems. For this to work the profiles in Planhat will need to have this externalId set.
date | Pass any valid JavaScript date string to specify the date of the event. In none is provided we will use the time the request was received.
assetExtId | Most likely you should leave this out, let us know if you need details about how to work with multiple assets.

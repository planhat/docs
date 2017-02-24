
# User Tracking
Keeping track of your end-user activity is an important part of the Customer Success effort. Getting started involves only two steps.

1. Decide a few events (user actions) that you want to track.
2. Decide how you want to get that information from your users to Planhat.

### What events to track
The most obvious events are probably logins and page-views. Logins tend to be fairly inaccurate since it depends a lot on user behavior, some people login and logout a lot, others keep their sessions open. Page views is likely a better metric. Even though page views have relatively weak correlation with the value created, it does give you a general sense of activity level, and it's great to understand when a given user/customer was last active.

In addition to page views you can probably find even more meaningful metrics such as “did an advanced search”, “created a campaign”, “added a team-member” etc. The exact metrics will obviously depend on the service you provide but 2-3 such events should be more than enough to get started. Add a weight if you want to indicate that some actions are more important/valuable than others.

### How to track
There are 3 different ways of getting your end-user activity into Planhat.

1. API (real time)
2. Planhat Tracking Script
3. Segment
4. API Bulk
5. API Stream


## 1. API (real time)
```shell
# With the open end-point no auth required
curl -X POST \
-H "Content-Type: application/json" \
-d '{"email": "ariana.wright_0@daimler.com", "action": "Logged in", "info": {"index": 20, "theme": "Blue"}, "date": "2016-10-13T08:00:00.000Z"}' \
https://api.planhat.com/analytics/[YOUR_TENANT_ID]
```
> Remember to replace the tenant id above with your own.

User activity relevant for your Customer Success strategies will typically involve a call to your servers.
Once the activity/event is captured you can simply forward it from your servers to Planhat’s activity end-point:



`POST: /analytics/[YOUR_TENANT_ID]`

Property | Description
--------- | -----------
name* | Full name of the contact/user.
email* | The contact’s id (user_id) in your system (required if externalId not specified)
externalId* | The contact’s id (user_id) in your system (required if email not specified)
companyExternalId | Typically the account/customer id in your system, the same id have to be present as externlId in Planhat.This is required to automatically add new end-users to the correct customer profile in Planhat. If left out, a Customer Success Rep will manually have to assign new end-users to the appropriate customer, or Planhat will make a qualified guess when certain enough it will be correct.
weight | optional parameter in case you want to give more RELATIVE importance to certain events, otherwise ignore or set to 1 which is the default.
date | Pass any valid JavaScript date string to specify the date of the event. In none is provided we will use the time the request was received.
action | Parameter describing the user activity. Typically on the form Action + Object, for example “Sent Invoice”,but it could also be just “Login” or whatever describes the activity event. This parameter is optional, but highly recommended and required if you want the appear in the event stream.
info | An optional, valid JSON object where you can add additional information related to the event. If the user activity was "purchased a gift card" then it might make sense to add info about the price, or product category.



## 2. Planhat Tracking Script
Your second option is adding a a small tracking script that we provide to your application
Like any tracking script it’s a tiny snippet (< 1 kb) you place just before the closing </head> tag.
The snippet will asynchronously load the tracking script to send data directly from your users to Planhat.
Just like Google Analytics or any other tracking script it won't slow down you app or have any other noticeable impact on your users.


`<script type="text/javascript">
var plantrack=plantrack||[];!function(){for(var t=["init","identify","track"],
a=function(t){return function(){plantrack.push([t].concat(Array.prototype.slice.call(arguments,0)))}},
n=0;n<t.length;n++)plantrack[t[n]]=a(t[n])}(),plantrack.load=function(t,a){plantrack._endpoint=t,a&&plantrack.init(a);
var n=document.createElement("script");n.type="text/javascript",n.async=!0,
n.src="https://app.planhat.com/analytics/plantrack.min.js";
var r=document.getElementsByTagName("script")[0];r.parentNode.insertBefore(n,r)};
plantrack.load("https://api.planhat.com/analytics/[YOUR_TENANT_TOKEN]");
</script>`

Then in your application, identify users as they login. This information is saved in a cookie ("\_plantrack") and used to identify the user in subsequent requests.

`plantrack.identify(USER_ID, {
email: USER_EMAIL,
name: USER_NAME,
companyId: CUSTOMER_ID
});`

Property | Description
--------- | -----------
USER_ID | The User Id in your database, or some other relevant id. Highly recommended and required if no USER_EMAIL is provided.
USER_EMAIL | The user's email, highly recommended and required if no USER_ID is provided.
USER_NAME | The full name of this contact / user. If not provided we'll try to use the email instead.
CUSTOMER_ID | The customer id (account id) added in Planhat as "externalId". Typically this would be the account-id in your own database. If you provide this id then Planhat can automatically map new (or "duplicate") users to the correct customer profile.


After that, when the event you would like to track happens simply call
plantrack.track(ACTION, [EXTRA_INFO]) from your app, for example:

`plantrack.track("created a new project", {"weight": 7});`

User information stored in the cookie (most importantly USER_ID) will automatically be added to all tracking calls.

Property | Description
--------- | -----------
ACTION | Description of the event. Examples could be "Added a team Member", "Downloaded a Report".
If you can make the actions somewhat specific it's typically valuable.
For example the action "Viewed a Page" could be further specified like "Viewed Settings Page", or "Viewed Page 'path/to/page'"
EXTRA_INFO / WEIGHT | Most likely the actions your end-users take in your service are not equally important. Generating a report, adding a comment, creating a project etc perhaps means more than a simple page view. To reflect this a parameter "weight" can optionally be added to each tracking call as a way to indicate the relative importance of different actions. Since it's relative, the absolute value (baseline) doesn't matter. But to make it readable in Planhat a guideline is to have an average user accumulate weights of 100 or less over a month.

<aside class="notice">
If you know you'll be adding multiple tracking scripts and don't want to use Segment consider
creating your own container and adding the tracking to that "proxy" as described in [this
quora answer](http://www.quora.com/Whats-the-best-way-to-use-Google-Analytics-and-Mixpanel).
</aside>

## 3. Segment
> Sample post to test the end-point without the need to pass through Segment.  Note the intentional colon (:) after your access token.

```shell
curl -X POST \
--user "[PERSONAL_ACCESS_TOKEN]:"
-H "Content-Type: application/json" \
-d '{"type": "identify", "traits": {"name":  "Peter Gibbons", "email": "peter@initech.com", "companyId": "ABCDE"}}' \
https://api.planhat.com/dock/segment
```




As a third option, Segment can be used to send User Events (user tracking data) to Planhat. When we receive an event from Segment,
we use the "user id" and/or email to to know which Planhat user the event refers to.
The events are then displayed in Planhat both on user and company level. When presented in text form, we display it as [user name]  + [event].
Preferably use event names on the form "verb + noun". For example "downloaded a report" is a better event name then "report_dowloaded".

Note that Segment is a service with it’s own license cost.


> Expected data formats

```json
{
  "type": "identify",
  "user_id": "12345",
  "traits" : {
    "name": "Peter Gibbons",
    "email":"peter@initech.com",
    "companyId": "ABCDE"
  }
}
```

```json
{
  "type": "track",
  "user_id": "12345",
  "event": "downloaded a report",
  "properties" : {}
}
```

### Creating Users Automatically
If we receive a call (Identify or Track) for a user that doesn't yet exist in Planhat, and the request contains a customer id, then Planhat will automatically create a new user for you. This is highly recommended unless you create users in Planhat automatically through the API. If you're providing an email address it is also likely that Planhat will be able to map a new end user to the correct customer account with sufficient accuracy to avoid manual confirmation, however most Segment set-ups rely not on email but in user id.



### Ignored Events

Events will be completely ignored when any of the following conditions are met

1. Request is not of type "Identify" or "Track"
2. Both User Id and Email are missing



### Un Assigned Users
If we receive a call (Identify or Track) for a user that doesn't yet exist in Planhat, and the request does NOT contain information about which customer it relates to, and we have no other way to relate the user to a customer with reasonable confidence, then it will be discarded but added a report in Planhat to help you identify potential issues with the integration. Typically this may happen when users that loggedin (with a persistant session) before the integration was activated. Events from these users will end up in the log, and eventually as the user gets logged out and login again it'll start working as expected.



## 4. API Bulk
```shell
# With the open end-point no auth required
curl -X POST \
-H "Content-Type: application/json" \
-d '[{"name": "Ariana Wright" "email": "ariana.wright@daimlor.com", "cId": "abc123", ""action": "Logged in", "date": "2016-10-13T08:00:00.000Z", "weight": 1, "count": 1}]' \
https://api.planhat.com/analytics/bulk/[YOUR_TENANT_ID]
```
> Remember to replace the tenant id above with your own.

The body contains an array of events. There is an upper limit on the body size of 10 Mb for each post, which typically corresponds to about 50,000 items. If you need more that that or if your data is on a csv format, the streaming option (see next section) is a better option.


`POST: /analytics/bulk/[YOUR_TENANT_ID]`

Property | Description
--------- | -----------
name | Full name of the contact/user.
email* | The contact’s id (user_id) in your system (required if externalId not specified)
id* | The contact’s id (user_id) in your system (required if email not specified)
cId | Typically the account/customer id in your system, the same id have to be present as externlId in Planhat.This is required to automatically add new end-users to the correct customer profile in Planhat. If left out, a Customer Success Rep will manually have to assign new end-users to the appropriate customer, or Planhat will make a qualified guess when certain enough it will be correct.
weight | Weight. optional parameter in case you want to give more RELATIVE importance to certain events, otherwise ignore or set to 1 which is the default.
date | Date. Pass any valid JavaScript date string to specify the date of the event. In none is provided we will use the time the request was received.
action | Action. Parameter describing the user activity. Typically on the form Action + Object, for example “Sent Invoice”,but it could also be just “Login” or whatever describes the activity event. This parameter is optional, but highly recommended and required if you want the appear in the event stream.
count | Indicate if the event happened multiple times, for example if the import cover past 24 hours and a given user logged in twice during this period, you could send it as one row and instead set the count to 2, of course they'd get the exact same date, but mostly this is not an issue. Defaults to one (1).


## 5. API Stream

```javascript
// Node Example of streaing a csv file to Planhat
var fs = require('fs');
var request = require('request');
var YOUR_TENANT_ID = 'fb08c97b-41f5-4a22-8888-579a3ce223ae';
var endPoint = 'https://api.planhat.com/analytics/stream/csv/'+YOUR_TENANT_ID;

fs.createReadStream('sampleData.csv').pipe(request.put(endPoint));
```

`PUT: /analytics/stream/csv/[YOUR_TENANT_ID]`

The csv file should have the following header:
cId, id, name, email, weight, action, date, count



Property | Description
--------- | -----------
name | Full name of the contact/user.
email* | The contact’s id (user_id) in your system (required if externalId not specified)
id* | The contact’s id (user_id) in your system (required if email not specified)
cId | Typically the account/customer id in your system, the same id have to be present as externlId in Planhat.This is required to automatically add new end-users to the correct customer profile in Planhat. If left out, a Customer Success Rep will manually have to assign new end-users to the appropriate customer, or Planhat will make a qualified guess when certain enough it will be correct.
weight | Weight. optional parameter in case you want to give more RELATIVE importance to certain events, otherwise ignore or set to 1 which is the default.
date | Date. Pass any valid JavaScript date string to specify the date of the event. In none is provided we will use the time the request was received.
action | Action. Parameter describing the user activity. Typically on the form Action + Object, for example “Sent Invoice”,but it could also be just “Login” or whatever describes the activity event. This parameter is optional, but highly recommended and required if you want the appear in the event stream.
count | Indicate if the event happened multiple times, for example if the import cover past 24 hours and a given user logged in twice during this period, you could send it as one row and instead set the count to 2, of course they'd get the exact same date, but mostly this is not an issue. Defaults to one (1).

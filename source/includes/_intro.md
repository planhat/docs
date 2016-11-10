# Introduction

Welcome to the Planhat API! It will help you interact with Planhat whenever you need something not covered by our standard integration.

The API has a few "open" end point that can be used to push data without the need to authenticate. This is ideal if all you need is to send some server side metrics,
user activities, call logs etc. If you need more that that your API keys will give you access to the full API which will let you do almost anything related to you data in Planhat.

There is a default limit of approximately 200 API calls / minute. Should you send more requests you'll receive a "429 Too many requests" response. The limit of 200 calls is reset every 60-120 seconds, and each response includes information about your limit and how many calls you have left until next reset.

`X-Rate-Limit-Limit: 200`
`X-Rate-Limit-Remaining: 192`

If you need more capacity let us know.

In addition to the API, you can upload data in xlsx-format in Planhat. We have prepared a couple of template for this, and you can read more about it [Here](https://www.planhat.com/get-started/).

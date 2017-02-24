# Licenses (Subscriptions)



A company can have multiple concurrent licenses, presently, historically and in the future.
A number of important metrics such as the customer's MRR and lifecycle phase are calculated based on the
license data.


`POST: /companies/:companyId/licenses`

`GET: /companies/:companyId/licenses/:id`

`PUT: /companies/:companyId/licenses/:id`

`DELETE: /companies/:companyId/licenses/:id`


### Properties

Property | Type | Description
--------- | ----------- | -----------
externalId | String | License id in your system, in SalesForce or in some other external system you're using to manage you licenses.
product | String | Name of the license product. Not required but highly recommended. Even if you only have one product we suggest adding it as "License Fee" or "Subscription".
\_currency* | String | The currency code, for example "USD". There is a "soft" mapping to the id's in your list of currencies. Technically we accept any string but we recommend using standard currency codes as Ids. Since the mapping is soft you can add licenses with a currency code not yet available in your list fo currencies.
fromDate* | Date | Start of the license period.
toDate | Date | End of the license period
fixedPeriod | Boolean | Boolean to indicate if this is an ongoing license (no defined end date) or license with a fixed term. Non fixed-term licenses require an mrr value specified, whereas fixed-term licenses specify the value over the whole period.
mrr* | Number | The monthly license value (required if fixedPeriod is false)
value* | Number | The total license value for the whole subscription period, must be positive (required if fixedPeriod is true and no mrr value)
renewalStatus | String | Can take one of the following values: 'ongoing', 'renewed', 'lost'
autoRenews | Boolean | If set to true Planhat can automatically renew the subscription for you once the notice period is passed.
renewalPeriod | Number | Defines the length of the next license period if the subscription should be renewed. Defaults to 1, which would mean the license renews with a new 1-month period, regardless of the length of the current license period. If the renewalPeriod is not set, it will be assumed that the contract renews on the same period as previous. If the license isn't set to autorenew the renewalPeriod will only serve as guidance for the manual input.
noticePeriod | Number | Number of days before subscription end date that the license would have to be canceled not to automatically renew. Only makes sense if autoRenews is set to true. Defaults to 0.
custom | Object | A flexible object with custom data, one level. Add any custom data points on the form. Use short and human friendly names as keys. For example "Plan" or "Subscription Plan" is better than "SUBSCRIPTION_PLAN", as this is how it will be displayed in Planhat. Date, Strings, Boolean, Number are allowed types.

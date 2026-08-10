# Debugging subscriptions

To make the event issuer more self-service, the integration team has set up multiple solutions to allow for as much debugging as possible on the client side before a member of the integration team has to get involved. The main methods recommended for debugging by the integration team are:

- Verifying your permissions via `/userinfo`
- Checking Subscription Status and error reports

### Verifying permissions with /userinfo

If you are experiencing unexpected access errors, the first thing to check is whether your user has been granted the correct policies. Call the `/userinfo` endpoint (https://test.api.apps.banenor.no/event-issuer/v1/userinfo) with your APIM subscription key — it returns your tenant, display name, and the complete list of topics you are permitted to produce to and subscribe to. If the topic in question is missing from the response, contact the integration team to have the policy added.

## Debugging by checking subscription status

The Event Issuer has built-in Error Reporting linked to the GetSubscription methods. This means that if an error has occurred on your subscription, you can call the **GetSubscription** method which in case an error has occurred, will return an error report with useful debug information such as:

- A relevant Error Message
- A Trace ID for tracking the error in Grafana
- A HTTP status code
- A timestamp for when the error occurred

An example of how this response looks like:

```json
{
  "id": "01JYP5ZRMZVSAG6VCJB6C430F5",
  "applicationId": "Test-Subscription",
  "eventName": "cloud.unauthorized.topic",
  "url": "https://123spill.no/",
  "apiKeyHeader": "Ocp-Apim-Subscription-Key",
  "authentication": null,
  "workerStatus": "Failed",
  "createdAt": "2025-06-26T13:14:14.5600252Z",
  "updatedAt": "2025-06-27T12:02:13.7163623Z",
  "deletedAt": null,
  "errors": [
    {
      "traceID": "00-f913d58d1fe8dbb6cb0400ba4646b389-23cde68619bb145b-01",
      "errorMessage": "Not authorized for topic: cloud.unauthorized.topic",
      "httpStatusCode": null,
      "createdAt": "2025-06-27T12:02:13.7163623Z"
    }
  ]
}
```

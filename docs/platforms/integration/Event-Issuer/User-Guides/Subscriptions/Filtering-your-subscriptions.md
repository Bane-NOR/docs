---
tags:
- integration
- event-issuer
- guide
- paramters
- queries
- filter
- filtering
---

# Filtering your subscriptions

If you have a lot of subscriptions in Event Issuer, it can be difficult to get a quick overview
of subscriptions in different states. To help with this, you can add filters when querying for your
subscriptions.

Currently this is only available in test.

## Filter paramters

We currently provide these optional paramters to filter your subscriptions:

```txt
- topic
- workerStatus
- url
```

## How to filter your subscriptions

To filter your subscriptions simply add parameters when querying your subscriptions:

```txt
## Worker status query
https://<env>.api.apps.banenor.no/event-issuer/v1/<tenant>/subscriptions?workerStatus=Failed

## Topic query
https://<env>.api.apps.banenor.no/event-issuer/v1/<tenant>/subscriptions?topic=cloud.internal.event-issuer.test.v1

## Url query
https://<env>.api.apps.banenor.no/event-issuer/v1/<tenant>/subscriptions?url=https://bogus-url.com/event

## Combined query
https://<env>.api.apps.banenor.no/event-issuer/v1/<tenant>/subscriptions?workerStatus=Failed&topic=cloud.internal.event-issuer.test.v1
```

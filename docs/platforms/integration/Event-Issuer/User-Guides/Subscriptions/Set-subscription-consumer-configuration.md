# Consumer Configuration

We have opened up for the possitibility to customize Consumer Configurations for your subscriptions. Per now you can only set this when creating a new subscription, it is not possible to update the consumerConfig for an existing subscription. 
This is an optional field, and will default to default config if it is not set.

!!! info
    This is a new feature and is only available in the test and staging environment

## Allowed Consumer Configuration Settings

The following settings can be customized by end-users:

### Offset Management

**AutoOffsetReset**
- Type: Enum
- Allowed values: `Earliest`, `Latest`, `Error`
- Default: `Earliest`
- Description: What to do when there is no initial offset in Kafka or if the current offset does not exist anymore on the server
  - `Earliest`: Automatically reset offset to the earliest offset
  - `Latest`: Automatically reset offset to the latest offset
  - `Error`: Throw exception if no previous offset is found


#### New Request Format with Optional Config
```json
{
  "subscriptionId": "my-service-orders",
  "topics": ["events.orders"],
  "webhookUrl": "https://my-service.example.com/webhook",
  "consumerConfig": {
    "autoOffsetReset": "Latest"
  }
}
```


Contact the integration team if you wish to be able to set other consumerConfig properties for your subscriptions, and we will look into it.

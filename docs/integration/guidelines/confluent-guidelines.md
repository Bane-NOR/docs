# Confluent Guidelines

We are building a Confluent Platform to provide a pub/sub solution using a kafka broker with extended abilities to manage data contracts using schema registry. In order for Bane NOR to get value from the Confluent Platform, we see the need to work in new ways, collaborate more, to get a common language across domains, and to define some common ground we all adhere to. For this, we have defined some guidelines to ensure better use of the Confluent Platform and its based on the event-driven architecture principles.
The Confluent platform, out of the box, can be used in a variety of ways, and there is no simple "set of settings" which enforces our principles for our desired architecture approach. However, with these guidelines, we aim to help you get familiar with our thoughts for how to use the platform, what we currently support, and how you can get started with pub/sub on our Confluent Platform in Bane NOR.

!!! note "Intended Audience" 
    - `Developers`
    - `Architects`

## Privacy, security and sensitive data 

Producers of data to the Confluent Platform are responsible for adhering to requirements and laws that detail data quality, privacy and security. All sensitive data (e.g. personal data) and data which is protected by the Norwegian Security Act "Sikkerhetsloven" or the General Data Protection Regulation (GDPR), should not be shared on the Confluent Platform for now. Before we want to have such data on our platform, we must ensure that all requirements are met and that roles and responsibilities are fully understood.

## Topic Types

In the center for pub/sub mechanisms, such as the Confluent Platform, we find topics. Topics are the "veins" of pub/sub were all messages are communicated; and how we support, structure, implement and use these "veins" are very important to do properly in order to succeed. We want to work iteratively with good use cases to maximize learning and to fail fast and learn faster. 

- **Event Topics** - Represents exactly one Business Event type, typically with one logical producer and several consumers. Event Topics should use common Bane NOR data- and information models in order for events to give meaning across domains. Event Topic naming conventions are detailed below.
- **Internal Topics** - Represents topics used for communication within a domain, e.g. between microservices within one domain or between different deployment components for the same application. Internal Topics may not give meaning to the outside world of the domain context, thus teams are more free to decide what to put onto these topics. Also, service teams typically have control of both consumers and producers since this topic type is used within one domain.
- **Data Topics** - Represents edge-cases as this event type is rarely and carefully used in EDA. However, Data Topics is used to reflect the changes of master data. Naming conventions should follow master data object definitions which is lacking in Bane NOR. Also, contrary to Event Topics, the producers for Data Topics are usually also the owner of the referenced Master Data object type. 
- **Commands** - Represents the inversion of Event Topics, as they usually have many different producers and one logical consumers. Contrary to Event and Data topics, Command topics are usually associated with the receiving application, such as a e.g. mail-sender, and should rarely be used in Event-Driven Architecture. Instead, you should communicate business events and create triggers based on these.

## Topic Ownership

Each topic must be owned by exactly one entity. The ownership may change over time if an entity is changed or replaced. Also, there may be multiple (different) applications writing to the same topic (although this should be a rare exception, see below). Still, one owner (entity) must have the responsibility for the topic and coordinate changes with all producing applications. This owner is also the first contact for queries by consumers.

As outlined in the Event-Driven Architecture Principles, an Event topic must contain all instances of the Business Event type referenced by the topic name. So it is highly recommended that, even if two different applications seem to generate the same Business Event type, a discriminator is found to allow for separate topics for the two applications. Example: If one application receives all Online orders, and another one receives all Orders made by phone, consider introducing two topics, sales.OnlineOrderReceived and sales.PhoneOrderReceived. A third "application" (which may just be a script e.g. within Apache Nify) then could combine these two into a new topic, sales.OrderReceived; this would be perfectly valid within the Event-Driven Architecture.

Note that this pattern has the nice implication that the third "application" contains (owns) the "Business Logic" which event types make up the "combined" event type OrderReceived. When a new sale channel is introduced (of course, together with a new application providing a new Business Event type), all you have to do is adjust this application (maybe just a script). Applications subscribed to sales.OrderReceived will receive the orders from the new sale channel without any adjustment being necessary.

## Topic Subscription

To be able to read from a published topic, an application may subscribe to this topic. This does, in this context, not refer to the technical process of connecting to the Confluent platform and be informed about new messages, but means that an application must explicitly state that it wants to read from a given topic, for which, in turn, it gets the required Role Based Access Control (RBAC) entries in the Confluent platform to be able to do so.

This gives a high level of transparency of communication flows within the company, and gives Topic owners a hint if changes to a topic have impact on others or not (see also next section).

## Topic Lifecycle

Usually, a company has multiple Stages for their application landscape, e.g. `DEV`, `TEST`, and `PROD`. Following these guidelines, DevOps teams may only create and change their topics on the first of these stages. Every change can only be propagated to later stages following an automatic, reliable Staging process, ensuring that every consumer had a chance to test changes on an integration environment before it goes live. Also, this ensures that no changes are applied "on-the-fly" to a production environment (but  to lower stages) which are populated to the next "formal" staging.

Deletion of (published API) topics has to take the other direction, to ensure that no consumer loses a critical resource without prior notice. First of all, all subscribers (see previous section) have to cancel their subscription of the topic explicitly (which also means their associated ACLs are removed). This is an explicit statement that they do no longer rely on this resource. Once a topic in production stage does not have subscribers, it is eligible for deletion by the topic owner.

Would the deletion be propagated the other way, consumers could lose their critical resource in production as soon as the staging is performed, although they may have already adapted in an integration stage. There would be no way for the topic owners to "know" when it is safe to stage the topic deletion to production.

Of course, it seems impractical to get all consumers to "leave" the topic which the owner wants to delete. This is why topics must be marked as deprecated as soon as a deletion is desired. The deprecation mark must include an EOL date until which the topic is guaranteed to exist (the EOL date must, of course, give consumers enough time to adapt). Consumers must be notified automatically about the deprecation of a topic, and after the EOL date, topic owners are allowed to delete the topic even if there are still subscribers.

As a nice side effect, if consumers adapt faster than the EOL date, the topic still may be deleted before the EOL date if no more subscribers for the topic are registered.

## Topic naming convention
For Bane NOR we have defined the following guidelines for how to build a Topic Name. With this naming convention, we will have the flexibility in the future to support further domain and sub-domains, different event types, and to manage breaking changes (if absolutely needed) with versioning. The Topic name must contain the associated Business Capability and a descriptive name of the event, but not contain the name of the producing application(s).

Environment - Describes the origin of data e.g. cloud or on-premises
Domain - Describes the domain - for us in Bane NOR, we will have to learn what are top-level domains are
Sub-domain - Describes the sub-domain. This will be optional, filled with default values, but in the future we see the need to define a more granular detail level with regards to events happening within one domain into sub-domains. 
Topic type - Describes the type of topic, see above for details.
Event - Describes the actual event, e.g. train-arrived-at-station
Version - Describes the version of the topic. This will be used if there are any breaking changes on the topic, but it should rarely be used as we should do proper work with defining the required information related to events for the topic before we publish it.

Example:
```text

{cloud}.{operational}.{optional}.{event}.{train-arrived-at-station}.{v1}
\_____/ \___________/  \_______/  \____/  \_______________________/ \__/
   |          |            |         \               |                |                                
Environment Domain     Sub-domain    Topic type     Event          Version             
```

## Data Formats

The messages on each topic, especially Event Topics, should be easily consumable by a variety of both known and unknown consumers without having to add complex or special libraries like Apache Avro. As we assume that the number of business events per time is, in most cases, much lower than for technical use cases like IoT logs or similar, we think that the tradeoff of using JSON, an uncompressed text format, instead of byte-optimized Avro format, is acceptable. This is still valid for millions of messages per topic and day, as proven in practice. JSON allows consumers to process messages easily.

To make sure relevant information can be found in each message, the CloudEvents format **should** be used on Event and Command Topics. It should also be considered for Data topics, but here, the providers are free to choose their very own JSON format, if the properties of the CloudEvent specification do not match their needs.

The Payload of the CloudEvents structure must match a JSON Schema provided by the owning application of the Business Event. This JSON Schema must be made available to all (potential) consumers and may only change in a consumer-compatible way.

## Encryption / Authentication / Authorizaion

To ensure privacy and data security considerations, all communication with the Confluent Platform must be encrypted, i.e., use TLS encryption. Also, a reliable authentication mechanism must be used to assign each application exactly one identity (Kafka user name) on each cluster. Developers may receive special authentication tokens with limited associated rights and / or limited lifetime of the authentication token, e.g. for analyzing issues directly on the cluster with special tools.

The rights for each application must be configured explicitly using Confluent RBAC and LDAP. Every application must only have the rights required for accessing configured topics, e.g.

* Write access to owned topics
* Read access to subscribed topics
* Full access to the prefix for their internal topics
* Access to the prefix for their consumer group IDs.

## Cloud Events
We will detail the cloudevent.io specification further later.
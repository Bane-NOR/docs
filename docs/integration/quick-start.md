# Quick Start

Welcome to the quick start guide with Integration.

The purpose of this document is to present the most relevant information regrading where to start and to help find relevant information. The document is split up into sections depending on what  the role and audience whom want to start.

!!! warning
    The Quick start guide is work in progress

## Get started sharing and consuming data

In order for the integration platform to realize value and help us become more data-driven, we must all increase our understanding and ownership of our data and better understand how consumers data uses and needs. 

Below you find a short summary of how to get started. Note that the list is not complete, but is meant to get business owners and such started. For technical details, please read our more in-dept guidelines for APIM or Confluent Platform. Also, make sure to consult your solution design with experts before implementation for all use cases. If you have any questions, please contact us.

=== "Publish events"

    In order to publish events to the Confluent Platform, we have summarized some of the key activities you need to undertake.

    1. Define ownership and context of your event [read more about Event Storming(Missing link)]()
    2. Create topic on Confluent Platform [according to guidelines](/guidelines/kafka-guidelines/)
        - Define your topic using our template (pipeline will validate, and we will do QA before it is accepted)
        - Produce events according to data contract 
        - Evaluate the need for translating between domain specific and common data models
    3. Ensure your known consumers and your producers can communicate with Confluent
        - Communicate events directly or through facades with Confluent
        - Ensure network communication with Confluent
        - Always ensure changes are backwards compatible

=== "Consume events"
    
    In order to consume events from the Confluent Platform, we have summarized some of the key activities you need to undertake.

    1. Ask topic owner for permission to consume topic data
    2. Set up communication with Confluent
        - Ensure firewall openings to Confluent Platform
        - Consume events directly or through facades
        - Evaluate the need for translating between domain specific and common data models
    3. Ensure backend consumes events according to data contract
    4. Ensure error handling of event consumption

=== "Expose APIs"

    For now, contact the integreation team for exposing APIs. 

    1. Understand your consumers and create reusable APIs according to our [guidelines](guidelines/integration.md)
    2. Ensure your backend can be exposed through APIM
    3. Follow our requirements and provide us with [needed technical information](../integration/apim/getting-started/index.md#must-have-the-following-information-for-name-and-ownership-of-api) so that we can expose your APIs
    4. Ensure your consumers are able to consume your API

=== "Consume APIs"

    For now, contact the integration team for consuming APIs.

    - We can help you get access to APIs
    - You must consume APIs using our components for authentication and authorization

## Useful resources

You should check out the following resources before using APIs:

- The [Integration Strategy](strategy.md) describes the overall Strategy for the Integration Platform as a whole - why it exists and what capabilities it provides.
- [Terms & Definitions](terms-and-definitions.md) outlines relevant terminology that are used across all documentation.

## API Management

The following section describes relevant info regarding API Management.

### API Audience

We distinguish between 3 API Audiences:

| Term | Description |
| :--- | :--- |
| Internal | Internal APIs are available to clients within the company. Internal APIs are sometimes referred to as _private_ APIs. |
| Partner | Partner APIs are available to selected business partners. Partner APIs have consumers outside the company, and typically need other mechanisms for access control and client developer communication than internal APIs.  |
| Public | Public APIs are publicly available on the Internet. Public APIs can potentially have a high number of consumers. Access control mechanisms and consumer communication channels need to be scalable. |

### Access to API Management

This section describes how to get access. Please select your audience type:

=== "Internal"

    1. Service teams need to familiarize themselves and meet [certain requirements](apim/getting-started/index.md) before they can enroll in API Manager.
    2. If your service team meets the [requirements](apim/getting-started/index.md) mentioned in the previous step, check that you can log into the [api-portal](https://api-portal.banenor.no/) with your Bane NOR AD account.
    3. You will also have access to the [Azure Portal](https://portal.azure.com/) and can proceed with following the [HowTo Guides](apim/howto/manage-api-access-groups.md)

=== "Partner"

    [Maskinporten](https://samarbeid.digdir.no/maskinporten/maskinporten/25) is used to have a common platform to do authentication and authorization across companies for sharing data. Check out the [Maskinporten documentation](apim/security/maskinporten.md) to get started.

=== "Public"

    [Maskinporten](https://samarbeid.digdir.no/maskinporten/maskinporten/25) is used to have a common platform to do authentication and authorization across companies for sharing data. Check out the [Maskinporten documentation](apim/security/maskinporten.md) to get started.

### Guidelines for creating products in API Management

You now have access to the API Manager!

We want all Bane NOR APIs to appear as if the same author created them. Therefore, some guidelines have been created to support a uniform way to create APIs.

Guidelines:

- [Integration Guidelines](guidelines/integration.md) presents soft and hard technical requirements when it comes to creating products/APIs
- [Naming conventions](guidelines/naming.md) explains rules and best practices when it comes to naming products/APIs.
<!-- - TODO: Add to list: [Event Modeling](TODO: Page not ready yet, add reference later) --->

!!! warning

    The guidelines are important to follow in order to make Bane NOR APIs appear unified, however, some requirements might be stricter or looser based on what audience level you are!

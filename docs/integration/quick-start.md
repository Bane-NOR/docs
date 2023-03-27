# Quick Start
Welcome to our quick start guide for integrations. This site tries to abstracts details in our documentation to a small and tidy list. For a more detailed list, please read our guidelines. 

## Get started sharing and consuming data

In order for the integration platform to realize value and help us become more data-driven, we must all increase our understanding and ownership of our data and better understand how consumers data uses and needs. 

Below you find a short summary of how to get started. Note that the list is not complete, but is meant to get business owners and such started. For technical details, please read our more in-dept guidelines for APIM or Confluent Platform. Also, make sure to consult your solution design with experts before implementation for all use cases. If you have any questions, please contact us.

=== "Publish events"

    In order to publish events to the Confluent Platform, we have summarized some of the key activities you need to undertake.

    1. Define ownership and context of your event - [read more about Event Storming](https://www.eventstorming.com/)
    2. Create topic on Confluent Platform [according to guidelines](../guidelines/confluent-guidelines/)
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

    For now, contact the integration team for exposing APIs. 

    1. Understand your consumers and create reusable APIs according to our [guidelines](guidelines/integration.md)
    2. Ensure your backend can be exposed through APIM
    3. Follow our requirements and provide us with [needed technical information](../integration/apim/getting-started/index.md#must-have-the-following-information-for-name-and-ownership-of-api) so that we can expose your APIs
    4. Ensure your consumers are able to consume your API

=== "Consume APIs"

    For now, contact the integration team for consuming APIs.

    - We can help you get access to APIs
    - You must consume APIs using our components for authentication and authorization
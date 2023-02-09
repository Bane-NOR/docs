# Quick Start

Welcome to the quick start guide with Integration.

The purpose of this document is to present the most relevant information regrading where to start and to help find relevant information. The document is split up into sections depending on what  the role and audience whom want to start.

!!! warning
    The Quick start guide is a work in progress, where internal development is in focus

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

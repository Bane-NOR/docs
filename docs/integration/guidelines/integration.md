# Integration Guidelines

Bane NOR's software is moving towards a decoupled microservice oriented architecture. The main part of going towards this is by using Event Driven Architecture with support for RESTful APIs. We have different service teams that own, deploy and operate these services.

APIs and events express what our systems do, and are therefore highly valuable business assets. Designing high-quality, long-lasting APIs and events has become even more critical for us since we need to share data between many systems and with external parties. Our strategy emphasizes developing APIs and events for our external business partners to use via third-party integrations and applications.

With this in mind, we've adopted API First as one of our key engineering principles. Microservice development begins with API definition outside the code and ideally involves ample peer review feedback to achieve high-quality APIs. API First encompasses a set of quality-related standards and fosters a peer review culture including a lightweight review procedure. We encourage our teams to follow them to ensure that our APIs:

- are easy to understand and learn
- are general and abstracted from specific implementation and use cases
- are robust and easy to use
- have a common look and feel
- are consistent with other teams’ APIs and our global architecture

!!! tip

    Ideally, all Bane NOR APIs will appear as if the same author created them.


## Conventions used in these guidelines

The requirement level keywords <span class="rfc-must">"MUST"</span>, "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", <span class="rfc-should">"SHOULD"</span>, "SHOULD NOT", "RECOMMENDED", <span class="rfc-may">"MAY"</span>, and "OPTIONAL" used in this document (case insensitive) are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

!!! note "Intended Audience" 
    - `Developers`
    - `Architects`

## API First principle

API First is one of our engineering and architectural principles. In a nutshell, API First requires two aspects:

1. Define APIs first, before coding its implementation, using a standard specification language.
2. Get early review feedback from peers and client developers.

By defining APIs outside the code, we want to facilitate early review feedback and also a development discipline that focuses service interface design on the following:

- profound understanding of the domain and required functionality
- generalized business entities/resources, i.e. avoidance of use case specific APIs
- clear separation of WHAT vs. HOW concerns, i.e. abstraction from implementation aspects — APIs should be stable even if we replace complete service implementation including its underlying technology stack

Moreover, API definitions with standardized specification format also facilitate:

- single source of truth for the API specification - it is a crucial part of a contract between the service provider and client users
- infrastructure tooling for API discovery, API GUIs, API documents, and automated quality checks

It is important to note, that API First is **not in conflict with the agile development principles**. Service applications should evolve incrementally — and so should their APIs. Our API specification will evolve iteratively in cycles, each starting with draft status and early team and peer review feedback. API may change and profit from implementation concerns and automated testing feedback. API evolution during the development life cycle may include breaking changes for features not yet in production and as long as we have aligned the changes with the clients. Hence, API First does not mean that you must have 100% domain and requirement understanding and can never produce code before you have defined the complete API and had it confirmed by peer review.

On the other hand, API First conflicts with the bad practice of publishing API definitions and asking for peer review after the service integration or even the service productive operation has started. It is crucial to request and obtain feedback, as early as possible, but not before the API changes are comprehensive, have a clear focus on the next evolutionary step, and have a certain quality (including API Guideline compliance) already confirmed via team internal reviews.

## Table of Contents

This document contains the following:

- [1. General Guidelines](#1-general-guidelines)
- [2. REST Guidelines](#2-rest-guidelines)
- [3. REST Meta Information](#3-rest-meta-information)
- [4. REST Security](#4-rest-security)
- [5. Data Formats](#5-data-formats)
- [6. URLs](#6-urls)
- [7. HTTP Status Codes](#7-http-status-codes)
- [8. HTTP Headers](#8-http-headers)

## 1. General Guidelines

### <span class="rfc-must">MUST</span> follow API First principle

APIs must follow the API First Principle, more specifically:

- define APIs first, before coding its implementation
- design your APIs in compliance with these guidelines
- call for early review feedback from peers and client developers

### <span class="rfc-must">MUST</span> provide API specification using OpenAPI

We use the [OpenAPI specification](https://swagger.io/specification/) as a standard to define API specification files. API designers are required to provide the API specification using a single self-contained YAML file to improve readability. We encourage the use of OpenAPI version 3.0, but we still support OpenAPI 2.0 (a.k.a. Swagger 2).

The API specification files should be subject to version control using a source code management system - together with the implementing sources.

We publish the component external API specification with the deployment of the implementing service, and, hence, make it discoverable for the group audience via our [API Portal](https://api-portal.banenor.no/).

A good way to explore OpenAPI 3.0/2.0 is to navigate through the [OpenAPI specification mind map](https://openapi-map.apihandyman.io/).

### <span class="rfc-must">MUST</span> write APIs using English (US)

### <span class="rfc-should">SHOULD</span> provide API user manual

In addition to the API Specification, it is good practice to provide an API user manual to improve client developers' experience, especially those that are less familiar with the given API. A helpful API user manual typically describes the following API aspects:

- API scope, purpose, and use cases
- concrete examples of API usage
- edge cases, error situation details, and repair hints
- architecture context and major dependencies - including figures and sequence flows

The user manual must be published online, e.g. via our documentation hosting platform service, or specific team web servers. Please do not forget to include a link to the API user manual into the API specification using the `#/externalDocs/url property`.

### <span class="rfc-must">MUST</span> only use durable and immutable remote references

Normally, API specification files must be self-contained, i.e. files should not contain references to local or remote content. The reason is, that the content referred to is *in general* **not durable** and **not immutable**. As a consequence, the semantic of an API may change in unexpected ways. (For example, the second link is already outdated due to code restructuring.)

### <span class="rfc-must">MUST</span> use content validation on data

Reduce the likelihood of injection attacks by validating and sanitizing API inputs. Examples of such measures include type and format checks, length and range checks, JSON or XML schema validation, file upload validation, etc.

### <span class="rfc-must">MUST</span> use semantic versioning

To share a common semantic of version information we expect API designers to comply to [Semantic Versioning 2.0](https://semver.org/spec/v2.0.0.html) rules 1 to 8 and 11 restricted to the format `<MAJOR>.<MINOR>.<PATCH>` for versions as follows:

- Increment the MAJOR version when you make incompatible API changes after having aligned the changes with consumers,
- Increment the MINOR version when you add new functionality in a backwards-compatible manner, and
- Optionally increment the PATCH version when you make backwards-compatible bug fixes or editorial changes not affecting the functionality.

## 2. REST Guidelines

### <span class="rfc-must">MUST</span> use Azure API Manager for external integration points

Service teams that need to grant external parties access to their APIs, must use API Manager, which is configured and put into production by Bane NOR, to achieve this.

### <span class="rfc-must">MUST</span> use policy control for external access to APIs in Azure API Manager

API Manager provides API policies configuration spread across inbound, backend, outbound, and on-error sections. Furthermore, API Management allows us to define policies at the following scopes, from most broad to most narrow:

- Global (all APIs)
- Product (APIs associated with a selected product)
- API (all operations in an API)
- Operation (single operation in an API)

Global and product scope policies are determined by the integration team while API specific policies can be delivered by service teams as well. <!-- TODO - Add: To see the complete list of currently imposed policies follow this link -->

Those are the policies imposed on APIs exposed by Bane NOR service teams, and also the policies that all APIs exposed by Bane NOR must abide by.

### <span class="rfc-must">MUST</span> version APIs

Handling changes to APIs without breaking client integration can be a challenge. There is no industry standard approach to versioning REST APIs, and API versioning can come at a significant cost. Supporting multiple versions of an API can make development, testing, and operation of the API much more complex.

API Manager supports the URI versioning scheme and has been selected as the main versioning of API's with addition to revisioning that can be added as a query parameter.

#### URI Versioning

In URI versioning, the API version is stated as part of the URI. E.g: `https://api.banenor.no/customer-info/v1/estimated-timetable-delivery`. This is an easy-to-understand and commonly used approach. The version number is explicitly stated in the URI, which makes it clear and concise to the user. Removing versions will break integration for clients that have not yet migrated, thus resulting in an explicit (rather than silent) failure. With URI versioning, the URI is no longer a unique address to the resource, but rather a version of the resource, thus violating the principle that a URI should refer to a unique resource.

#### OpenAPI Specification

OpenAPI allows to specify the API specification version in #/info/version. This should be versioned with semantic versioning as described in [here](#must-use-semantic-versioning).

- Pre-release versions (rule 9) and build metadata (rule 10) must not be used in API version information.
- While patch versions are useful for fixing typos etc, API designers are free to decide whether they increment it or not.
- API designers should consider to use API version 0.y.z (rule 4) for initial API design.

#### Example:

``` yaml
openapi: 3.0.1
info:
    title: Parcel Service API
    description: API for <...>
    version: 1.3.7
    <...>
```

## 3. REST Meta Information

### <span class="rfc-must">MUST</span> contain API meta information

API specifications must contain the following OpenAPI meta information to allow for API management:

- `#/info/title` as (unique) identifying, functional descriptive name of the API
- `#/info/version` to distinguish API specifications versions following [semantic rules](#must-use-semantic-versioning)
- `#/info/description` containing a proper description of the API
- `#/info/contact/{name,url,email}` containing the responsible team
- `#/info/x-audience` intended target audience of the API as described in [API audience](#must-provide-api-audience)

### <span class="rfc-must">MUST</span> provide API audience

Each API must be classified with respect to the intended target audience supposed to consume the API, to facilitate differentiated standards on APIs for discoverability, changeability, quality of design and documentation, as well as permission granting. We differentiate the following API audience groups with clear organizational and legal boundaries:

| **Audience group name** |  **Explanation**                                                                                                                               |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| internal                | Internal APIs are available to clients within the company. Internal APIs are sometimes referred to as _private_ APIs.                          |
| partner                 | Partner APIs are available to selected business partners. Partner APIs have consumers outside the company, and typically need other mechanisms for access control and client developer communication than internal APIs. |
| public                  | Public APIs are publicly available on the Internet. Public APIs can potentially have a high number of consumers. Access control mechanisms and consumer communication channels need to be scalable.                                                                      |

*Note:* a smaller audience group is intentionally included in the wider group and thus does not need to be declared additionally.

The API audience is provided as API meta information in the `info`-block of the OpenAPI specification and must conform to the following specification:

``` yaml
/info/x-audience:
  type: string
  x-extensible-enum:
    - internal
    - partner
    - public
  description: |
    Intended target audience of the API. Relevant for standards around
    quality of design and documentation, reviews, discoverability,
    changeability, and permission granting.
```

!!! hint
    Exactly one audience per API specification is allowed.

### <span class="rfc-must">MUST</span>/<span class="rfc-may">MAY</span> use functional naming schema

Functional naming is a powerful, yet easy way to align global resources as host, permission, and event names within an application landscape. It helps to preserve uniqueness of names while giving readers meaningful context information about the addressed component. Besides, the most important aspect is, that it allows to keep APIs stable in the case of technical and organizational changes.

A unique functional-name is assigned to each functional component serving an API. It is built of the domain name of the functional group the component is belonging to and a unique a short identifier for the functional component itself:

```
<functional-name>      ::= <functional-domain>-<functional-component>
<functional-domain>    ::= [a-z][a-z0-9-]* -- managed functional group of components
<functional-component> ::= [a-z][a-z0-9-]* -- name of API owning functional component
```

Depending on the API audience, you must/should/may follow the functional naming schema for hostnames and event names (and also permission names, in future) as follows:

| **Functional name**                    | **Audience**    |
|----------------------------------------|-----------------|
| <span class="rfc-must">MUST</span>     | public, partner |
| <span class="rfc-may">MAY</span>       | internal        |

## 4. REST Security

### <span class="rfc-must">MUST</span> secure endpoints

Every API endpoint must be protected and armed with authentication and authorization. As part of the API definition you must specify how you protect your API using either the `http` typed `bearer` or `oauth2` typed security schemes defined in the [OpenAPI Authentication Specification](https://swagger.io/docs/specification/authentication/).

!!! warning
    Do not use OpenAPI oauth2 typed security scheme flows (e.g. implicit) if your service does not fully support it and implements a simple bearer token scheme, because it exposes authentication server address details and may make use of redirection.

### <span class="rfc-must">MUST</span> define and assign permissions (scopes)

APIs must define permissions to protect their resources. Thus, at least one permission must be assigned to each API endpoint. Please refer to [<span class="rfc-must">MUST</span>/<span class="rfc-may">MAY</span> follow naming convention for permissions (scopes)](#mustmay-use-functional-naming-schema) for designing permission names and see the following examples.

<!-- TODO: Add example table? -->

!!! note 
    APIs should stick to component specific permissions without resource extension to avoid the complexity of too many fine grained permissions. For the majority of use cases, restricting access for specific API endpoints using read or write is sufficient.

Following a minimal a minimal API specification approach, the Authorization-header does not need to be defined on each API endpoint, since it is required and so to say implicitly defined via the security section.

## 5. Data Formats

### <span class="rfc-must">MUST</span> use standard data formats

[Open API](https://github.com/OAI/OpenAPI-Specification) (based on [JSON Schema Validation vocabulary](https://tools.ietf.org/html/draft-bhutton-json-schema-validation-00#section-7.3)) defines formats from ISO and IETF standards for date/time, integers/numbers and binary data. You must use these formats, whenever applicable. Check out the [Open API Data Types documentation](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md#data-types) for more details.

<!-- TODO: Add ref to Bane NOR Business data formats when ready! -->

### <span class="rfc-must">MUST</span> use standard formats for date and time properties

As a specific case of [<span class="rfc-must">MUST</span> use standard data formats](#must-use-standard-data-formats), you must use the `string` typed formats `date`, `date-time`, `time`, `duration`, or `period` for the definition of date and time properties. The formats are based on the standard [RFC 3339](https://tools.ietf.org/html/rfc3339) internet profile  - a subset of [ISO 8601](https://tools.ietf.org/html/rfc3339#ref-ISO8601).

### <span class="rfc-must">MUST</span> use content negotiation, if clients may choose from different resource representations

In some situations the API supports serving different representations of a specific resource (at the same URL), e.g. JSON, PDF, TEXT, or HTML representations for an invoice resource. You should use [content negotiation](https://en.wikipedia.org/wiki/Content_negotiation) to support clients specifying via the standard HTTP headers `Accept`, `Accept-Language`, `Accept-Encoding` which representation is best suited for their use case, for example, which language of a document, representation / content format, or content encoding. You **must** use standard media types like `application/json` or `application/xml` for defining the content format in the `Accept` header.

## 6. URLs

Guidelines for naming and designing resource paths and query parameters.

### <span class="rfc-must">MUST</span>/<span class="rfc-may">MAY</span> not use `/api` as base path

In most cases, all resources provided by a service are part of the public API, and therefore should be made available under the root "/" base path.

We see API’s base path as a part of deployment variant configuration. Therefore, this information has to be declared in the [server object](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.2.md#server-object).

For more information see [naming guidelines](naming.md).

| **Functional name**                    | **Audience**                      |
|----------------------------------------|-----------------------------------|
| <span class="rfc-must">MUST</span>     | external-public, external-partner |
| <span class="rfc-may">MAY</span>       | internal, internal-external       |

### <span class="rfc-must">MUST</span> use URL-friendly resource identifiers

To simplify encoding of resource IDs in URLs they must match the regex `[a-zA-Z0-9:.\-]*`. Resource IDs only consist of ASCII strings using letters, numbers, minus, colon and period.

### <span class="rfc-must">MUST</span> use kebab-case for path segments

Path segments are restricted to ASCII kebab-case strings matching regex `^[a-z][a-z\-0-9]*$`. The first character must be a lower case letter, and subsequent characters can be a letter, or a dash(-), or a number.

Example:
```
/customer-info/{id}
```
Kebab-case applies to concrete path segments and not necessarily the names of path parameters.

### <span class="rfc-must">MUST</span> pluralize resource names

Usually, a collection of resource instances is provided (at least the API should be ready here). The special case of a resource singleton must be modeled as a collection with cardinality 1 including definition of `max-items` = `min-items` = 1 for the returned `array` structure to make the cardinality constraint explicit.

### <span class="rfc-may">MAY</span> consider the use of nouns and verbs when designing APIs

A good practice when **designing** endpoints is to use nouns frequently. 

### <span class="rfc-must">MUST</span> keep URLs verb-free - use nouns (actions)

The API describes resources, so the only place where actions should appear is in the HTTP methods. In URLs, use only [nouns](#span-stylecolorredmustspan-use-semantic-versioning-202). Instead of thinking of actions (verbs), it’s often helpful to think about putting a message in a letter box: e.g., instead of having the verb cancel in the url, think of sending a message to cancel an order to the _cancellations_ letter box on the server side.

The HTTP verbs `POST, GET, PUT, DELETE` already describes what kind of action is to be performed on a specific resource - therefore no need to specify actions.

For instance, if an application has to lock articles explicitly so that only one user may edit them, create an article lock with `PUT` or `POST`. The added benefit is that you already have a service for browsing and filtering article locks. Example:
```
PUT /article-locks/{article-id}
```

### <span class="rfc-must">MUST</span> use domain-specific resource names

API resources represent elements of the application’s domain model. Using domain-specific nomenclature for resource names helps developers to understand the functionality and basic semantics of your resources. It also reduces the need for further documentation outside the API definition. For example, "sales-order-items" is superior to "order-items" in that it clearly indicates which business object it represents. Along these lines, "items" is too general.

### <span class="rfc-must">MUST</span> identify resources and sub-resources via path segments

Some API resources may contain or reference sub-resources. Embedded sub-resources, which are not top-level resources, are parts of a higher-level resource and cannot be used outside of its scope. Sub-resources should be referenced by their name and identifier in the path segments as follows:

```/resources/{resource-id}/sub-resources/{sub-resource-id}```

In order to improve the consumer experience, you should aim for intuitively understandable URLs, where each sub-path is a valid reference to a resource or a set of resources. For instance, if `/partners/{partner-id}/addresses/{address-id}` is valid, then, in principle, also `/partners/{partner-id}/addresses`, `/partners/{partner-id}` and `/partners` must be valid. Examples of concrete url paths:

```
/shopping-carts/de:1681e6b88ec1/items/1
/shopping-carts/de:1681e6b88ec1
/content/images/9cacb4d8
/content/images
```

Note: resource identifiers may be build of multiple other resource identifiers (see MAY expose compound keys as resource identifiers).

Exception: In some situations the resource identifier is not passed as a path segment but via the authorization information, e.g. an authorization token or session cookie. Here, it is reasonable to use self as pseudo-identifier path segment. For instance, you may define /employees/self or /employees/self/personal-details as resource paths —  and may additionally define endpoints that support identifier passing in the resource path, like define /employees/{empl-id} or /employees/{empl-id}/personal-details.

### <span class="rfc-should">SHOULD</span> limit number of resource types
To keep maintenance and service evolution manageable, we should follow "functional segmentation" and "separation of concern" design principles and do not mix different business functionalities in same API definition. In practice this means that the number of resource types exposed via an API should be limited. In this context a resource type is defined as a set of highly related resources such as a collection, its members and any direct sub-resources.

For example, the resources below would be counted as three resource types, one for customers, one for the addresses, and one for the customers' related addresses:

```
/customers
/customers/{id}
/customers/{id}/preferences
/customers/{id}/addresses
/customers/{id}/addresses/{addr}
/addresses
/addresses/{addr}
```

Note that:

- We consider `/customers/id/preferences` part of the `/customers` resource type because it has a one-to-one relation to the customer without an additional identifier.
- We consider `/customers` and `/customers/id/addresses` as separate resource types because `/customers/id/addresses/{addr}` also exists with an additional identifier for the address.
- We consider `/addresses` and `/customers/id/addresses` as separate resource types because there’s no reliable way to be sure they are the same.

Given this definition, our experience is that well defined APIs involve no more than 4 to 8 resource types. There may be exceptions with more complex business domains that require more resources, but you should first check if you can split them into separate subdomains with distinct APIs.

Nevertheless one API should hold all necessary resources to model complete business processes helping clients to understand these flows.

### <span class="rfc-should">SHOULD</span> limit number of sub-resource levels

There are main resources (with root url paths) and sub-resources (or *nested* resources with non-root urls paths). Use sub-resources if their life cycle is (loosely) coupled to the main resource, i.e. the main resource works as collection resource of the subresource entities. You should use ⇐ 3 sub-resource (nesting) levels — more levels increase API complexity and url path length. (Remember, some popular web browsers do not support URLs of more than 2000 characters.)

## 7. HTTP Status Codes

### <span class="rfc-must">MUST</span> use official HTTP status codes
You must only use official HTTP status codes consistently with their intended semantics. Official HTTP status codes are defined via RFC standards and registered in the [IANA Status Code Registry](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml). Main RFC standards are [RFC7231 - HTTP/1.1: Semantics](https://tools.ietf.org/html/rfc7231#section-6) (or [RFC7235 - HTTP/1.1: Authentication](https://tools.ietf.org/html/rfc7235#page-6)) and [RFC 6585 - HTTP: Additional Status Codes](https://tools.ietf.org/html/rfc6585) (and there are upcoming new ones, e.g. [draft legally-restricted-status](https://tools.ietf.org/html/draft-tbray-http-legally-restricted-status-05)). An overview on the official error codes provides [Wikipedia: HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) (which also lists some unofficial status codes, e.g. defined by popular web servers like Nginx, that we do not suggest to use).

### <span class="rfc-must">MUST</span> specify success and error responses
APIs should define the functional, business view and abstract from implementation aspects. Success and error responses are a vital part to define how an API is used correctly.

Therefore, you must define **all** success and service specific error responses in your API specification. Both are part of the interface definition and provide important information for service clients to handle standard as well as exceptional situations. Error code response descriptions should provide information about the specific conditions that lead to the error, especially if these conditions can be changed by how the endpoint is used by the clients.

API designers should also think about a **troubleshooting board** as part of the associated online API documentation. It provides information and handling guidance on application-specific errors and is referenced via links from the API specification. This can reduce service support tasks and contribute to service client and provider performance.

**Exception:** Standard errors, especially for client side error codes like 401 (unauthenticated), 403 (unauthorized) or 404 (not found) that can be inferred straightforwardly from the specific endpoint definition need not to be individually defined. Instead you can combine multiple error response specifications with the default pattern below. However, you should not use it and explicitly define the error code as soon as it provides endpoint specific indications for clients of how to avoid calling the endpoint in the wrong way, or be prepared to react on specific error situation.

### <span class="rfc-should">SHOULD</span> only use most common HTTP status codes
The most commonly used codes are best understood and listed below as subset of the [official HTTP status codes](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml#http-status-codes-1) and consistent with their semantics in the RFCs. We avoid less commonly used codes that easily create misconceptions due to less familiar semantics and API specific interpretations.

**Important:** As long as your HTTP status code usage is well covered by the semantic defined here, you should not describe it to avoid an overload with common sense information and the risk of inconsistent definitions. Only if the HTTP status code is not in the list below or its usage requires additional information aside the well defined semantic, the API specification must provide a clear description of the HTTP status code in the response.


### <span class="rfc-must">MUST</span> use most specific HTTP status codes
You must use the most specific HTTP status code when returning information about your request processing status or error situations.

### <span class="rfc-must">MUST</span> not expose stack traces 
Stack traces contain implementation details that are not part of an API, and on which clients should never rely. Moreover, stack traces can leak sensitive information that partners and third parties are not allowed to receive and may disclose insights about vulnerabilities to attackers.


## 8. HTTP Headers

We describe a handful of standard HTTP headers, which we found raising the most questions in our daily usage, or which are useful in particular circumstances but not widely known.

Though we generally discourage usage of proprietary headers, they are useful to pass generic, service independent, overarching information relevant for our specific application architecture. We consistently define these proprietary headers in this section below. Whether services support these concerns or not is optional. Therefore, the OpenAPI API specification is the right place to make this explicitly visible — use the parameter definitions of the resource HTTP methods.

### <span class="rfc-may">MAY</span> use standard headers
Use [this list](http://en.wikipedia.org/wiki/List_of_HTTP_header_fields) and explicitly mention its support in your OpenAPI definition.

### <span class="rfc-should">SHOULD</span> use kebab-case with uppercase separate words for HTTP headers

This convention is followed by (most of) the standard headers e.g. as defined in [RFC 2616](https://tools.ietf.org/html/rfc2616) and [RFC 4229](https://tools.ietf.org/html/rfc4229). Examples:

```
If-Modified-Since
Accept-Encoding
Content-ID
Language
```

Note, HTTP standard defines headers as case-insensitive ([RFC 7230, p.22](https://tools.ietf.org/html/rfc7230#page-22)). However, for sake of readability and consistency you should follow the convention when using standard or proprietary headers. Exceptions are common abbreviations like `ID`.


## Publications, specifications, and standards

- [RFC 3339](https://tools.ietf.org/html/rfc3339): Date and Time on the Internet: Timestamps
- [RFC 4122](https://tools.ietf.org/html/rfc4122): A Universally Unique IDentifier (UUID) URN Namespace
- [RFC 4627](https://tools.ietf.org/html/rfc4627): The application/json Media Type for JavaScript Object Notation (JSON)
- [RFC 8288](https://tools.ietf.org/html/rfc8288): Web Linking
- [RFC 6585](https://tools.ietf.org/html/rfc6585): Additional HTTP Status Codes
- [RFC 6902](https://tools.ietf.org/html/rfc6902): JavaScript Object Notation (JSON) Patch
- [RFC 7159](https://tools.ietf.org/html/rfc7159): The JavaScript Object Notation (JSON) Data Interchange Format
- [RFC 7230](https://tools.ietf.org/html/rfc7230): Hypertext Transfer Protocol (HTTP/1.1): Message Syntax and Routing
- [RFC 7231](https://tools.ietf.org/html/rfc7231): Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content
- [RFC 7232](https://tools.ietf.org/html/rfc7232): Hypertext Transfer Protocol (HTTP/1.1): Conditional Requests
- [RFC 7233](https://tools.ietf.org/html/rfc7233): Hypertext Transfer Protocol (HTTP/1.1): Range Requests
- [RFC 7234](https://tools.ietf.org/html/rfc7234): Hypertext Transfer Protocol (HTTP/1.1): Caching
- [RFC 7240](https://tools.ietf.org/html/rfc7240): Prefer Header for HTTP
- [RFC 7396](https://tools.ietf.org/html/rfc7396): JSON Merge Patch
- [RFC 7807](https://tools.ietf.org/html/rfc7807): Problem Details for HTTP APIs
- [RFC 4648](https://tools.ietf.org/html/rfc4648): The Base16, Base32, and Base64 Data Encodings
- [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601): Date and time format

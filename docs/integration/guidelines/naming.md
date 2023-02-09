# Naming conventions

!!! note "Intended Audience"
    - `Architects`
    - `Developers`

1. Prefer simple, concise, and intuitive names
2. Prefer lower-case, hyphen-separated names
3. Prefer single-word names
4. Prefer 7-bit ascii

## URI Components Names

URIs follow [RFC 3986](https://tools.ietf.org/html/rfc3986) specification. This specification simplifies REST API service development and consumption.

The guidelines in this section govern your URI structure and semantics following the [RFC 3986](https://tools.ietf.org/html/rfc3986) constraints.

> [RFC 3986](https://tools.ietf.org/html/rfc3986): the scheme and host are case-insensitive and therefore should be normalized to lowercase. For example, the URI `HTTP://www.EXAMPLE.com/` is equivalent to `http://www.example.com/`. The other generic syntax components are assumed to be case-sensitive unless specifically defined otherwise by the scheme

> [RFC 2616](https://tools.ietf.org/html/rfc2616): When comparing two URIs to decide if they match or not, a client **should** use a case-sensitive octet-by-octet comparison of the entire URIs, with these exceptions:

> [RFC 7230](https://tools.ietf.org/html/rfc7230): The scheme and host are case-insensitive and normally provided in lowercase; all other components are compared in a case-sensitive manner.

### URI Components

Simple and concise names are easy to remember, easy to talk about, and aid communication. URLs **should** avoid non-standard abbreviations and avoid overloading names - prefer different names for different concepts.

Some name reuse is unavoidable, in which case there should be sufficient _context_ available for the concept itself to become unambiguous.

When deciding on a name, look to familiar terminology, and consider both established terms and concepts in computer programming, in addition to the domain.

URLs **must** follow the standard naming convention as described below:

```text
https://api.banenor.no/{domain-name}/{api-name}/v2/{endpoint}?fields=start-date,end-date
\___/   \____________/\_____________________________________/\____________________________/
  |           |                          |                                  |
 scheme   authority                     path                              query
```

### URI Naming Conventions

URLs **must** follow the standard naming convention as described below:

- the URI **must** be specified in all lower case
- only hyphens ‘-‘ can be used to separate words or path parameters for readability (no spaces or underscores)
- camelCase **may** be used to separate words in query parameter names but not as part of the base URI

The following table provides a breakdown of how to construct the API URI.

| URI Element               | Description                                                                                                            | Example                                                                                                                                                                  |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Protocol                  | All APIs **must** be exposed using HTTPS.                                                                              | `https:\\`                                                                                                                                                               |
| Authority > Environment   | The domain of where the API endpoint will be exposed. Refer to the ‘DNS’ standard section for details.                 | `api.banenor.no`                                                                                                                                                         |
| Path > API                | The API name which is derived from the business domain.                                                                | e.g. `/domain-name/api-name/v1/endpoints` Any service owner/department can specify the API name that they would like to expose their services on.                        |
| Path > Version            | The version of the API that is desired to be accessed by the consumer.                                                 | e.g. `/v1` All APIs must specify a version that follow the versioning scheme as specified in ‘versioning’ below. This is part of the API Management configuration        |
| Path > Endpoint           | The endpoint identifies a list of resources. The endpoint **must** be named using the plural representation of a noun. | e.g. As part of the workforce API - a resource could be a list of `employees`                                                                                            |
| Path > Resource           | The resource identifier which corresponds to an instance of the resource.                                              | e.g. As part of the **api-name** - if there was a specific document with id E13454. These details can be retrieved using `GET /domain-name/api-name/v1/documents/E13454` |
| Query String > Parameters | Query parameters **must not** be used to transport payload or actual data.                                             |                                                                                                                                                                          |

## Domains & API names

Domain names refer to the Bane NOR domain where APIs belongs to, such as `Customer Info`. 'Domain' names **must** be consistently used by APIs, UIs, documentation, Terms of Service, etc. Bane NOR APIs **must** use domain names approved by the `service owners`.

The table below shows examples of related product name and their consistency. See further below on this page for more details on the respective names and their conventions.

| API Name     | Example                                                                   |
| ------------ | ------------------------------------------------------------------------- |
| Product Name | Customer Info                                                             |
| Audience     | external-public                                                           |
| API Name     | siri                                                                      |
| API Version  | v3                                                                        |
| URL          | https://api.banenor.no/customer-info/siri/v3/estimated-timetable-delivery |

For all types of audiences see [integration guidelines](integration.md).

## Lower-case, hyphen-separated

URLs are `case sensitive`, except for the host component, but in practice a lot of systems are still case insensitive (like IIS and the Windows file system).

To help reduce friction, prefer lower-case hyphen-separated names, both in URLs and possibly fields. Consider a function _get infrastructure location-info_; assuming case insensitivity and the option of camelcase, pascalcase, and hyphenation:

```text
https://api.banenor.no/customer-info/siri/v3/estimated-timetable-delivery
https://api.banenor.no/customer-info/siri/v1/EstimatedTimetableDelivery
https://api.banenor.no/customer-info/siri/v2/estimatedtimetabledelivery
```

The latter could be the same function, and they could not be - it depends on client software, and sets up for miscommunication. Additionally, it's slightly harder to make out different words since there is no horizontal space at all.

The hyphen (\-) is preferred over the underscore (\_), since the underscore in some fonts and renderers is difficult to separate from the whitespace, and leaves no room for doubt. The hyphen is well suited as a word separator, because it does not have to be [escaped](http://www.ietf.org/rfc/rfc1738.txt) in an URL.

> [RFC 1738](http://www.ietf.org/rfc/rfc1738.txt): Scheme names consist of a sequence of characters. The lower case letters "a"--"z", digits, and the characters plus ("+"), period ("."), and hyphen ("-") are allowed.

## 7-bit ASCII

While in particular useful for identifiers, prefer pure 7-bit ASCII whenever possible, i.e. try and avoid language-specific letters whenever you can. While all clients and servers should be UTF-8 (or in general encoding) aware and robust, a lot of potential friction is reduced when sticking to 7-bit ASCII.

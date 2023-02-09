# API Security

Bane NOR has different ways of securing their APIs depending on the intended audience. As seen in the [guidelines](../../guidelines/integration.md) Bane NOR as found audiences that APIs belongs in under.

- Open Data which should be available for the public audience needs to create an account and subscribe to the API
- Business Partners uses `Maskinporten` to authenticate and authorize to the APIs available
- Internal APIs are protected by use of the internal Identity Provider and as such are only available internally

## Open Data

Bane NOR has an `open data` [policy](https://banenor.no/Om-oss/Apne-data-fra-Bane-NOR/) which means that APIs are available to the public. These APIs are available in the Open Data APIM product.

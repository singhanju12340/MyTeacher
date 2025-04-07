---
Creation Time: Tuesday, February 25th 2025
Modified Time: Wednesday, February 26th 2025
---
### API structure 
_domain/api/root/version/resources/path

>resources should be noun
>path should be verb

_version/resources/action

Change version when API contract is changing

GET api caching is done based on url parameters, thats the reason we should not have get api with request body. as caching take only url params not request body.

### Gradle standards

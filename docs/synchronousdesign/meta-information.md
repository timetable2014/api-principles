---
layout: default
title: Meta Information
parent: API Design
nav_order: 15
---

{MUST} Contain API Meta Information
===================================

API specifications must contain the following OpenAPI meta information to allow for API management:

-   `#/info/title` as (unique) identifying, functional descriptive name of the API

-   `#/info/version` to distinguish API specifications versions following [semantic rules](#116)

-   `#/info/description` containing a proper description of the API

-   `#/info/contact/{name,url,email}` containing the responsible team

Following OpenAPI extension properties **must** be provided in addition:

-   `#/info/x-api-id` unique identifier of the API ([see rule 215](#215))

-   `#/info/x-audience` intended target audience of the API ([see rule 219](#219))

{MUST} Use Semantic Versioning
==============================

OpenAPI allows to specify the API specification version in `#/info/version`. To share a common semantic of version information we expect API designers to comply to [Semantic Versioning 2.0](http://semver.org/spec/v2.0.0.html) rules `1` to `8` and `11` restricted to the format &lt;MAJOR&gt;.&lt;MINOR&gt;.&lt;PATCH&gt; for versions as follows:

-   Increment the **MAJOR** version when you make incompatible API changes after having aligned this changes with consumers,

-   Increment the **MINOR** version when you add new functionality in a backwards-compatible manner, and

-   Optionally increment the **PATCH** version when you make backwards-compatible bug fixes or editorial changes not affecting the functionality.

**Additional Notes:**

-   **Pre-release** versions ([rule 9](http://semver.org#spec-item-9)) and **build metadata** ([rule 10](http://semver.org#spec-item-10)) must not be used in API version information.

-   While patch versions are useful for fixing typos etc, API designers are free to decide whether they increment it or not.

-   API designers should consider to use API version `0.y.z` ([rule 4](http://semver.org/#spec-item-4)) for initial API design.

Example:

    openapi: 3.0.1
    info:
      title: Parcel Service API
      description: API for <...>
      version: 1.3.7
      <...>

{MUST} Provide API Identifiers
==============================

Each API specification must be provisioned with a globally unique and immutable API identifier. The API identifier is defined in the `info`-block of the OpenAPI specification and must conform to the following definition:

    /info/x-api-id:
      type: string
      format: urn
      pattern: ^[a-z0-9][a-z0-9-:.]{6,62}[a-z0-9]$
      description: |
        Mandatory globally unique and immutable API identifier. The API
        id allows to track the evolution and history of an API specification
        as a sequence of versions.

API specifications will evolve and any aspect of an OpenAPI specification may change. We require API identifiers because we want to support API clients and providers with API lifecycle management features, like change trackability and history or automated backward compatibility checks. The immutable API identifier allows the identification of all API specification versions of an API evolution. By using [API semantic version information](#116) or [API publishing date](#192) as order criteria you get the **version** or **publication history** as a sequence of API specifications.

**Note**: While it is nice to use human readable API identifiers based on self-managed URNs, it is recommend to stick to UUIDs to relief API designers from any urge of changing the API identifier while evolving the API. Example:

    openapi: 3.0.1
    info:
      x-api-id: d0184f38-b98d-11e7-9c56-68f728c1ba70
      title: Parcel Service API
      description: API for <...>
      version: 1.5.8
      <...>

{MUST} Provide API Audience
===========================

Each API must be classified with respect to the intended target **audience** supposed to consume the API, to facilitate differentiated standards on APIs for discoverability, changeability, quality of design and documentation, as well as permission granting. We differentiate the following API audience groups with clear organisational and legal boundaries:

**component-internal**  
This is often referred to as a *team internal API* or a *product internal API*. The API consumers with this audience are restricted to applications of the same **functional component** which typically represents a specific **product** with clear functional scope and ownership. All services of a functional component / product are owned by a specific dedicated owner and engineering team(s). Typical examples of component-internal APIs are APIs being used by internal helper and worker services or that support service operation.

**business-unit-internal**  
The API consumers with this audience are restricted to applications of a specific product portfolio owned by the same business unit.

**company-internal**  
The API consumers with this audience are restricted to applications owned by the business units of the same the company (e.g. Zalando company with Zalando SE, Zalando Payments SE & Co. KG. etc.)

**external-partner**  
The API consumers with this audience are restricted to applications of business partners of the company owning the API and the company itself.

**external-public**  
APIs with this audience can be accessed by anyone with Internet access.

**Note:** a smaller audience group is intentionally included in the wider group and thus does not need to be declared additionally.

The API audience is provided as API meta information in the `info`-block of the Open API specification and must conform to the following specification:

    /info/x-audience:
      type: string
      x-extensible-enum:
        - component-internal
        - business-unit-internal
        - company-internal
        - external-partner
        - external-public
      description: |
        Intended target audience of the API. Relevant for standards around
        quality of design and documentation, reviews, discoverability,
        changeability, and permission granting.

**Note:** Exactly **one audience** per API specification is allowed. For this reason a smaller audience group is intentionally included in the wider group and thus does not need to be declared additionally. If parts of your API have a different target audience, we recommend to split API specifications along the target audience — even if this creates redundancies ([rationale (internal link)](https://apis.zalando.net/redirect/85ee93a3-7a78-4461-8bf1-08ffdaebcd18)).

Example:

    openapi: 3.0.1
    info:
      x-audience: company-internal
      title: Parcel Helper Service API
      description: API for <...>
      version: 1.2.4
      <...>

For details and more information on audience groups see the [API Audience narrative (internal link)](https://apis.zalando.net/redirect/85ee93a3-7a78-4461-8bf1-08ffdaebcd18).
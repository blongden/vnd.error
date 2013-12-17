# vnd.error

* Author: Ben Longden <ben@nocarrier.co.uk>
* Created: 2012-04-24
* Last modified: 2012-06-29
* Status: Draft

## Description

vnd.error is a simple way of expressing an error response in XML or JSON.

Often when returning a response to a client a response type is needed to represent a problem to the user (human or otherwise).  A media type representation is a convenient way of expressing the error in a standardised format and can be understood by many client applications.

The format is 100% compatible with the [HAL specification](http://tools.ietf.org/html/draft-kelly-json-hal). This draft adds a series of link relations, body elements and recommendations for communicating an error state.

This media type is intended for use with the HTTP status codes 4xx and 5xx, though this does not exclude it from use in any other scenario.

The vnd.error+json

## Compliance

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements is said to be “unconditionally compliant”; one that satisfies all the MUST level requirements but not all the SHOULD level requirements is said to be “conditionally compliant.”

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Media Type Identifiers

* application/vnd.error+xml
* application/vnd.error+json

## Examples

application/vnd.error+xml
```xml
<?xml version="1.0"?>
<resource logref="42">
  <link rel="help" href="http://.../" title="Error Information"/>
  <link rel="describes" href="http://.../" title="Error Description"/>
  <message>Validation failed</message>
</resource>
```

application/vnd.error+json
```json
{
    "logref": 42,
    "message": "Validation failed",
    "_links": {
        "help": {
            "href": "http://.../",
            "title": "Error Information"
        },
        "describes": {
            "href": "http://.../",
            "title": "Error Description"
        }
    }
}
```

## Error Attributes

### logref

REQUIRED

For expressing a (numeric/alpha/alphanumeric) identifier to refer to the specific error on the server side for logging purposes (i.e. a request number).

### message

REQUIRED

For expressing a human readable message related to the current error which may be displayed to the user of the api.

## Link Attributes

Link attributes follow the same semantics as defined in the HAL specification [section 5](http://tools.ietf.org/html/draft-kelly-json-hal-06#section-5). For completeness, the required elements are all represented below.

### href

The "href" property is REQUIRED.

Its value is either a URI [RFC3986](http://tools.ietf.org/html/rfc3986) or a URI Template [RFC6570](http://tools.ietf.org/html/rfc6570).

If the value is a URI Template then the Link Object SHOULD have a "templated" attribute whose value is true.

## Recommendations

### Retry-After

RFC2616 defines the [Retry-After](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.37) header for use with a 503 (Service Unavailable), or 3xx response. If you wish to communicate a delay before retrying a request, for example 'Rate limit reached', make use of this with the vnd.error document.

### I18n

It has been considered that internationalisation (i18n) should be incorporated into the media type, however it was decided that the Accept-Language and Content-Language headers of HTTP should be used instead. The XML variation may take advantage of the [xml:lang](http://www.w3.org/TR/xml/#sec-lang-tag) attribute if a need arises.

## Acknowledgements

Thanks to Mike Kelly, Darrel Miller and Mike Amundsen for their initial discusson on vnd.error+xml on the 'Hal Discuss' group.

# vnd.error

* Author: Ben Longden <ben@nocarrier.co.uk>
* Created: 2012-04-24
* Last modified: 2014-09-09
* Status: Draft

## Description

vnd.error is a simple way of expressing an error response in JSON.

Often when returning a response to a client a response type is needed to represent a problem to the user (human or otherwise).  A media type representation is a convenient way of expressing the error in a standardised format and can be understood by many client applications.

The format is 100% compatible with the [HAL specification](http://tools.ietf.org/html/draft-kelly-json-hal). This draft adds a series of link relations, body elements and recommendations for communicating an error state.

This media type is intended for use with the HTTP status codes 4xx and 5xx, though this does not exclude it from use in any other scenario.

## Compliance

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements is said to be “unconditionally compliant”; one that satisfies all the MUST level requirements but not all the SHOULD level requirements is said to be “conditionally compliant.”

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Media Type Identifiers

* application/vnd.error+json

## Profile Links

Whilst the specification does register its own media type identifiers, it is also possible to use the profile link relation below  ([RFC6906](https://tools.ietf.org/html/rfc6906)) along side the application/hal+json media type.

* http://nocarrier.co.uk/profiles/vnd.error/

## Examples

application/vnd.error+json
```json
{
    "message": "Validation failed",
    "path": "/username",
    "logref": 42,
    "_links": {
        "about": {
            "href": "http://path.to/user/resource/1"
        },
        "describes": {
            "href": "http://path.to/describes"
        },
        "help": {
            "href": "http://path.to/help"
        }
    }
}
```

### Multiple Errors

Multiple errors may be represented in a collection of embedded vnd.error objects.

```javascript
{
    "total": 2,
    "_embedded": {
        "errors": [
            {
                "message": "\"username\" field validation failed",
                "logref": 50,
                "_links": {
                    "help": {
                        "href": "http://.../"
                    }
                }
            },
            {
                "message": "\"postcode\" field validation failed",
                "logref": 55,
                "_links": {
                    "help": {
                        "href": "http://.../"
                    }
                }
            }
        ]
    }
}
```

### Nested Errors

Nested errors may be represented by embedding multiple errors inside a vnd.error resource.

```javascript
{
    "message": "Validation failed",
    "logref": 42,
    "_links": {
        "describes": {
            "href": "http://path.to/describes"
        },
        "help": {
            "href": "http://path.to/help"
        },
        "about": {
            "href": "http://path.to/user/resource/1"
        }
    },
    "_embedded": {
        "errors": [
            {
                "message": "Username must contain at least three characters",
                "path": "/username",
                "_links": {
                    "about": {
                        "href": "http://path.to/user/resource/1"
                    }
                }
            }
        ]
    }
}
```

## Error Attributes

### message

REQUIRED

For expressing a human readable message related to the current error which may be displayed to the user of the api.

### logref

OPTIONAL

For expressing a (numeric/alpha/alphanumeric) identifier to refer to the specific error on the server side for logging purposes (i.e. a request number).

### path

OPTIONAL

For expressing a JSON Pointer ([RFC6901](http://tools.ietf.org/html/rfc6901)) to a field in related resource (contained in the 'about' link relation) that this error is relevant for.

## Link Attributes

Link attributes follow the same semantics as defined in the HAL specification [section 5](http://tools.ietf.org/html/draft-kelly-json-hal-06#section-5). For completeness, the required elements are all represented below.

### href

The "href" property is REQUIRED.

Its value is either a URI [RFC3986](http://tools.ietf.org/html/rfc3986) or a URI Template [RFC6570](http://tools.ietf.org/html/rfc6570).

If the value is a URI Template then the Link Object SHOULD have a "templated" attribute whose value is true.

## Link Relations

### help

The "help" link relation is OPTIONAL.

Links to a document describing the error. This has the same definition as the help link relation in the [HTML5 specification](http://www.w3.org/TR/html5/links.html#link-type-help)

### describes

The "describes" link relation is OPTIONAL.

Present if this error representation describes another representation of the error on the server side. See [RFC6892](http://tools.ietf.org/html/rfc6892) for further details.

### about

The "about" link relation is OPTIONAL.

Links to a resource that this error is related to. See [RFC6903](http://tools.ietf.org/html/rfc6903#section-2) for further details.

## Recommendations

### Retry-After

RFC2616 defines the [Retry-After](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.37) header for use with a 503 (Service Unavailable), or 3xx response. If you wish to communicate a delay before retrying a request, for example 'Rate limit reached', make use of this with the vnd.error document.

### I18n

It has been considered that internationalisation (i18n) should be incorporated into the media type, however it was decided that the Accept-Language and Content-Language headers of HTTP should be used instead.

## Acknowledgements

Thanks to Mike Kelly, Darrel Miller and Mike Amundsen for their initial discusson on vnd.error on the 'Hal Discuss' group.

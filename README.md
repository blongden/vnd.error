# vnd.error

* Author: Ben Longden <ben@nocarrier.co.uk>
* Created: 2012-04-24
* Last modified: 2012-06-29
* Status: Draft

## Description

vnd.error is a simple way of expressing an error response in XML or JSON.

Often when returning a response to a client a response type is needed to represent a problem to the user (human or otherwise).  A media type representing this error is a convenient way of expressing the error in a standardised format and can be understood by many client applications.

This media type is intended for use with the HTTP status codes 4xx and 5xx, though this does not exclude it from use in any other scenario.

It has been considered that internationalisation (i18n) should be incorporated into the media type, however it was decided that the Accept-Language and Content-Language headers of HTTP should be used instead. The XML variation may take advantage of the [xml:lang](http://www.w3.org/TR/xml/#sec-lang-tag) attribute if a need arises.

## Compliance

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements is said to be “unconditionally compliant”; one that satisfies all the MUST level requirements but not all the SHOULD level requirements is said to be “conditionally compliant.”

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Media Type Identifiers

* application/vnd.error+xml
* application/vnd.error+json

## Examples

application/vnd.error+xml
```xml
<errors xml:lang="en">
    <error logref=42>
        <message>Validation failed</message>
        <link rel='help' href='http://...' title='Error information'/>
        <link rel='describes' href='http://...' title='Error description'/>
    </error>
</errors>
```

application/vnd.error+json
```json
[
    {
        "logref": 42,
        "message": "Validation failed",
        "_links": {
            "help": [
                { "href":  "http://...", "title": "Error information" }
            ],
            "describes": [
                { "href":  "http://...", "title": "Error description" }
            ]
        }
    }
]
```

## Components

vnd.error provides hypertext capabilities via two elements.

### error

REQUIRED

The root element representing the errors themselves. Within the root element are 1 to N error representations as represented by the media format. The three other hypertext elements MUST be contained within the error representation (see examples above).

### link

OPTIONAL

For expressing "outbound" hyperlinks to other, related resources.

## Error Attributes

### logref

REQUIRED

For expressing a (numeric/alpha/alphanumeric) identifier to refer to the specific error on the server side for logging purposes (i.e. a request number).

### message

REQUIRED

For expressing a human readable message related to the current error which may be displayed to the user of the api.

## Link Attributes

### rel

REQUIRED

For identifying how the target URI relates to the ‘Subject Resource’. The Subject Resource is the closest parent error element.

rel corresponds with the '[relation parameter](http://tools.ietf.org/html/rfc5988#section-5.3)' as defined in [Web Linking (RFC 5988)](http://tools.ietf.org/html/rfc5988).

rel attribute SHOULD be used to identify link elements in vnd.error representation.

### href

REQUIRED

This attribute MAY contain a URI template. Whether or not this is the case SHOULD be indicated to clients by the rel value.

### title

OPTIONAL

For labeling the destination of a link with a human-readable identifier.

### hreflang

OPTIONAL

For indicating what the language of the result of dereferencing the link should be.

## Constraints

The root of a vnd.error representation MUST be an 'errors' element containing one or more 'error' representations.

## Acknowledgements

Thanks to Mike Kelly, Darrel Miller and Mike Amundsen for their initial discusson on vnd.error+xml on the 'Hal Discuss' group.

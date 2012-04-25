# vnd.error

* Author: Ben Longden <ben@nocarrier.co.uk>
* Date: 2012-04-24 (Created)
* Status: Draft

## Description

vnd.error is a simple way of expressing an error response in XML or JSON.

Often when returning a response to a client a response type is needed to represent a problem to the user (human or otherwise).  A media type representing this error is a convenient way of expressing the error in a standardised format and can be understood by many client applications.

This media type is intended for use with the HTTP status codes 4xx and 5xx, though this does not exclude it from use in any other scenario.

## Compliance

An implementation is not compliant if it fails to satisfy one or more of the MUST or REQUIRED level requirements. An implementation that satisfies all the MUST or REQUIRED level and all the SHOULD level requirements is said to be “unconditionally compliant”; one that satisfies all the MUST level requirements but not all the SHOULD level requirements is said to be “conditionally compliant.”

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

## Media Type Identifiers

* application/vnd.error+xml
* application/vnd.error+json

## Examples

application/vnd.error+xml
```xml
<error id="42">
    <message xml:lang="en">Validation failed</message>
    <message xml:lang="de">Validierung fehlgeschlagen</message>
    <link rel='help' href='http://...' title='Error information'/>
</error>
```

application/vnd.error+json
```json
{
    "id": 42,
    "messages": [ 
        { "lang": "en", "message": "Validation failed" },
        { "lang": "de", "message": "Validierung fehlgeschlagen" }
    ],
    "_links": {
        "help": [
            { "href":  "http://...", "title": "Error information" }
        ]
    }
}
```

## Components

vnd.error provides hypertext capabilitied via two elements.

### error

REQUIRED

The root element representing the error itself. The three other hypertext elements MUST be contained within the error representation (see examples above).

### link

OPTIONAL

For expressing "outbound" hyperlinks to other, related resources.

## Error Attributes

### id

REQUIRED

For expressing an identifier to refer to the specific error on the server side for logging purposes.

### message

REQUIRED

For expressing a human readable message related to the current error.

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

The root of a vnd.error representation MUST be a Error element.

## Acknowledgements

Thanks to Mike Kelly, Darrel Miller and Mike Amundsen for their initial discusson on vnd.error+xml on the 'Hal Discuss' group.

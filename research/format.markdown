# Research on `format` keyword 
Changes in `format` Keyword Behaviour Across all JSON Schema Drafts: Additions, Deletions, and Upgrades.

## Draft 0 to Draft 2 (No changes)

## Draft 2 to Draft 3

References: [Draft 2 Section 5.20](https://json-schema.org/draft-02/draft-zyp-json-schema-02.txt), [Draft 3 Section 5.23](https://json-schema.org/draft-03/draft-zyp-json-schema-03.pdf)

> Behaviour:   
      >>[Before](https://json-schema.org/draft-02/draft-zyp-json-schema-02.txt): Not required to validate  
        [After](https://json-schema.org/draft-03/draft-zyp-json-schema-03.pdf): MAY validate (but aren’t required to)

| Change Type | Format Attribute | Description                                                      |
|-------------|------------------|------------------------------------------------------------------|
| Deleted     | `street-address`  | This SHOULD be a street address                               |
| Deleted     | `locality`         | This SHOULD be a city or town                                 |
| Deleted     | `region`           | This SHOULD be a region (a state in the US, province in Canada, etc.) |
| Deleted     | `postal-code`      | This SHOULD be a postal code (AKA zip code)                    |
| Deleted     | `country`          | This SHOULD be the name of a country|
| Added       | `host-name`        | This SHOULD be a host-name                                     |

## Draft 3 to Draft 4

References: [Draft 3 Section 5.23](https://json-schema.org/draft-03/draft-zyp-json-schema-03.pdf), [Draft 4](https://json-schema.org/draft-04/draft-fge-json-schema-validation-00#rfc.section.7
)

> Behaviour:  
      >>[Before](https://json-schema.org/draft-03/draft-zyp-json-schema-03.pdf): MAY validate.   
      [After](https://json-schema.org/draft-04/draft-fge-json-schema-validation-00#rfc.section.7.2):  
      a. they SHOULD implement validation for all the format attributes.   
      b. they SHOULD offer an option to disable validation for this keyword.  
      c. Implementations MAY add custom format attributes but not peer implementations.

| Change Type | Format Attribute | Description                                                                                 |
|-------------|------------------|---------------------------------------------------------------------------------------------|
| Deleted     | `date`             | This SHOULD be a date in the format of YYYY-MM-DD                                          |
| Deleted     | `time`             | This SHOULD be a time in the format of hh:mm:ss                                            |
| Deleted     | `utc-millisec`     | This SHOULD be the difference, measured in milliseconds, between the specified time and midnight, 00:00 of January 1, 1970 UTC. The value SHOULD be a number (integer or float) |
| Deleted     | `regex`            | A regular expression, following the regular expression specification from ECMA 262/Perl 5  |
| Deleted     | `color`            | This is a CSS color (like `#FF0000` or `red`), based on [CSS 21](https://www.w3.org/TR/2007/CR-CSS21-20070719/)                          |
| Deleted     | `style`            | This is a CSS style definition (like `color: red; backgroundcolor:#FFF`), based on [CSS 2.1](https://www.w3.org/TR/2007/CR-CSS21-20070719/) |
| Deleted     | `phone`            | This SHOULD be a phone number (format MAY follow E.123)                                  |

## Draft 4 to Draft 6

References: [Draft 4](https://json-schema.org/draft-04/draft-fge-json-schema-validation-00#rfc.section.7
),
[Draft 6](https://json-schema.org/draft-06/draft-wright-json-schema-validation-01#rfc.section.8)

| Change Type | Format Attribute | Description                                                                                                          |
|-------------|------------------|----------------------------------------------------------------------------------------------------------------------|
| Added       | `uri-reference`    | This attribute applies to string instances. A string instance is valid against this attribute if it is a valid URI Reference (either a URI or a relative-reference), according to [RFC3986](https://www.rfc-editor.org/info/rfc3986) |
| Added       | `uri-template`     | This attribute applies to string instances. A string instance is valid against this attribute if it is a valid URI Template (of any level), according to [RFC6570](https://www.rfc-editor.org/info/rfc6570)                          |
| Added       | `json-pointer`     | This attribute applies to string instances. A string instance is valid against this attribute if it is a valid JSON Pointer, according to [RFC6901](https://www.rfc-editor.org/info/rfc6901)                                            |

## Draft 6 to Draft 7

References: [Draft 6](https://json-schema.org/draft-06/draft-wright-json-schema-validation-01#rfc.section.8), [Draft 7](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#rfc.section.7)

| Change Type | Format Attribute           | Description                                                                                                          |
|-------------|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| Added       | `date`                       | A string instance is valid against this attribute if it is a valid representation according to the `full-date` production                                |
| Added       | `time`                       | A string instance is valid against this attribute if it is a valid representation according to the `full-time` production                                |
|             |                            | Implementations MAY support additional attributes using the other production names defined in that section. If `full-date` or `full-time` are implemented, the corresponding short form (`date` or `time` respectively) MUST be implemented and MUST behave identically. Implementations SHOULD NOT define extension attributes with any name matching an [RFC 3339](https://www.rfc-editor.org/info/rfc3339) production unless it validates according to the rules of that production |
| Added       | `idn-email`                  | As defined by RFC 6531 [RFC6531](https://www.rfc-editor.org/info/rfc6531) |
| | |idn-email is a superset of email              |
| Added       | `hostname`                   | As defined by RFC 1034, section 3.1 [RFC1034](https://www.rfc-editor.org/info/rfc1034), including host names produced using the Punycode algorithm specified in RFC 5891, section 4.4 [RFC5891](https://www.rfc-editor.org/info/rfc5891) |
| Added       | `idn-hostname`               | As defined by either RFC 1034 as for hostname, or an internationalized hostname as defined by RFC 5890, section 2.3.2.3 [RFC5890](https://www.rfc-editor.org/info/rfc5890)  |
|||idn-hostname is a superset of hostname|
| Added       | `ipv6`                       | A string instance is valid against this attribute if it is a valid representation of an IPv6 address as defined in RFC 2373, section 2.2 [RFC2373](https://www.rfc-editor.org/info/rfc2373) An IPv6 address as defined in RFC 4291, section 2.2 [RFC4291](https://www.rfc-editor.org/info/rfc4291) |
| Added       | `iri`                        | A string instance is valid against this attribute if it is a valid IRI according to [RFC3987](https://www.rfc-editor.org/info/rfc3987)                                                          |
| Added       | `iri-reference`              | A string instance is valid against this attribute if it is a valid IRI Reference (either an IRI or a relative-reference) according to [RFC3987](https://www.rfc-editor.org/info/rfc3987)  |
|||iri is a superset of uri. iri-reference is a superset of uri-reference |
| Added       | `relative-json-pointer`      | A string instance is valid against this attribute if it is a valid Relative JSON Pointer [relative-json-pointer](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#relative-json-pointer) |
| Added       | `regex`                      | A regular expression, which SHOULD be valid according to the ECMA 262 [ecma262](https://262.ecma-international.org/12.0/index.html) regular expression dialect                                               |

## Draft 7 to 2019-09

References: [Draft 7](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#rfc.section.7), [2019-09](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7)

> Behaviour: 
      >>[Before](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#rfc.section.7.2): They SHOULD implement validation, they SHOULD offer an option to disable validation for this keyword.  
      [After](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7.2): The `format` keyword functions as an annotation, and optionally as an assertion. Also, when treating values as annotations unknown formats SHOULD be collected as annotations.

| Change Type | Format Attribute | Description |
|-------------|------------------|-------------|
| Added       | `duration`         | A string instance is valid against this attribute if it is a valid representation according to the `duration` production |
| Added       | `uuid`             | A string instance is valid against this attribute if it is a valid string representation of a UUID, according to [RFC4122](https://www.ietf.org/rfc/rfc4122.txt) |

## 2019-09 to 2020-12

References: [2019-09](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7), [2020-12](https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01#section-7)

> Behaviour: 
      >>[Before](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7.2.3): An implementation MUST NOT fail validation or cease processing due to an unknown format attribute.  
      [After](https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01#section-7.2.3): When the Format-Assertion vocabulary is specified, implementations MUST fail upon encountering unknown formats.

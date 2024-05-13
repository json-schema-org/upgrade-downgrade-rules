## Draft 2 to Draft 3:

> Behaviour: Not required to validate → MAY validate(but aren’t required to)

**\-**  **street-address** : This should be a street address.

**\-**  **locality** : This should be a city or town.

**\-**  **region** : This should be a region (a state in the US, province in Canada, etc.)

**\-**      **postal-code** : This should be a postal code (AKA zip code).

**\-**      **country** : This should be the name of a country.


**\+**     **host-name** : This SHOULD be a host-name.  



## Draft 3 to Draft 4:

> MAY validate → 
      a. they SHOULD implement validation for attributes defined below;
      b. they SHOULD offer an option to disable validation for this keyword.
      c. Implementations MAY add custom format attributes but not peer implementations.

**\-**     **date** : This SHOULD be a date in the format of YYYY-MM-DD.

**\-**     **time** : This SHOULD be a time in the format of hh:mm:ss.

**\-**     **utc-millisec** : This SHOULD be the difference, measured in milliseconds, between the specified time and midnight, 00:00 of January 1, 1970 UTC. The value SHOULD be a number (integer or float).

**\-**     **regex** : A regular expression, following the regular expression specification from ECMA 262/Perl 5.

**\-**     **color** : This is a CSS color (like "#FF0000" or "red"), based on CSS 2.1 (Hickson, I., Lie, H., Çelik, T., and B. Bos, “Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification,” July 2007.) [W3C.CR‑CSS21‑20070719].

**\-**     **style** : This is a CSS style definition (like "color: red; backgroundcolor:#FFF"), based on CSS 2.1 (Hickson, I., Lie, H., Çelik, T., and B. Bos, “Cascading Style Sheets Level 2 Revision 1 (CSS 2.1) Specification,” July 2007.) [W3C.CR‑CSS21‑20070719].

**\-**     **phone** : This SHOULD be a phone number (format MAY follow E.123).

### Changes: 

| Attribute   | Previous Description                                                                      | PresentDescription           |
|-------------|-------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| date-time   | This SHOULD be a date in ISO 8601 format of YYYY-MMDDThh:mm:ssZ in UTC time. This is... | A string instance is valid against this attribute if it is a valid date representation as defined by RFC 3339, section 5.6[RFC3339].                                  |
| URI         | Should be a uri                                                                           | A string instance is valid against this attribute if it is a valid URI, according to [RFC3986].           |
| email       | Should be email                                                                           | A string instance is valid against this attribute if it is a valid Internet email address as defined by RFC 5322, section 3.4.1 [RFC5322].                         |
| ip-address  | Should be ip v 4 address                                                                 | ipv4: A string instance is valid against this attribute if it is a valid representation of an IPv4 address according to the "dotted-quad" ABNF syntax as defined in RFC 2673, section 3.2 [RFC2673]. |
| ipv6        | Should be ip v 6 address                                                                 | A string instance is valid against this attribute if it is a valid representation of an IPv6 address as defined in RFC 2373, section 2.2 [RFC2373].                 |
| host-name   | This SHOULD be a host-name                                                               | A string instance is valid against this attribute if it is a valid representation for an Internet host name, as defined by RFC 1034, section 3.1 [RFC1034].           |



## Draft 4 to Draft 6:

References: [draft4](https://json-schema.org/draft-04/draft-fge-json-schema-validation-00#rfc.section.7
) & 
[draft6](https://json-schema.org/draft-06/draft-wright-json-schema-validation-01#rfc.section.8)

**\+**   **uri-reference** : This attribute applies to string instances. A string instance is valid against this attribute if it is a valid URI Reference (either a URI or a relative-reference), according to [RFC3986].

**\+**   **uri-template** : This attribute applies to string instances. A string instance is valid against this attribute if it is a valid URI Template (of any level), according to [RFC6570].

**\+**    **json-pointer** : This attribute applies to string instances. A string instance is valid against this attribute if it is a valid JSON Pointer, according to [RFC6901].



## Draft 6 to Draft 7:

References: [draft6](https://json-schema.org/draft-06/draft-wright-json-schema-validation-01#rfc.section.8) & [draft7](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#rfc.section.7)

**\+**   **date** : A string instance is valid against this attribute if it is a valid representation according to the "full-date" production.

**\+**   **time** : A string instance is valid against this attribute if it is a valid representation according to the "full-time" production.

Implementations MAY support additional attributes using the other production names defined in that section. If "full-date" or "full-time" are implemented, the corresponding short form ("date" or "time" respectively) MUST be implemented, and MUST behave identically. Implementations SHOULD NOT define extension attributes with any name matching an RFC 3339 production unless it validates according to the rules of that production. [CREF2]

**\+**   **idn-email** : As defined by RFC 6531 [RFC6531]. 

**idn-email ⊃ email**

**\+**   **hostname** : As defined by RFC 1034, section 3.1 [RFC1034], including host names produced using the Punycode algorithm specified in RFC 5891, section 4.4 [RFC5891].

**\+**   **idn-hostname** : As defined by either RFC 1034 as for hostname, or an internationalized hostname as defined by RFC 5890, section 2.3.2.3 [RFC5890].

**idn-hostname ⊃  hostname**

**\+**   **ipv6** : A string instance is valid against this attribute if it is a valid representation of an IPv6 address as defined in RFC 2373, section 2.2 [RFC2373]. 

An IPv6 address as defined in RFC 4291, section 2.2 [RFC4291].

**\+**   **iri**: A string instance is valid against this attribute if it is a valid IRI, according to [RFC3987].

**\+**   **iri-reference**: A string instance is valid against this attribute if it is a valid IRI Reference (either an IRI or a relative-reference), according to [RFC3987].
**iri ⊃  uri**
**iri-reference ⊃  uri-reference**

**\+**   **relative-json-pointer-** : A string instance is valid against this attribute if it is a valid Relative JSON Pointer [relative-json-pointer].

**\+**   **regex** : A regular expression, which SHOULD be valid according to the ECMA 262 [ecma262] regular expression dialect.



## Draft 7 to 2019-09:

References: [Draft 7](https://json-schema.org/draft-07/draft-handrews-json-schema-validation-01#rfc.section.7), [2019-09](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7)

> Behavior: They SHOULD implement validation, they SHOULD offer an option to disable validation for this keyword → The "format" keyword functions as an annotation, and optionally as an assertion. Also, when treating values as annotations unknown formats SHOULD be collected as annotations.

**\+**    **duration** -  A string instance is valid against this attribute if it is a valid representation according to the "duration" production.

**\+**     **uuid**: A string instance is valid against this attribute if it is a valid string representation of a UUID, according to [RFC4122].

### Changes :

| Keyword   | Previous Description                                                                                                                         | After Description                                                                                                                               |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| hostname  | As defined by RFC 1034, section 3.1 [RFC1034], including host names produced using the Punycode algorithm specified in RFC 5891, section 4.4 [RFC5891]. | As defined by RFC 1123, section 2.1, including host names produced using the Punycode algorithm specified in RFC 5891, section 4.4 [RFC5891]. |





## 2019-09 - 2020-12:

References: 
- [2019-09](https://json-schema.org/draft/2019-09/draft-handrews-json-schema-validation-02#rfc.section.7)
- [2020-12](https://json-schema.org/draft/2020-12/draft-bhutton-json-schema-validation-01#section-7)

> !!! An implementation MUST NOT fail validation or cease processing due to an unknown format attribute --> When the Format-Assertion vocabulary is specified, implementations MUST fail upon encountering unknown formats.

### Changes :

| Keyword    | Previous Description                                                                                                    | After Description                                                                                                           |
|------------|--------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| email      | Defined in RFC 5321                                                                                                      | As defined by the "Mailbox" ABNF rule in RFC 5321, section 4.1.2 [RFC5321].                                                |
| idn-email  | RFC 6531                                                                                                                 | As defined by the extended "Mailbox" ABNF rule in RFC 6531, section 3.3 [RFC6531].                                           |
| regex      | ECMA-262                                                                                                                 | ECMA-262 defined in the Regular Expressions (Section 4.3).                                                                   |

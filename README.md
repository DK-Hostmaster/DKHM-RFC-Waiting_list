![DK Hostmaster Logo](https://www.dk-hostmaster.dk/sites/default/files/dk-logo_0.png)

# DKHM RFC for Domain Names Offered From Waiting List

![Markdownlint Action](https://github.com/DK-Hostmaster/DKHM-RFC-Waiting_list/workflows/Markdownlint%20Action/badge.svg)
![Spellcheck Action](https://github.com/DK-Hostmaster/DKHM-RFC-Waiting_list/workflows/Spellcheck%20Action/badge.svg)

2020-09-19
Revision: 1.1

## Table of Contents

<!-- MarkdownTOC bracket=round levels="1,2,3,4" indent="  " autolink="true" autoanchor="true" -->

- [Introduction](#introduction)
  - [About this Document](#about-this-document)
  - [License](#license)
  - [Document History](#document-history)
  - [XML and XSD Examples](#xml-and-xsd-examples)
- [Description](#description)
  - [Registration of a Domain Name Offered from a Waiting list](#registration_of_a_domain_name_offered_from_a_waiting_list)
- [References](#references)

<!-- /MarkdownTOC -->

<a id="introduction"></a>
## Introduction

This is a draft and proposal for changes to the process for domain name creation via the DK Hostmaster EPP portal/service. The specification briefly touches on the registrar portal service, which mimicks the EPP service for consistency.

The concept is to get the registration and application process to be uniform. DK Hostmaster offers a waiting list product to end-users today. When waiting list offers require registration this is completed via DK Hostmaster.

The raises some issues in regard to:

- name servers associated with the domain name
- optional specification of billing contact
- optional specification administrative contact
- and ultimately, the registrar contact, based on the end-users choice of administrative model

The registrar model offered by DK Hostmaster, gives registrars the option to manage a customer’s .dk domain name if the customer would prefer this. We call this "registrar management". Where the model to allow the customer to manage their own domain name themselves, as they do today, is referred to as "registrant management".

The overall [description of the concept][CONCEPT] of the registrar model offered by DK Hostmaster A/S provided as a general overview.

This RFC proposes to let domain names offered from the waiting list to be registered via the existing channels and by using the existing procedures to address the above short comings to the existing process.

The procedure can be contained in the current registration process and within the EPP standard described in [RFC:5731][RFC5371].

The only addition is that the offering communicates a AuthInfo token to the registrant, which has to be communicated as part of the registration request.

<a id="about-this-document"></a>
### About this Document

We have adopted the term RFC (_Request For Comments_), due to the recognition in the term and concept, so this document is a process supporting document, aiming to serve the purpose of obtaining a common understanding of the proposed implementation and to foster discussion on the details of the implementation. The final specification will be lifted into the [DK Hostmaster EPP Service Specification][DKHMEPPSPEC] implementation and this document will be closed for comments and the document no longer be updated.

This document is not the authoritative source for business and policy rules and possible discrepancies between this an any authoritative sources are regarded as errors in this document. This document is aimed at the technical specification and possible implementation and is an interpretation of authoritative sources and can therefor be erroneous.

<a id="license"></a>
### License

This document is copyright by DK Hostmaster A/S and is licensed under the MIT License, please see the separate LICENSE file for details.

<a id="document-history"></a>
### Document History

- 1.1 2020-09-19
  - Addition of disclaimer

- 1.0 2020-09-19
  - Initial revision

<a id="xml-and-xsd-examples"></a>
### XML and XSD Examples

All example XML files are available in the [DK Hostmaster EPP XSD repository][DKHMXSDSPEC].

The proposed extensions and XSD definitions are available in the  [3.2 candidate][DKHMXSD3.2] of the DK Hostmaster XSD, which is currently a draft and work in progress and marked as a  _pre-release_.

<a id="description"></a>
## Description

<a id="registration_of_a_domain_name_offered_from_a_waiting_list"></a>
### Registration of a Domain Name Offered from a Waiting list

The registration, as described in the introduction is uniform to the process used today and described in the [DK Hostmaster EPP Service Specification][DKHMEPPSPEC].

The only addition is the use of the standard AuthInfo section.

```xml
<?xml version="1.0" encoding="utf-8"?>
<epp xmlns="urn:ietf:params:xml:ns:epp-1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="urn:ietf:params:xml:ns:epp-1.0 epp-1.0.xsd">
    <command>
        <create>
            <domain:create xmlns:domain="urn:ietf:params:xml:ns:domain-1.0" xsi:schemaLocation="urn:ietf:params:xml:ns:domain-1.0 domain-1.0.xsd">
                <domain:name>dk-hostmaster-test-906.dk</domain:name>
                <domain:period unit="y">1</domain:period>
                <domain:ns>
                    <domain:hostObj>ns1.dk-hostmaster.dk</domain:hostObj>
                    <domain:hostObj>ns2.dk-hostmaster.dk</domain:hostObj>
                </domain:ns>
                <domain:registrant>DKHM1-DK</domain:registrant>
                <domain:contact type="admin">DKHM2-DK</domain:contact>
                <domain:contact type="billing">DKHM3-DK</domain:contact>
                <domain:authInfo>
                    <domain:pw>445e6568-fa53-11ea-adc1-0242ac120002</domain:pw>
                </domain:authInfo>
            </domain:create>
        </create>
        <extension>
            <dkhm:orderconfirmationToken xmlns:dkhm="urn:dkhm:params:xml:ns:dkhm-3.0">testtoken</dkhm:orderconfirmationToken>
        </extension>
        <clTRID>92724843f12a3e958588679551aa988d</clTRID>
    </command>
</epp>
```

Example lifted from the [DK Hostmaster EPP Service Specification][DKHMEPPSPEC] and modified.

As can be read from the example:

- name servers are now assigned as part of the registration and possible DNS issues addressed from registration time
- Administrative contact is specified
- Billing contact is specified

The details on associating the registrar account is described in the ["DKHM RFC for Client ID support for EPP"][DKHMRFCCLID].

For the registrar portal, the procedure would be the same and additional optional parameter would be available for the transport of the AuthInfo token,

<a id="references"></a>
## References

- [DK Hostmaster EPP Service Specification][DKHMEPPSPEC]
- [DK Hostmaster EPP Service XSD Repository][DKHMXSDSPEC]
- [RFC:5730 "Extensible Provisioning Protocol (EPP)"][RFC5730]

[RFC5730]: https://www.rfc-editor.org/rfc/rfc5730.html
[DKHMEPPSPEC]: https://github.com/DK-Hostmaster/epp-service-specification
[DKHMXSDSPEC]: https://github.com/DK-Hostmaster/epp-xsd-files
[DKHMXSD3.2]: https://github.com/DK-Hostmaster/epp-xsd-files/blob/master/dkhm-3.2.xsd
[DKHMRFCCLID]: https://github.com/DK-Hostmaster/DKHM-RFC-CLID
[CONCEPT]: https://www.dk-hostmaster.dk/en/new-basis-collaboration-between-registrars-and-dk-hostmaster

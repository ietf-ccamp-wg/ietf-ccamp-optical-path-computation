---
title: "YANG Data Models for requesting Path Computation in WDM Optical Networks"
abbrev: "YANG for WDM Path Computation"
category: std

docname: draft-ietf-ccamp-optical-path-computation-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "Common Control and Measurement Plane"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "ietf-ccamp-wg/ietf-ccamp-optical-path-computation"
  latest: "https://ietf-ccamp-wg.github.io/ietf-ccamp-optical-path-computation/draft-ietf-ccamp-optical-path-computation-yang.html"

author:
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com


contributor:
  -
    name: Daniel King
    org: Old Dog Consulting
    email: daniel@olddog.co.uk

--- abstract

This document provides a mechanism to request path computation in Wavelength-Division Multiplexing (WDM) optical networks composed of Wavelength Switched Optical Networks (WSON) and Flexi-Grid Dense Wavelength Division Multiplexing (DWDM) switched technologies. This model augments the Remote Procedure Calls (RPCs) defined in RFC YYYY.

\[RFC EDITOR NOTE: Please replace RFC YYYY with the RFC number of
draft-ietf-teas-yang-path-computation once it has been published.

--- middle

# Introduction

{{!I-D.ietf-teas-yang-path-computation}} describes key use cases, where a client needs to request
underlying Software-Defined Network (SDN) controllers for path computation. In some of these use cases, the
underlying SDN controller can control a single-layer optical technologies, including
Optical Transport Network (OTN), Wavelength Switched Optical Networks (WSON), Flexi-grid, and multi-layer Optical network.

This document defines YANG data models, which augment the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, with technology-specific augmentations required to request path computation to an underlying Optical SDN controller. These models allow
a client to delegate path computation tasks to the underlying Optical SDN controller without having to obtain optical-layer information from the controller and performing feasible path computation itself. This is especially helpful in cases where computing optically-feasible paths require knowledge of physical-layer states, such as optical impairments, which are visible only to the Optical controller.

## Terminology and Notations

  Refer to {{?RFC7446}} and {{?RFC7581}} for the key terms used in this
  document.  The following terms are defined in {{!RFC7950}} and are not
  redefined here:

  *  client

  *  server

  *  augment

  *  data model

  *  data node

  The following terms are defined in {{!RFC6241}} and are not redefined
  here:

  *  configuration data

  *  state data

  The terminology for describing YANG data models is found in
  {{!RFC7950}}.

## Tree Diagram

  A simplified graphical representation of the data model is used in
  {{wdm-pc-tree}} of this document.  The meaning of the symbols in these
  diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects
  are prefixed using the standard prefix associated with the
  corresponding YANG imported modules, as shown in
  {{tab-prefixes}}.

| Prefix       | YANG module                      | Reference
| l0-types     | ietf-layer0-types                | \[RFCZZZZ]
| te           | ietf-te                          | \[RFCKKKK]
| tepc         | ietf-te-path-computation         | \[RFCYYYY]
| wdm-pc       | ietf-wdm-path-computation        | RFCXXXX
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-rfc9093-bis}}.
Please replace KKKK with the RFC number assigned to {{!I-D.ietf-teas-yang-te}}.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-teas-yang-path-computation}}.
Please remove this note.

# YANG Data Models for WDM Path Computation

## YANG Models Overview

The YANG data models for requesting WDM path computation are defined as augmentations of the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, as shown in {{fig-wdm-pc}}.

~~~~ ascii-art
                    +--------------------------+    o: augment
       TE generic   | ietf-te-path-computation |
                    +--------------------------+
                                 o
                                 |
                                 |
                                 |
                    +---------------------------+
                    | ietf-wdm-path-computation |
                    +---------------------------+

~~~~
{: #fig-wdm-pc title="Relationship between WDM and TE path computation models" artwork-name="wdm-path-computation.txt"}

The entities and Traffic Engineering (TE) attributes, such as requested path and tunnel attributes, defined in {{!I-D.ietf-teas-yang-path-computation}}, are still applicable when requesting WDM path computation and the models defined in this document only specifies the additional technology-specific attributes/information, using the attributes defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

## Attributes Augmentation

The common characteristics for layer 0 (WSON and Flexi-grid) path computation are under definition in {{!I-D.ietf-ccamp-rfc9093-bis}} and re-used in the ietf-wdm-path-computation YANG models.

## Bandwidth Augmentation {#wdm-te-bandwidth}

As described in Section 4.2 of {{!RFC7699}}, there is some overlap
between bandwidth and label in layer0.

The WSON and flexi-grid label resource information described in {{wdm-te-label}},
is sufficient to describe also the spectrum resources within WSON and
flexi-grid networks. Therefore, the model does not define any augmentation
for the te-bandwidth containers defined in {{!I-D.ietf-teas-yang-path-computation}}.

## Label Augmentations {#wdm-te-label}

The models augment all the occurrences of the label-restriction list
with WSON and Flexi-grid technology-specific attributes using the
l0-label-range-info and flexi-grid-label-range-info groupings defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

Moreover, the models augment all the occurrences of the te-label
container with the WSON, Flexi-grid and OTN technology-specific attributes using the
wson-label-start-end, wson-label-hop, wson-label-step,
flexi-grid-label-start-end, flexi-grid-label-hop and flexi-grid-label-step defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

# WDM Path Computation Tree Diagrams {#wdm-pc-tree}

{{fig-wdm-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-wdm-path-computation.yang.

~~~~ ascii-art
{::include-fold yang/ietf-wdm-path-computation.tree}
~~~~
{: #fig-wdm-pc-tree title="WDM path computation tree diagram" artwork-name="ietf-wdm-path-computation.tree"}


# YANG Models for WDM Path Computation {#wdm-pc-yang}

~~~~ yang
{::include yang/ietf-wdm-path-computation.yang}
~~~~
{: #fig-wdm-pc-yang title="WDM path computation YANG module" sourcecode-markers="true" sourcecode-name="ietf-wdm-path-computation@2026-02-27.yang"}


# Manageability Considerations

This document provides a method for requesting path computations for WSON and Flexi-Grid tunnels. Consideration of mechanisms to gather and collate information required for the path computations will be necessary. Furthermore, storing path computation requests and responses and triggering actions will also need to be carefully managed and secured.

Future versions of this document will contain additional information.

# Security Considerations

The YANG module defined in this document will be accessed via the NETCONF protocol {{!RFC6241}} or RESTCONF protocol {{!RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS and the mandatory-to-implement secure transport is TLS {{!RFC8446}}.

The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access to particular NETCONF or RESTCONF users to a pre-configured subset of all available NETCONF or RESTCONF protocol operations and content.

Some of the RPC operations defined in this YANG module may be
considered sensitive or vulnerable in some network environments. It is thus essential to control access to these operations.

Operations defined in this document, and their sensitivities and possible vulnerabilities, will be discussed further in future versions of this document.

# IANA Considerations

   This document registers the following URIs in the "ns" subregistry
   within the "IETF XML registry" {{!RFC3688}}.

~~~~
  URI: urn:ietf:params:xml:ns:yang:ietf-wdm-path-computation
  Registrant Contact:  The IESG.
  XML: N/A, the requested URI is an XML namespace.
~~~~

   This document registers the following YANG module in the "YANG Module Names"
   registry {{!RFC7950}}.

~~~~
  name:      ietf-wdm-path-computation
  namespace: urn:ietf:params:xml:ns:yang:ietf-wdm-path-computation
  prefix:    wdm-pc
  reference: this document
~~~~

--- back

# Change Log

The initial YANG data model requesting path computation in optical networks was draft-gbb-ccamp-optical-path-computation-yang-00. This document included path computation request capabilities for WSON, Flexi-Grid and OTN technologies. However, it was proposed at IETF 113 (March 25, 2022) to split the initial document into separate documents for WDM (WSON and Flexi-Grid) and OTN technologies, as each technology may be developed and implemented separately.

The WDM technology capabilities were kept in this document, and the OTN capabilities were moved into {{?I-D.draft-gbb-ccamp-otn-path-computation-yang}}.

Editors note, please remove this appendix before publication.

# Acknowledgments
{:numbered="false"}

The authors of this document would like to thank the authors of {{?I-D.ietf-teas-actn-poi-applicability}} for having identified the gap and requirements to trigger this work.

The authors of this document would also like to thank
Young Lee,
Haomian Zheng,
Victor Lopex,
Ricard Vilalta,
Bin Yeong Yoon,
Jorge E. Lopez de Vergara Mendez,
Daniel Perdices Burrero,
Oscar Gonzalez de Dios,
Gabriele Galimberti,
Zafar Ali,
Daniel Michaud Vallinoto and
Dhruv Dhody
who have contributed to the development of path computation augmentations for WSON and Flexi-grid topology in earlier versions of
{{?I-D.ietf-ccamp-wson-tunnel-model}} and of {{?I-D.ietf-ccamp-flexigrid-tunnel-yang}}.

This document was prepared using kramdown.

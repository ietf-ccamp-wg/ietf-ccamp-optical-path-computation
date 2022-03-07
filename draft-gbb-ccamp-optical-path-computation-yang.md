---
coding: utf-8

title: YANG Data Models for requesting Path Computation in Optical Networks

abbrev: Yang for Optical Path Computation
docname: draft-gbb-ccamp-optical-path-computation-yang-01
workgroup: CCAMP Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

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

This document describes YANG data models for Remote Procedure Calls (RPCs) to request Path Computation in Optical Networks (OTN, WSON and Flexi-grid).

The YANG data models defined in this
document conforms to the Network Management Datastore Architecture
(NMDA).

--- middle

# Introduction

{{!I-D.ietf-teas-yang-path-computation}} describes key use cases, where a client needs to request
underlying SDN controllers for path computation. In some of these use cases, the
underlying SDN controller can control a single-layer optical technologies, including 
Optical Transport Network (OTN), Wavelength Switched Optical Networks (WSON), Flexi-grid, 
and multi-layer Optical network.

This document defines YANG data models, which augment the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, with technology-specific augmentations required to request path computation to an underlying Optical SDN controller. These models allow
a client to delegate path computation tasks to the underlying Optical SDN controller without having to obtain optical-layer information from the controller and performing feasible path computation itself. This is especially helpful in cases where computing optically-feasible paths require knowledge of physical-layer states, such as optical impairments, which are visible only to the Optical controller.

The YANG data model defined in this document conforms to the Network
Management Datastore Architecture {{!RFC8342}}.

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
  {{optical-pc-tree}} of this document.  The meaning of the symbols in these
  diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects
  are prefixed using the standard prefix associated with the
  corresponding YANG imported modules, as shown in
  {{tab-prefixes}}.

| Prefix       | YANG module                      | Reference
| l0-types     | ietf-layer0-types                | {{!RFC9093}}
| l0-types-ext | ietf-layer0-types-ext            | \[RFCYYYY]
| l0-types     | ietf-layer0-types                | {{!RFC8776}}
| l1-types     | ietf-layer1-types                | \[RFCZZZZ]
| te           | ietf-te                          | \[RFCKKKK]
| tep          | ietf-te-path-computation         | \[RFCJJJJ]
| flexg-pc     | ietf-flexi-grid-path-computation | RFCXXXX         
| wson-pc      | ietf-wson-path-computation       | RFCXXXX         
| otn-pc       | ietf-otn-path-computation        | RFCXXXX         
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-ccamp-layer0-types-ext}}.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-layer1-types}}.
Please replace KKKK with the RFC number assigned to {{!I-D.ietf-teas-yang-te}}.
Please replace JJJJ with the RFC number assigned to {{!I-D.ietf-teas-yang-path-computation}}.
Please remove this note.

# YANG Data Models for Optical Path Computation 

## YANG Models Overview

The YANG data models for requesting WSON, Flexi-grid and OTN path computation are defined as augmentations of the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, as shown in {{fig-optical-pc}}.

~~~~
                    +--------------------------+    o: augment
       TE generic   | ietf-te-path-computation |
                    +--------------------------+
                          o      o      o
                          |      |      |
                          |      |      |
              +-----------+      |      +-----------+
              |                  |                  |
              |                  |                  |
              |                  |                  |
              |    +---------------------------+    |
              |    | ietf-otn-path-computation |    |
              |    +---------------------------+    |
              |                 OTN                 |
              |                                     |
              |                                     |
+----------------------------+                      |
| ietf-wson-path-computation |                      |
+----------------------------+                      |
            WSON                                    |
                                +----------------------------------+
                                | ietf-flexi-grid-path-computation |
                                +----------------------------------+
                                            Flexi-grid
~~~~
{: #fig-optical-pc title="Relationship between WSON, Flexi-grid, OTN and TE path computation models"}

The entities and Traffic Engineering (TE) attributes, such as requested path and tunnel attributes, defined in {{!I-D.ietf-teas-yang-path-computation}}, are still applicable when requesting WSON, Flexi-grid and OTN path computation and the models defined in this document only specifies the additional technology-specific attributes/information, using the attributes defined in {{!RFC9093}}, {{!I-D.ietf-ccamp-layer0-types-ext}} and {{!I-D.ietf-ccamp-layer1-types}}.

The YANG modules ietf-wson-path-computation, ietf-flexi-grid-path-computation and ietf-otn-path-computation defined in this document conforms
to the Network Management Datastore Architecture (NMDA) defined in
{{!RFC8342}}.

## Attributes Augmentation

The common characteristics for layer 0 (WSON and Flexi-grid) tunnels are under definition in {{!I-D.ietf-ccamp-layer0-types-ext}} and re-used in the ietf-wson-path-computation and ietf-flexi-grid-path-computation YANG models

{: #optical-te-bandwidh}

## Bandwidth Augmentation

As described in Section 4.2 of {{!RFC7699}}, there is some overlap
between bandwidth and label in layer0.

The WSON and flexi-grid label resource information described in {{optical-te-label}},
is sufficient to describe also the spectrum resources within WSON and
flexi-grid networks. Therefore, the model does not define any augmentation
for the te-bandwidth containers defined in {{!I-D.ietf-teas-yang-path-computation}}.

The OTN path computation model augments all the occurrences of the te-bandwidth container
with the OTN technology-specific attributes using the otn-link-bandwidth and otn-path-bandwidth groupings defined in {{!I-D.ietf-ccamp-layer1-types}}.

{: #optical-te-label}

## Label Augmentations

The models augment all the occurrences of the label-restriction list
with WSON, Flexi-grid and OTN technology-specific attributes using the 
l0-label-range-info and flexi-grid-label-range-info groupings defined in {{!RFC9093}} and the
otn-label-range-info grouping defined in {{!I-D.ietf-ccamp-layer1-types}}.

Moreover, the models augment all the occurrences of the te-label
container with the WSON, Flexi-grid and OTN technology-specific attributes using the
wson-label-start-end, wson-label-hop, wson-label-step,
flexi-grid-label-start-end, flexi-grid-label-hop and flexi-grid-label-step defined in {{!RFC9093}} and the
otn-label-start-end, otn-label-hop and otn-label-step groupings defined in {{!I-D.ietf-ccamp-layer1-types}}.

{: #optical-pc-tree}

# Optical Path Computation Tree Diagrams

{: #wson-pc-tree}

## WSON Path Computation Tree Diagrams

{{fig-wson-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-wson-path-computation.yang.

~~~~
{::include ./ietf-wson-path-computation.tree}
~~~~
{: #fig-wson-pc-tree title="WSON path computation tree diagram"}

{: #flexg-pc-tree}

## Flexi-grid Path Computation Tree Diagrams

{{fig-flexg-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-flexi-grid-path-computation.yang.

~~~~
{::include ./ietf-flexi-grid-path-computation.tree}
~~~~
{: #fig-flexg-pc-tree title="Flexi-grid path computation tree diagram"}

{: #otn-pc-tree}

## OTN Path Computation Tree Diagrams

{{fig-otn-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-otn-path-computation.yang.

~~~~
{::include ./ietf-otn-path-computation.tree}
~~~~
{: #fig-otn-pc-tree title="OTN path computation tree diagram"}

{: #optical-pc-yang}

# YANG Models for Optical Path Computation

{: #wson-pc-yang}

## YANG Model for WSON Path Computation

~~~~
<CODE BEGINS> file "ietf-wson-path-computation@2022-03-07.yang"
{::include ./ietf-wson-path-computation.yang}
<CODE ENDS>
~~~~
{: #fig-wson-pc-yang title="WSON path computation YANG module"}

{: #flexg-pc-yang}

## YANG Model for Flexi-grid Path Computation

~~~~
<CODE BEGINS> file "ietf-flexi-grid-path-computation@2022-03-07.yang"
{::include ./ietf-flexi-grid-path-computation.yang}
<CODE ENDS>
~~~~
{: #fig-flexg-pc-yang title="Flexi-grid path computation YANG module"}

{: #otn-pc-yang}

## YANG Model for OTN Path Computation

~~~~
<CODE BEGINS> file "ietf-otn-path-computation@2021-10-07.yang"
{::include ./ietf-otn-path-computation.yang}
<CODE ENDS>
~~~~
{: #fig-otn-pc-yang title="OTN path computation YANG module"}

# Manageability Considerations

  TBD. 

# Security Considerations

  \<Add any security considerations>

# IANA Considerations

   This document registers the following URIs in the "ns" subregistry
   within the "IETF XML registry" {{!RFC3688}}.

~~~~
  URI: urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
  Registrant Contact:  The IESG.
  XML: N/A, the requested URI is an XML namespace.

  URI: urn:ietf:params:xml:ns:yang:ietf-wson-path-computation
  Registrant Contact:  The IESG.
  XML: N/A, the requested URI is an XML namespace.

  URI: urn:ietf:params:xml:ns:yang:ietf-flexi-grid-path-computation
  Registrant Contact:  The IESG.
  XML: N/A, the requested URI is an XML namespace.
~~~~

   This document registers the following YANG module in the "YANG Module Names"
   registry {{!RFC7950}}.

~~~~
  name:      ietf-otn-path-computation
  namespace: urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
  prefix:    otn-pc
  reference: this document

  name:      ietf-wson-path-computation
  namespace: urn:ietf:params:xml:ns:yang:ietf-wson-path-computation
  prefix:    wson-pc
  reference: this document

  name:      ietf-flexi-grid-path-computation
  namespace: ietf:params:xml:ns:yang:ietf-flexi-grid-path-computation
  prefix:    flexg-pc
  reference: this document
~~~~

--- back

{: numbered="false"}

# Acknowledgments

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

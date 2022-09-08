---
coding: utf-8

title: YANG Data Models for requesting Path Computation in Optical Networks

abbrev: Yang for Optical Path Computation
docname: draft-gbb-ccamp-optical-path-computation-yang-02
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

This document provides a mechanism to request path computation in Optical Networks (WSON and Flexi-grid) by augmenting the Remote Procedure Calls (RPCs) defined in RFC YYYY.

\[RFC EDITOR NOTE: Please replace RFC YYYY with the RFC number of
draft-ietf-teas-yang-path-computation once it has been published.

--- middle

# Introduction

{{!I-D.ietf-teas-yang-path-computation}} describes key use cases, where a client needs to request
underlying SDN controllers for path computation. In some of these use cases, the
underlying SDN controller can control a single-layer optical technologies, including 
Optical Transport Network (OTN), Wavelength Switched Optical Networks (WSON), Flexi-grid, 
and multi-layer Optical network.

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
  {{optical-pc-tree}} of this document.  The meaning of the symbols in these
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
| wson-pc      | ietf-wson-path-computation       | RFCXXXX         
| flexg-pc     | ietf-flexi-grid-path-computation | RFCXXXX         
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-rfc9093-bis}}.
Please replace KKKK with the RFC number assigned to {{!I-D.ietf-teas-yang-te}}.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-teas-yang-path-computation}}.
Please remove this note.

# YANG Data Models for Optical Path Computation 

## YANG Models Overview

The YANG data models for requesting WSON and Flexi-grid path computation are defined as augmentations of the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, as shown in {{fig-optical-pc}}.

~~~~ ascii-art
                    +--------------------------+    o: augment
       TE generic   | ietf-te-path-computation |
                    +--------------------------+
                          o             o
                          |             |
                          |             |
              +-----------+             +-----------+
              |                                     |
              |                                     |
+----------------------------+  +----------------------------------+
| ietf-wson-path-computation |  | ietf-flexi-grid-path-computation |
+----------------------------+  +----------------------------------+
            WSON                            Flexi-grid

~~~~
{: #fig-optical-pc title="Relationship between WSON, Flexi-grid and TE path computation models" artwork-name="optical-path-computation.txt"}

The entities and Traffic Engineering (TE) attributes, such as requested path and tunnel attributes, defined in {{!I-D.ietf-teas-yang-path-computation}}, are still applicable when requesting WSON and Flexi-grid path computation and the models defined in this document only specifies the additional technology-specific attributes/information, using the attributes defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

## Attributes Augmentation

The common characteristics for layer 0 (WSON and Flexi-grid) tunnels are under definition in {{!I-D.ietf-ccamp-rfc9093-bis}} and re-used in the ietf-wson-path-computation and ietf-flexi-grid-path-computation YANG models

{: #optical-te-bandwidh}

## Bandwidth Augmentation

As described in Section 4.2 of {{!RFC7699}}, there is some overlap
between bandwidth and label in layer0.

The WSON and flexi-grid label resource information described in {{optical-te-label}},
is sufficient to describe also the spectrum resources within WSON and
flexi-grid networks. Therefore, the model does not define any augmentation
for the te-bandwidth containers defined in {{!I-D.ietf-teas-yang-path-computation}}.

{: #optical-te-label}

## Label Augmentations

The models augment all the occurrences of the label-restriction list
with WSON and Flexi-grid technology-specific attributes using the 
l0-label-range-info and flexi-grid-label-range-info groupings defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

Moreover, the models augment all the occurrences of the te-label
container with the WSON, Flexi-grid and OTN technology-specific attributes using the
wson-label-start-end, wson-label-hop, wson-label-step,
flexi-grid-label-start-end, flexi-grid-label-hop and flexi-grid-label-step defined in {{!I-D.ietf-ccamp-rfc9093-bis}}.

{: #optical-pc-tree}

# Optical Path Computation Tree Diagrams

{: #wson-pc-tree}

## WSON Path Computation Tree Diagrams

{{fig-wson-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-wson-path-computation.yang.

~~~~ ascii-art
{::include ./ietf-wson-path-computation.tree}
~~~~
{: #fig-wson-pc-tree title="WSON path computation tree diagram" artwork-name="ietf-wson-path-computation.tree"}

{: #flexg-pc-tree}

## Flexi-grid Path Computation Tree Diagrams

{{fig-flexg-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-flexi-grid-path-computation.yang.

~~~~ ascii-art
{::include ./ietf-flexi-grid-path-computation.tree}
~~~~
{: #fig-flexg-pc-tree title="Flexi-grid path computation tree diagram" artwork-name="ietf-flexi-grid-path-computation.tree"}

{: #optical-pc-yang}

# YANG Models for Optical Path Computation

{: #wson-pc-yang}

## YANG Model for WSON Path Computation

~~~~ yang
{::include ./ietf-wson-path-computation.yang}
~~~~
{: #fig-wson-pc-yang title="WSON path computation YANG module" sourcecode-markers="true" sourcecode-name="ietf-wson-path-computation@2022-09-08.yang"}

{: #flexg-pc-yang}

## YANG Model for Flexi-grid Path Computation

~~~~ yang
{::include ./ietf-flexi-grid-path-computation.yang}
~~~~
{: #fig-flexg-pc-yang title="Flexi-grid path computation YANG module" sourcecode-markers="true" sourcecode-name="ietf-flexi-grid-path-computation@2022-09-08.yang"}

# Manageability Considerations

  TBD. 

# Security Considerations

  \<Add any security considerations>

# IANA Considerations

   This document registers the following URIs in the "ns" subregistry
   within the "IETF XML registry" {{!RFC3688}}.

~~~~
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

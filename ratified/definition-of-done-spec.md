---
seo:
  title: HPE GreenLake Developer Specification for Definition of Done | HPE GreenLake Platform
toc:
  enable: true
---

# Quality Working Group Business Feature Definition of Done

The Definition of Done is the commitment by the Developers for the
Increment, much like the Sprint goal is the commitment by the Developers
for the Sprint Backlog and the Product Goal is the commitment by the
Product Owner for the Product Backlog.  
  
The Definition of Done includes all the characteristics and standards an
Increment needs to meet to be released.

The [Scrum
Guide](https://www.scrum.org/resources/scrum-guide?utm_source=google&utm_medium=adwords&utm_id=psmii&adgroup=%7bgroupid%7d&gad_source=1&gclid=EAIaIQobChMI2ZeQkajehwMV20t_AB1qkAENEAAYASAAEgJxUvD_BwE)
says the Definition of Done is a formal description of the state of the
Increment when it meets the quality measures required for the product.
Once the Definition of Done is met, the Increment is Done and can be
delivered.

The Definition of Done creates transparency by providing everyone a
shared understanding of what work was completed and what standards were
met as part of the Increment. If a Product Backlog item does not meet
the Definition of Done, it cannot be released yet. Think of the
Definition of Done as the standard set for the products delivered.

Sometimes the Definition of Done for an Increment includes the standards
of the organization. In that case, all Scrum Teams must follow these
standards as a minimum. They can elaborate on it with any other
standards or characteristics that need to be met for the product. If
there are not specific organizational standards, the Scrum Team must
create those elements of Done appropriate for the product/service.

The [Definition of Done Policy](../policies/definition-of-done-policy.md)
describes which parts of HPE this specification applies to, provides additional
benefits for adhering to the Definition of Done, and lists related
standards.

The next table below defines the key areas required to be understood, accounted for,
and completed to develop enterprise-quality products and services.

<!-- Markdown does not support complex tables, so this one has been coded in HTML. -->
<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 39%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Criterion</strong></th>
<th style="text-align: center;"><strong>Definition</strong></th>
<th style="text-align: center;"><strong>Requirement</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Acceptance Criteria Met</td>
<td>Acceptance criteria are the conditions and/or targets that must be
satisfied for a product, user story, or increment of work to be
accepted.</td>
<td><ul>
<li><p>All documented acceptance criteria have been demonstrated to be met and signed off as completed by stakeholders.</p></li>
</ul></td>
</tr>
<tr>
<td>Unit Test Code Coverage</td>
<td>A unit test exercises the smallest piece of testable software in the
application to determine whether it behaves as expected.</td>
<td><ul>
<li><p>The minimum code line coverage threshold must be defined, met, or
exceeded for all new code merged to the mainline branch.</p></li>
</ul></td>
</tr>
<tr>
<td>Defects</td>
<td>A defect occurs when the product or a function of the product does not
perform as specified or required by the customer. The customer could be
an internal user, a service or support partner, or the end user of the
product.</td>
<td><ul>
<li><p>All defects must be submitted to the product/service defect
management system.</p></li>
<li><p>No open release showstopper (catastrophically bad for any
customer) defects attributable to the developed feature or defects that
regress available customer capability unless reviewed and approved per
established business organizational exception process. Approval requires
a fix delivery plan.</p></li>
</ul></td>
</tr>
<tr>
<td>Code Review</td>
<td>Code reviews are methodical assessments of code designed to identify
bugs, increase code quality, and help developers learn the source
code.</td>
<td><ul>
<li><p>Code reviews must comply with the <a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/peer-reviews/">Peer
Code Review Standard</a>.</p></li>
</ul></td>
</tr>
<tr>
<td>Continuous Integration (CI)</td>
<td>Continuous integration is the automatic and frequent integration and
testing of code changes.</td>
<td><ul>
<li><p>Automated unit tests must be executed as part of CI.</p></li>
<li><p>The integration of code into the mainline/release branch must be
gated at a 100% unit test pass rate.</p></li>
</ul></td>
</tr>
<tr>
<td>Functional and End-to-end Test Strategies, Cases, Execution</td>
<td><p>Functional testing verifies that the component(s) under test
meets business goals irrespective of the component architecture. The
component is treated as a black box.</p>
<p>Feature use cases / user journeys tests verify that the system meets
business goals irrespective of the component architecture. The system is
treated as a black box and the tests exercise as much of the fully
deployed system as possible, manipulating it through public interfaces
such as graphical user interfaces (GUIs) and service application
programming interfaces (APIs).</p></td>
<td><ul>
<li><p>Feature and end-to-end test strategies must be documented and
reviewed by stakeholders.</p></li>
<li><p>Test cases exercising all feature use cases / user journeys /
acceptance criteria must be documented and executed in an integrated
context in a traceable and auditable way.</p></li>
<li><p>100% of planned tests must be executed.</p></li>
<li><p>Aggregate and unique test case pass rate must be &gt;= 95%.</p></li>
</ul></td>
</tr>
<tr>
<td>Enterprise-Ready Non-Functional Requirements (NFRs)</td>
<td>NFRs are a system's characteristics not related to specific functionality or
behavior describing how it should perform, rather than what it should
do. They are overall properties of the system or of a particular
aspect.</td>
<td><ul>
<li><p>Enterprise-ready feature NFRs must
be validated to their documented expectations.</p></li>
<blockquote>
<p><strong>NOTE</strong>: For definitions of these terms, see the
<a href="#nfr-definitions">Non-Functional Requirements Definitions Reference</a> table below.</p>
</blockquote>
<ul>
<li><p><a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/accessibility/">Accessibility</a></p></li>
<li><p>Fault Tolerance</p></li>
<li><p>Monitorability</p></li>
<li><p>Observability</p></li>
<li><p>Performance</p></li>
<li><p>Reliability</p></li>
<li><p>Resiliency</p></li>
<li><p>Scalability</p></li>
<li><p>Supportability/Serviceability</p></li>
<li><p>Testability</p></li>
<li><p>Updatability</p></li>
<li><p>Upgradability</p></li>
<li><p>Usability</p></li>
</ul></ul></td>
</tr>
<tr>
<td>Regression Tests</td>
<td>Regression tests are designed to provide an understanding of how the code changes
being introduced unintentionally impact previously delivered code /
consumer value / experience including from the NFR perspective.</td>
<td><ul>
<li><p>Existing product regression tests currently used for quality
assessment at any level (unit, integration, …) must be evaluated for
relevance (based on the scope and impact of changed/introduced code) and
either updated or obsoleted.</p></li>
<li><p>Product regression test cases must exist for future quality
assessment of the delivered feature value.</p></li>
<li><p>100% of relevant legacy and new regression tests must be
executed.</p></li>
<li><p>Unique product regression test case pass rate must be 100%. Refer
to the Defect section.</p></li>
<li><p>Regression tests must be automated to the extent that they
produce a positive business return on investment (ROI) and be integrated
into the automated regression test suite(s).</p>
<ul>
<li><p>Any unautomated regression tests must be integrated with the
manual regression test suite(s).</p></li>
</ul></li>
</ul></td>
</tr>
<tr>
<td>Consumer Documentation</td>
<td><p>Consumer documentation provides information that describes the
feature/service to the people who deploy, support, and use it to
accomplish their objectives.</p>
<p>Examples of consumer documentation include:</p>
<ul>
<li><p>User interface (UI) help</p></li>
<li><p>Whitepapers</p></li>
<li><p>Configuration guides</p></li>
<li><p>Functional user guides</p></li>
<li><p>Quick specs</p></li>
<li><p>Reference architecture</p></li>
<li><p>Application programming interface (API) documentation</p></li>
<li><p>Runbook</p></li>
<li><p>Training material</p></li>
</ul>
<p>Examples of content include:</p>
<ul>
<li><p>Configuration, usage, best practices, and
troubleshooting of the feature/service</p></li>
<li><p>Documentation of (changes to) feature capabilities (e.g.,
NFRs, functional, input/output operations per second (IOPS), …) and defect fixes</p></li>
<li><p>Identification of constraints/boundaries</p></li>
<li><p>Information required by support and service teams</p></li>
<li><p>How data and data-related sources will be used</p></li>
</ul>
<p>A document can be provided in various formats, such as paper, digital, video, or as part of a user experience (UX).</p></td>
<td><ul>
<li><p>Documentation artifacts are created and updated with change
control to reflect the:</p>
<ul>
<li><p>Method/means to deploy, support, and use the new feature/service.</p></li>
<li><p>Changes to any currently documented method/means to deploy,
support, and use legacy features.</p></li>
</ul></li>
<li><p>Stakeholder reviews of changes must be completed, and all
feedback prioritized and incorporated per business unit
process.</p></li>
</ul></td>
</tr>
<tr>
<td>Internal (Governance) Documentation</td>
<td><p>Internal documentation provides information that
describes the formal structures and processes that align development
activities with the business strategy of HPE.</p>
<p>Examples of internal documentation include:</p>
<ul>
<li><p>Software development lifecycle (SDLC)</p></li>
<li><p>Secure software development lifecycle (SSDLC)</p></li>
<li><p>Requirements / product requirements document (PRD)</p></li>
<li><p>Architectural/design</p></li>
<li><p>Legal/compliance</p></li>
<li><p>Test plan and results</p></li>
<li><p>Artifacts that are HPE records</p></li>
</ul>
<p>A document can be provided in various formats, such as paper, digital, video, or as part of a UX.</p></td>
<td><ul>
<li><p>Documentation artifacts are created and updated with change
control to ensure compliance with HPE records management, <a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/ssdf-policy/">SSDLC</a>
and program processes/requirements/standards.</p></li>
<li><p>Stakeholder reviews of changes must be completed, and all
feedback must be prioritized and incorporated per business unit
process.</p></li>
</ul></td>
</tr>
<tr>
<td>Security</td>
<td>Product/Service security refers to the strategies, processes, and
practices employed to protect software, hardware, and digital services
from malicious attacks, data breaches, data loss, and vulnerabilities,
as well as non-hostile events such as hardware failures, software
crashes, data corruption, network outages, and other security threats /
risks.</td>
<td><ul>
<li><p>Risk assessment, architecture, design, coding, verification,
mitigation, remediation, and exception handling must comply with the
documented security standards and associated governance:</p></li>
<ul>
<li><p><a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/secure_design_and_architecture_policy/">Secure
Architecture and Design</a></p></li>
<li><p><a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/secure-coding/">Secure
Coding and Review</a></p></li>
</ul></ul>
</td>
</tr>
<tr>
<td>UX</td>
<td>UX is the method/means of how a user interacts with
and experiences a product, or service. It includes a person's
perceptions of utility, ease of use, and efficiency.</td>
<td><ul>
<li><p>Each UX must be designed per documented business unit standards.</p></li>
<li><p>UX assessment must be completed by business-designated subject matter
experts.</p></li>
<li><p>Each UX implementation plan, including any exceptions, must be approved per
established business organizational process and governance.</p></li>
</ul></td>
</tr>
<tr>
<td>Consumer-facing APIs</td>
<td>An API is a set of rules/instructions that enables software
components/applications to communicate with each other in a
structured, secure, reliable, and well-defined manner to exchange data
and provide services.</td>
<td><ul>
<li><p>APIs must comply with the <a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/api/">API
Design Standard</a> and associated governance.</p></li>
</ul></td>
</tr>
</tbody>
</table>

## Non-Functional Requirements Definitions Reference

The following table defines the terms used in the NFR row of the previous table.

<!-- Markdown does not support complex tables, so this one has been coded in HTML. -->
<table id="nfr-definitions">
<colgroup>
<col style="width: 23%" />
<col style="width: 76%" />
</colgroup>
<thead>
<tr>
<th><strong>Non-Functional Requirement</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Accessibility</td>
<td>See the <a
href="https://developer.greenlake.hpe.com/docs/greenlake/standards/policies/accessibility/">Accessibility Guidelines</a> developer policy.</td>
</tr>
<tr>
<td>Fault tolerance</td>
<td>The system's measured ability to continue operating uninterrupted
despite the failure of one or more of its components.</td>
</tr>
<tr>
<td>Monitorability</td>
<td>The system’s measured ability to assess the component/product
health, from an internal perspective.</td>
</tr>
<tr>
<td>Observability</td>
<td>The ability to assess the component/product health, from external,
measurable, perspective(s).</td>
</tr>
<tr>
<td>(Characterization of) Performance</td>
<td><ul>
<li><p>Characterization: Understanding of a feature’s behavior in
isolation.</p></li>
<li><p>The system’s measured ability to function under defined
conditions (e.g., throughput, IOPS,
millions of instructions per second (MIPS), latencies, …).</p></li>
</ul></td>
</tr>
<tr>
<td>Reliability</td>
<td>The system’s ability to function, given environmental conditions,
for a particular amount of time.</td>
</tr>
<tr>
<td>Resiliency</td>
<td>The system’s measured ability to maintain an acceptable service
level in the face of faults and challenges (e.g., infra, …).</td>
</tr>
<tr>
<td>Scalability</td>
<td>The system’s ability to increase its capacity and functionalities
based on its users’ demand while remaining stable (without experiencing
crashes, errors, or failures).</td>
</tr>
<tr>
<td>Supportability/serviceability</td>
<td><ul>
<li><p>The system’s ability to be supported and maintained.</p></li>
<li><p>The system's ability to fully enable the support organization
to use available mechanisms to aid customers as required.</p></li>
</ul></td>
</tr>
<tr>
<td>Testability</td>
<td>The system’s ability to support reliable mechanisms to validate
established criteria and targets.</td>
</tr>
<tr>
<td>Updatability</td>
<td>The ability to introduce enhancements to existing capabilities.</td>
</tr>
<tr>
<td>Upgradability</td>
<td>The ability to introduce future new capabilities.</td>
</tr>
<tr>
<td>Usability</td>
<td>The ability of a specified consumer to configure, manage, and use
the product or service intuitively, easily, and quickly.</td>
</tr>
</tbody>
</table>

**Original publication date (yyyy-mm-dd):** 2024-09-06

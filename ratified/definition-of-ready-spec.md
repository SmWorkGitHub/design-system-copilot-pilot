---
seo:
  title: HPE GreenLake Developer Specification for Definition of Ready | HPE GreenLake Platform
toc:
  enable: true
---

# Quality Working Group Business Feature Definition of Ready

The Business Feature Definition of Ready outlines the necessary
conditions that must be fulfilled for a business feature to be
considered sufficiently detailed and understood by the delivery team,
such that it is actionable and can be committed for delivery into a
release increment.

The [Definition of Ready Policy](../policies/definition-of-ready-policy.md)
describes which parts of HPE this specification applies to.

The delivery team is the group of all (Engineering, System/Solution
Test, Documentation, Product, Support, etc.) individuals who contribute
to providing/defining what is required for a business feature to be
sufficiently detailed and understood such that it is actionable and can be
committed for delivery into a release increment. Examples include:

- Scope, business case, priority

- Criteria for acceptance/completion

- Work breakdown and estimations of the work

- Engineering dependencies and risks

Business features are focused on delivering customer-facing value,
whereas enabler features focus on building the foundational capabilities
that allow the business features to be developed. Business features
directly benefit the business, users, or customers and represent
requirements that deliver value.

The expected benefits of this standard include:

- Avoids beginning work that does not have clearly defined completion
  criteria, thus removing vacillation and/or rework, ultimately
  contributing to improved business productivity, execution efficiency,
  and value delivery.

- Ensures a shared understanding of the value to be delivered,
  high-level cross-functional tasks, dependencies, and promotes team and
  organizational alignment. The shared understanding will lead to more
  holistic, inclusive, and improved estimates which will lead to better
  (feature release, capacity) planning, predictability, and quality.

- Assists in identifying potential issues or risks early, giving teams
  more time to plan and manage them effectively.

- Provides a mechanism for the delivery team to request further
  clarification of the requirements before finalizing forecasts based on
  estimations.

A feature must be:

- Independent: As much as possible, a feature should have little or
  no dependence on other features.

- Negotiable: The documented level of detail must allow for
  conversation. Room needs to be left for discussion regarding its
  optimal implementation. The feature definition was iterated on with
  the appropriate engineering and cross-functional teams.

- Valuable: The value that will be added through the creation of
  this feature must be clearly defined. The team understands the “why”
  this needs to be done, the “what” that needs doing, and the expected
  customer impact.

- Estimable: The feature contains sufficient information for the
  delivery team to determine its relative complexity and the effort and
  elapsed time required to deliver.

- Small: The estimations and allowances for discovery must show that
  the feature can be completed to support phased customer value
  delivery.

- Testable: There must be reasonable, reliable ways for the team to
  be able to validate meeting the specified acceptance criteria.

The terms Independent, Negotiable, Valuable, Estimable, Small, and Testable make
up the acronym "INVEST."

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 29%" />
<col style="width: 47%" />
</colgroup>
<thead>
<tr>
<th><strong>Criterion</strong></th>
<th><strong>Definition</strong></th>
<th><strong>Requirement</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Value Description &amp; Business
Objective</strong></td>
<td>Description of the customer and business value proposition, goals,
and objectives to enable a shared organizational understanding.</td>
<td><p>A high-level description of the business objective(s) and the
value that will be delivered upon feature completion must be documented
and articulated:</p>
<ul>
<li><p>Who: Which customer segment(s)/persona(s) is/are being
targeting?</p></li>
<li><p>What: The high-level business objective and goals for the
customer and HPE. What will the consumer have / what will be different
for them after completion of the feature? How will their experience be
different?</p></li>
<li><p>Why: The customer/business benefit of the feature,
highlighting:</p>
<ol type="1">
<li><p>Business importance and alignment (strategic, operational, etc.)
and risks of not doing</p></li>
<li><p>Specific needs/problems/risks it will solve, prevent or
mitigate</p></li>
<li><p>How the feature will address the needs/problems/risks</p></li>
<li><p>Impact of not doing</p></li>
</ol></li>
</ul>
<p>All available supporting artifacts should be linked or referenced,
such as customer interviews, product requirements document (PRD), market
analyses, metrics, customer request/customer found issue IDs, high-level
designs, etc.</p></td>
</tr>
<tr>
<td><strong>Acceptance
Criteria</strong></td>
<td>Acceptance criteria are the conditions and/or targets that must be
satisfied for a product, user story, or increment of work to be
accepted.</td>
<td><p>Acceptance criteria must:</p>
<ul>
<li><p>Be clearly and concisely documented.</p></li>
<li><p>Authored as verifiable, demonstrable, unambiguous persona-focused
solution- / use case- / customer workflow-based outcomes including:</p>
<ul>
<li><p>Positive, negative, and boundary expectations.</p></li>
<li><p>Relevant, non-functional, enterprise-ready*, requirements that
are either documented or HPE/organizational policy. (*See the next table
for a description of enterprise-ready requirements.)</p></li>
<li><p>New / changes to / interactions with previously delivered
customer value.</p></li>
</ul></li>
<li><p>Contain clearly defined pass or fail outcomes.</p></li>
<li><p>Focused on the outcome itself and not how it is
achieved.</p></li>
</ul></td>
</tr>
<tr>
<td><strong>Work Identification &amp;
Breakdown</strong></td>
<td>The high-level decomposition of the total scope of work required to
deliver the feature into actionable elements. Additional detailed
discovery will occur during feature development.</td>
<td><p>The delivery team, using their shared understanding of the
feature, must identify and document, in a transparent manner, the work
items required to complete all the following:</p>
<ul>
<li><p>Fulfill the feature acceptance criteria</p></li>
<li><p>Deploy the artifact(s)</p></li>
<li><p>Onboard the customer</p></li>
<li><p>Support the feature</p></li>
<li><p>Meet the Feature Definition of Done</p></li>
</ul></td>
</tr>
<tr>
<td><strong>Priority</strong></td>
<td>Prioritization is the process by which potential development items
are ranked in order of importance and intended application of resources
based on business value, customer needs, and alignment with strategic
objectives.</td>
<td><p>Prioritization will consider customer value, urgency, and
alignment with business goals.</p>
<p>Priorities may be adjusted based on effort and time to implement.</p>
<p>Relative scoring of the feature must be arrived at and documented per
business unit framework/process (e.g., RICE, Kano, WSJF, stack ranking,
etc.) in a transparent manner.</p></td>
</tr>
<tr>
<td><strong>Dependencies &amp;
Assumptions</strong></td>
<td><p>A dependency is a logical, constraint-based or preferential
relationship between two activities. These may be:</p>
<ul>
<li><p>Internal—within the team’s control</p></li>
<li><p>External—outside of the team’s control</p></li>
</ul>
<p>An assumption is a belief that is taken as being true for the
purposes of planning.</p></td>
<td>Significant dependencies and assumptions must be identified,
analyzed, communicated, aligned (accepted), documented, and accounted
for across all feature development contributor’s estimations, iteration
plans, and roadmap.</td>
</tr>
<tr>
<td><strong>Engineering Risks</strong></td>
<td>Engineering risks are uncertain events or conditions that can have a
negative impact to the feature delivery plan (e.g., scope, schedule,
cost, or quality).</td>
<td><p>Significant engineering risks must be documented in a transparent
manner per business unit processes and should consider and/or
include:</p>
<ul>
<li><p>Description, including type (schedule, environmental, technical,
etc.)</p></li>
<li><p>Owner</p></li>
<li><p>Impact of occurrence</p></li>
<li><p>Probability of occurrence</p></li>
<li><p>Priority to mitigate</p></li>
<li><p>Triggers</p></li>
<li><p>Known mitigations, resolutions, or workarounds</p></li>
</ul></td>
</tr>
<tr>
<td><strong>Estimation/Sizing</strong></td>
<td>Feature estimation/sizing is the process of assessing the effort and
complexity involved in delivering the work items identified in the “Work
Identification &amp; Breakdown” section of this standard, accounting for
all associated engineering and cross-functional activities, assessments,
risks and dependencies.</td>
<td>All feature work items identified in the “Work Identification &amp;
Breakdown” section of this standard must be estimated (sized) at a
gross/high level and documented by the delivery team per business unit
process. It is understood that not all details will be known at the time
of estimation.</td>
</tr>
</tbody>
</table>

## Non-Functional, Enterprise-Ready Requirements Definitions Reference to Evaluate for Relevancy

The following table defines the non-functional enterprise-ready
requirements.

<table>
<colgroup>
<col style="width: 39%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr>
<th><strong>Non-Functional
Requirement</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Fault Tolerance</td>
<td>The system's measured ability to continue operating uninterrupted
despite the failure of one or more of its components.</td>
</tr>
<tr>
<td>Monitorability</td>
<td>The system’s measured ability to assess the health of the
component/product from an internal perspective.</td>
</tr>
<tr>
<td>Observability</td>
<td>The ability to assess the health of the component/product from an
external, measurable perspective.</td>
</tr>
<tr>
<td>(Characterization of) Performance</td>
<td><ul>
<li><p>Characterization: Understanding of a feature’s behavior in
isolation.</p></li>
<li><p>The system’s measured ability to function under defined
conditions (e.g., throughput, IOPS, MIPS, latencies, etc.).</p></li>
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
level in the face of faults and challenges (infra, etc.).</td>
</tr>
<tr>
<td>Scalability</td>
<td>The system’s ability to increase its capacity and functionalities
based on its users’ demand while remaining stable (without experiencing
crashes, errors, or failures).</td>
</tr>
<tr>
<td>Security</td>
<td>Product/service security refers to the strategies, processes, and
practices employed to protect software, hardware, and digital services
from malicious attacks, data breaches, data loss, and vulnerabilities,
as well as non-hostile events such as hardware failures, software
crashes, data corruption, network outages, and other security
threats/risks.</td>
</tr>
<tr>
<td>Supportability/Serviceability</td>
<td><ul>
<li><p>The system’s ability to be supported and maintained.</p></li>
<li><p>The system’s ability to fully enable the Support organization to
use available mechanisms to aid customers as required.</p></li>
</ul></td>
</tr>
<tr>
<td>Testability</td>
<td>The system’s ability to support reliable mechanisms for validating
established criteria and targets.</td>
</tr>
<tr>
<td>Updatability</td>
<td>The ability to introduce enhancements of existing capabilities.</td>
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

**Original publication date (yyyy-mm-dd):** 2024-11-18

---
seo:
  title: Production Readiness - Availability | HPE GreenLake Platform
toc:
  enable: true
---

# Availability

Availability is a critical measure of system performance, ensuring that a service remains accessible and functional for end users. To achieve high availability, a combination of active monitoring, redundancy, fault tolerance, and efficient incident management is required.

Teams building services MUST:

- Have a well-defined and automated production deploy process as described in [Push Small, Frequent Deploys and Automate as Much as Possible](#push-small-frequent-deploys-and-automate-as-much-as-possible).

- Have a well-defined and practiced procedure for dealing with an outage as described in [Define Outage Procedures](#define-outage-procedures).

Teams building services MUST NOT:

- Have a single point of failure as described in [No Single Point of Failure](#no-single-point-of-failure).

Teams building services SHOULD:

- Monitor downstream dependencies as described in [Monitor Downstream Services' Response Times](#monitor-downstream-services-response-times).

- Implement retries, fallback policies, and caching as necessary to protect themselves from dependencies as described in [Retries, Fallback, Cache for Dependency Failures](#implement-retries-fallbacks-or-caching-for-dependency-failures).

Use the following best practices to achieve high availability.

## Define Your Error Budget

**Error budget is the amount of unreliability you are willing to tolerate for your service offering.**

A service's **reliability** is measured by how well its customers see or *feel* about it. A service that is always up (100%) but can only successfully fulfill 75% of its customer requests is NOT a reliable one. On the other hand, a service that consistently returns correct results albeit slowly might be considered by its consumers to be more reliable than one that has unpredictable response rates.

A very popular and simple indicator that can be used for error budget is **total uptime**. A service might state that its total uptime (reliability) is 99.9% per month, which effectively leaves 0.1% for downtime (unreliability). This means every month the team might have up to 43 minutes of downtime that can be the consequence of introducing risks such as new product features, technical debts, or planned downtime, etc. into its development work.

Another indicator that can be used for error budget is **response time**. A service such as authentication/authorization might want to state that 99.95% of its requests should be handled and returned to clients within 50 ms. That means at most 0.05% of its requests can occasionally take more than 50 ms (unreliability). This 0.05% error budget could possibly translate to engineering work on testing a new encryption/decryption algorithm or implementing an internal cache to store results from dependencies.

To come up with a feasible error budget, a service team first need to identify its **SLOs** (Service Level Objective) and **SLIs** (Service Level Indicator). Then, error budget = 100% - SLO.

As is often the case, a service can have multiple SLOs. Hence, error budget is actually a set of allowed failures depending on the number of SLOs set by the service.

### References

[The Site Reliability Workbook: Implementing SLOs](https://sre.google/workbook/implementing-slos/) (O'Reilly/Google SRE)

[Site Reliability Engineering: Embracing Risk](https://sre.google/sre-book/embracing-risk/) (O'Reilly/Google SRE)

## Push Small, Frequent Deploys and Automate as Much as Possible

**Deploy processes define the methods by which new code lands in a production environment.**

To help teams maintain their SLO, production deploy procedures should have certain characteristics.

Production pipelines should enable small, frequent deploys. The amount of code or configuration change in any deploy should be minimized. Changing 10 lines of code at once incurs much less risk than changing 1000 lines. Batching large amounts of change should be avoided.

Human error is a common source of instability, so humans should be removed, as much as possible, from the production deploy process. Deployments should be automated and repeatable. Post-deploy validation should also be automated so that newly deployed code is always exercised. Humans may want to manually review the results of the post-deploy validation to decide on the next steps, but it should be considered a best practice to automatically roll back changes if validation fails.

A single deploy that effects all customers results in a huge amount of risk to production stability and business continuity. New versions should be slowly rolled out to customers so that problems can still be identified and fixed while they only have a small impact on a service’s SLO. Blue/green or canary deployment patterns should be considered best practice.

### References

[CircleCI Blog: Canary vs. blue-green deployment to reduce downtime](https://circleci.com/blog/canary-vs-blue-green-downtime/)

## Avoid Over-Reliability to Prevent Customer Complacency

**Services that significantly exceed their SLO can provide a false sense of security in services that depend on them.**

Services that consistently overperform can result in their customers assuming that the service will always be available and not planning for a dependency that can fail.

Services should ensure that they have some instability, either synthetic or organic, so that their consumers will be required to deal with potential failure and implement the appropriate remediations.

### References

[The Site Reliability Workbook: The Global Chubby Planned Outage](https://sre.google/sre-book/service-level-objectives/#xref_risk-management_global-chubby-planned-outage) (O'Reilly/Google SRE)

## Define Outage Procedures

**Outage Procedure is a set of procedures, processes, artifacts that help service teams to quickly response to an outage incident and bring back service reliability.**

An outage in its rawest form can be understood as any incident that renders the service completely useless to its customers. For example, your service website might still be up but when customers hit functional buttons, nothing happens. Or, all calls to your API return HTTP 401 code or authentication errors even though clients use correct credentials.

An outage procedure should have an incident workflow dedicated to such situations. This might include implementing PagerDuty schedules, escalation policies, multi-party conference calls/chats, post-mortem reviews, or root cause analysis reports. An outage procedure should also include incident run books, automated scripts, or step-by-step troubleshooting.

An outage procedure might want to first focus on limiting the impact before trying to bring back the service.

Starting with the software components, dependencies, and tools, teams will want to sit together to define what can possibly cause outages to their services.

Having a dedicated and clear PagerDuty schedule/escalation/workflow for outage level incidents is highly recommended.

Teams with well-written runbooks that are easily accessible and understood by all engineers typically respond well to outages.

Teams may also want to practice outage drills on a regular basis (weekly, bi-weekly, monthly) so that everyone builds a natural reflective habit when it comes to actual outages.

### References

[PagerDuty: What to do during a major incident](https://response.pagerduty.com/during/during_an_incident/)

[PagerDuty: Postmortem process](https://response.pagerduty.com/after/post_mortem_process/)

## Implement Retries, Fallbacks, or Caching for Dependency Failures

**Services should remediate or gracefully handle failures that might happen with its dependencies through the use of retries, fallbacks, or internal caching.**

Downstream failures can happen in many forms. As long as a service component has dependencies, be it another software component of the same service or an external entity, these dependencies can and will have failures.

The most commonly encountered downstream failure is network errors. A service team might want to implement some form of retries, such as exponential backoff, which works fairly well in cases of sporadic network issues.

Services can also implement fallback if they have more than one option for downstream dependencies. If the downstream component A fails, a service can switch to downstream component B provided B and A offer the same functionality.

Another popular way to reduce impact is to leverage caching of fairly static contents served by downstream. A service may need to only call its dependency once, then cache the result internally for subsequent calls.

Teams will want to review dependencies that they are currently relying on, including where, when, and how the calls are made, to come up with the appropriate approach. Start small with easy-to-implement solutions such as retries. More complex implementations might require changes at the architectural level, but it could be worth the effort if it helps maintain overall system reliability.

Teams should periodically test their resiliency by injecting instability between their systems and their dependencies. The outcome of those tests can highlight fragile interactions that need to be fixed.

### References

[PagerDuty: Chaos testing](https://www.pagerduty.com/resources/learn/what-is-chaos-testing/)

## No Single Point Of Failure

**A single point of failure (SPOF) is a system component which, upon failure, renders the entire system unavailable or unreliable.** Services need to be architected and designed with **failure and redundancy** in mind.

Modern-day systems are composed of various components. Without proper preparation and care, the failure of a component may have a cascading effect to many other parts, and in some cases can cripple the entire service.

To avoid these scenarios, service teams should consider in advance how their services might fail and, if possible, what redundancy can be implemented. AWS-based applications can leverage many highly available and highly scalable services provided out-of-the box by Amazon. For off-the-shelf products, teams should evaluate the best practices for reliability and scalability provided by the manufacturer. For those opting to implement their own solutions or running on-prem, you may want to consider if a form of redundancy (e.g. primary/secondary) can be put in place or the components can make up a cluster.

Teams will want to first identify potential SPOF risks using a checklist that assesses hardware, software, and people. This checklist should enable teams to focus on areas that are critical to their service reliability.

Teams will also want to constantly review their system load metrics for potential bottlenecks caused by rising load. Even with high availability enabled, any component may still crumble with overload. Hence, scaling needs to be enforced and adjusted accordingly.

## Monitor Downstream Services' Response Times

**A service needs to ensure the reliability of its dependencies.**

A service may have multiple dependencies; some are external from third-party providers, and some are internal. Even within a service that is composed of multiple microservices, it's common to see interdependence among its microservices.

Due to the nature of dependency, the reliability of a service is impacted by that of its dependency. For a service to react to changes downstream, it will need to be aware of the health of its dependencies. In other words, a service needs to have metrics and alerts regarding its downstream reliability.

**Response time** is a popular linear quality indicator that can be helpful for such purposes. However, the choice of metrics should be determined based on the specific requirements of the services.

Create a list of the internal and external dependencies of a service. A review of existing monitoring tools and metrics is also needed as this may mean adding more tools to the pipeline or creating more metrics and alerts.

The team will want to actively monitor their service’s dependencies based on the metrics it has put in place. This may lead to internal work to adjust services or collaboration with other teams or partners to tune the traffic accordingly.

## Quickly Detect and Recover from Data Integrity Issues

**Data integrity refers to the accuracy, consistency, completeness, and reliability of data over its lifecycle.**

The service team must quickly detect data integrity issues and have a repair and recovery plan.

Sanitization of data coming to your system is arguably the very first step to prevent data integrity issues from happening. Many databases also offer integrity checks which teams can leverage.

Depending on the nature of the applications, service teams may want to set up automated data checks that run on a regular basis. Even a routine manual check could help, though it is less ideal.

Teams should have runbooks or an ops bible dedicated to data recovery and make sure all on-call engineers are familiar with handling data through recovery fire drills.

Cloud providers such as AWS offer backup/recovery options for DynamoDB, RDS, etc. Teams will want to turn these features on and practice using them.

Start with data. Application logic is usually the origin of many data integrity issues. Wherever possible, enforce data checks as data is ingested and before it is processed by the application.

Data storage mechanism is another place to look after. Files stored on S3 will have different data integrity criteria from those in MySQL or ElasticSearch. Leverage whatever database providers can offer.

**Original publication date (yyyy-mm-dd):** 2025-01-27

---
seo:
  title: Production Readiness - Observability & Incident Management | HPE GreenLake Platform
toc:
  enable: true
---

# Observability and Incident Management

Ensuring production readiness involves adhering to observability and incident management guidelines. Key practices include monitoring service performance, maintaining availability metrics, and handling incidents promptly to meet Service Level Objectives (SLOs).

Teams building services MUST:

- Monitor and alert on the golden signals defined in [Google's SRE book](https://sre.google/sre-book/monitoring-distributed-systems/):

  - Latency - The time it takes to service a request.

  - Traffic - A measure of how much demand is being placed on your system, measured in a high-level system-specific metric.  

  - Errors - The rate of requests that fail, either explicitly (e.g., with HTTP 500), implicitly (e.g., an HTTP 200 success response, but coupled with the wrong content), or by policy (e.g., "If you committed to one-second response times, any request over one second is an error").

  - Saturation - How "full" your service is. A measure of your system fraction, emphasizing the resources that are most constrained (e.g., in a memory-constrained system, show memory; in an I/O-constrained system, show I/O). Note that many systems degrade in performance before they achieve 100% utilization, so having a utilization target is essential.  

- Provide availability metrics for the last 30 days.

Teams SHOULD:

- Use OpenTelemetry for instrumentation of application performance monitoring (APM), metrics, and logs where possible.

- Monitor services for situations that fall outside the standard signals. As an example, a service that is properly responding to resource creation requests may be operating technically correctly but if the request rate is far beyond the norm, it may still indicate a problem. Teams should detect anomalous behavior even when signals are still operating within tolerances.  

- Consider adopting [error budgets](https://handbook.gitlab.com/handbook/engineering/error-budgets/) to provide a clear, objective metric that determines how unreliable the service is allowed to be within a time window. Working to an error budget allows teams to take calculated risks to increase feature velocity while still maintaining their SLO.

## Observability Guidance

The following observability recommendations will help ensure that services maintain production readiness.

### Define and Monitor SLOs and SLIs

SLOs and SLIs are key concepts in observability that help organizations monitor their service performance.

**A Service Level Indicator (SLI) indicates how well a service is performing at any given moment in time.**

**A Service Level Objective (SLO) is the precise numerical target for system reliability: the target of the SLI aggregated over time.**

**SLIs** should map to user expectations, such as **response time**, **latency**, **quality**, **throughput**, etc. SLIs can be understood as (the number of good events / the number of valid events) \* 100%. This gives you a range of 0% to 100%, where 0 = nothing works and 100 = nothing is broken.  
  
A good place to start with SLIs is a service's monitoring and reporting systems. Prometheus, Grafana, AWS CloudWatch, OpsRamp, New Relic, and Coroot are good tools with lots of built-in or open-sourced library metrics that service teams can leverage to then drive to their own SLIs.  
  
**SLO** represents the point at which a service no longer meets the expectations of its users. Once SLIs are expressed as a percentage between 0% and 100%, SLOs should generally be just short of 100%, like 99.9%, 99.95%, or 99.99%.  
  
SLOs draw the line between happy and unhappy customers. A typical customer should be happy if you meet your SLO. Anything below your SLO will result in an unhappy customer, whose expectations for reliable service are not being met. So, being well within your SLOs, for example 99.9% \<= SLIs \< 100%, is a signal that you can move faster without causing user pains (error budget).  
  
From a product perspective it will be the work of POs/PMs, together with architects and engineers to essentially translate the business commitments to SLIs and SLOs. Without clear product requirements and a thorough team understanding, it's unlikely you'll develop effective SLOs for your service. To put it another way, SLIs and SLOs need to be understood and agreed upon by every single member of the team.

### Store 30 Days of Availability Metrics

**A service team needs to show its availability metrics for any time during the last 30 days.**

Teams should be able to prove that they have met their SLO at any time over a 30-day period.

### Correlate Critical Issues with SLO

**A service team needs to be able to trace SLO interruptions to critical incidents.**

SLO violations normally happen when an unexpected circumstance arises, severely degrading service quality. Having availability SLO metrics for the last 30 days helps address the questions: “What level has our SLO been at?” and “When did we violate the SLO?” Being able to tie a drop in our SLO metrics to a specific incident answers the question, “What caused our unreliability?”  
  
A well-documented RCA should be available as a reference for any interruption in service SLOs.

### Maintain Audit Logs for API Interactions

**Each service must maintain an audit trail of changes that have affected system state.**

Teams must generate and store audit logs for API interactions following the guidelines from [HPE GreenLake Audit Logging Standard.](https://developer.greenlake.hpe.com/docs/greenlake/standards/ratified/logging/audit_logging_guidelines/) Audit logs shall be made accessible to all engineers within the team.

### Centralize and Make Logs Searchable

**All service teams should use a centralized search engine and analytics solution for logging.**

Modern applications composed of different components utilize various logging solutions. Infrastructure logs in AWS are normally written to S3 and managed by AWS CloudWatch. Application-specific logs are usually dumped to local files. Some applications may choose to write to a remote destination directly. This diversity makes searching for a particular piece of information complicated and time consuming, especially when you must combine data from multiple sources for the complete picture.

### Create Dashboards with Key Metrics

**Service teams need to bring telemetry data together and present it analytically to help engineers troubleshoot, understand, and explore.**

Complex systems must be observed in a comprehensive and real time manner. Telemetry data—application metrics, logs, and traces—needs to be combined and transformed to provide engineers with the best insights about service reliability in real time.  
  
It is ideal to have a centralized place for operational dashboards to ensure a seamless experience, yet it's not uncommon for a service team to have multiple dashboards across different tools. However, service teams should strive for a consolidated observability solution when possible to improve efficiency.

### Review Metrics and Logs for Non-Alarm Anomalies Periodically

**Besides responding to incidents, service teams need to actively look for anomalies in its observability system such as logs, dashboards, reports that have not necessarily triggered incidents.**

While incidents generated by the monitoring system cover most error conditions a service may encounter in operation, there are still less obvious and potentially harmful situations that are unknown to service teams. For example, INFO log messages about dependencies are not typical candidates for alerting. But when you suddenly see a 10x increase of such messages when nothing really changes in the system this might be a good indicator that there is an anomaly somewhere worth looking into.  
  
Another example often overlooked is missing logging. Without logs, no one can really tell if there have been any errors with your application given that your log-based alarms have no input to work on.  
  
Choosing the right metrics for your dashboard and regularly reviewing them can give much insight about the health of your service. An average might not change much when there are small number of surges underneath, hence not noticeable. However, a percentile graph may show clearly that these surges seem to correlate to certain customers at certain times. These kinds of symptoms are normally not picked up by your alerting system, but they are good indicators that something abnormal is happening and worth checking.

Teams should schedule regular reviews of logs, dashboards, reports, runbooks, and other operational materials. Teams will want to keep adding new metrics, dashboards, alerts to help with new discoveries.

### Don't Trust a Service to Report on Itself

**A service should be watched by multiple independent monitoring systems to guarantee its reliability.**

Applications normally come with an in-house monitoring system running in the same infrastructure. Traditional machine-based applications use Nagios, Sensu, and the like on the same node or VM. CCP clusters run their own Prometheus setup in the same K8s cluster.

One interesting question about any monitoring system is, “What if your monitoring system dies?” When this happens, the best-case scenario is you lose visibility for a while until it comes backs up. The worst-case scenario is that your application was also down, and you had no clue or were unable to troubleshoot or restore service due to a lack of monitoring data, even after your monitoring tool came back to life.  
  
One way to address this issue is to use additional monitoring systems to at least provide the most crucial insights about your service reliability. External canaries can provide reliable indicators that your public service endpoints are up and responsive globally.  
  
Another approach that can help is to utilize an external system to monitor your monitoring system. The external monitor can alert a service team if their internal monitoring software is having issues, prompting necessary actions to mitigate or restore it.

## Incident Management Guidance

The following incident management recommendations will help ensure that services maintain production readiness.

### Implement 24/7 On-Call Rotation

**Service teams need to have 24/7 production support to maintain service reliability.**

On-call rotation is the first and arguably the most important step towards committing to reliability for HPE customers while also not burning out the team.

In essence, an on-call schedule ensures that the right people are always available, day and night, to quickly respond to incidents and outages. On the other hand, a good on-call rotation should share the workload evenly amongst team members, especially on weeknights, weekends, and holidays.  
  
For example, teams with members across different continents may employ a follow-the-sun schedule, which puts engineers in on-call mode only during local working hours while still covering 24-hour operations globally.  
  
For teams that are more centralized geographically, a daily or weekly schedule might be a better choice to spread the workload. Teams can also opt to shift-based if it makes sense to do so given the team dynamics.

Coming up with a good on-call rotation that sustains work-life balance requires input from all members of the team. After creating an initial on-call plan, teams will want to gradually adjust and refine it to work toward an optimal plan.  
  
Service teams may also want to have multiple schedules and policies to dynamically adapt to changes in working calendar or situations: sick leaves, member departures/arrivals, vacations, holidays, etc.  
  
As an example, service teams could leverage [PagerDuty](https://hpe-hcss.pagerduty.com/) to utilize its great support for building up [**on-call schedules**](https://support.pagerduty.com/docs/schedules) and [**escalation policies**](https://support.pagerduty.com/docs/escalation-policies).

#### References

- [PagerDuty: Best Practices for On Call Teams](https://goingoncall.pagerduty.com/)

### Service Teams SHOULD Manage and Act On *All* Incidents

**A service team needs to keep track of all incidents it has received and act on them accordingly in a timely fashion.**

When a service receives incidents, the on-call engineers need to work on addressing these based on the predetermined resolutions. High urgency or severe incidents usually require a quick response, while low urgency or less severe ones can be dealt with later within an acceptable time frame. However, **under no circumstance does the service team ignore any incidents regardless of severity**. If the incident can be ignored, or is not actionable, it should not be an incident.  
  
Service teams also need to continuously refine alerting and incident systems to avoid alert fatigue. This happens when the on-call team receives more incidents than it can handle, leading to missed or ignored alerts, delayed responses, or burnout.

A service team will want to start with changing in their culture towards being on-call. This starts with taking full ownership of operation which on-call is a part of.  
  
Service teams need to continuously review and improve the monitoring and incident management with intelligent thresholds, tiered alert priorities, thoughtful schedules, consolidated redundant alerts, and consolidated information.

#### References

- [The Site Reliability Workbook: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/) (O'Reilly/Google SRE)

- [Site Reliability Engineering: Practical Alerting from Time-Series Data](https://sre.google/sre-book/practical-alerting/) (O'Reilly/Google SRE)

### Define Alerts for Known Error Conditions

**Incidents resulting from incoming alerts are for conditions that interrupt service reliability.**

A monitoring service can send out many events to the incident management, be it ServiceNow or PagerDuty. Flooding these systems with unimportant messages not only prevents engineers from paying attention to the real issues but also impacts team morale and quickly leads to burnout as engineers may be on edge unnecessarily all the time.  
  
Even when a monitoring software is configured to send out only error alerts to an incident management tool, there is still fine-tuning needed between the two systems to avoid alert fatigue on the receiver. Generating a new incident for every incoming alert of the same symptom can quickly overwhelm on-call personnel with the sheer number of incidents. For example, a CPU-based alert may generate 20 alerts whenever CPU usage exceeds 85%, which could become 20 incidents when they reach PagerDuty, while in fact they all should be merged into just one single incident about CPU exceeding its intended limit.  
  
Another consideration when sending alerts is to properly assign the level of urgency or severity when alerts turn into incidents. For example, while sending certain alerts about WARNING may make sense if these are forewarnings of some imminent error condition, this should be used with care and probably not raise alarms in the middle of the night.

Service team SHOULD add, update, and refine its alerts and incidents definitions as it adds more product features, more software components, and more customers.  
  
Considering the work-life balance effect that comes from dealing with alerts/incidents is the soft aspect of engineering life. If your alerts are properly configured for error conditions and your team is still struggling with keeping up with volume, perhaps it's time to review your software architecture or ask for help.

### Ensure Incidents are Assigned Properly and Write Runbooks

**Incidents resulting from incoming alerts are purposely assigned and have a clear resolution action that can be taken by a responder.**

An incident needs to reach its receivers, the on-call engineers, before they can actually do something about it. Mindlessly generating alerts without a dedicated team of responders defeats the very purpose of alerting in the first place when no one claims responsibility or will even be notified to act.  
  
When creating alerts, service teams need to create runbooks to instruct on-call engineers to quickly resolve incidents formed by these alerts. Runbooks can describe actions to reduce the impact and scope of an incident, instructions for troubleshooting an issue, or next steps for escalation. Without runbooks, even if an engineer acknowledges an incident, they may not know what to do and can potentially cause further chaos by trying to fix it improperly.

Service teams will want to be active in regularly reviewing their alerts and corresponding runbooks. There will be alerts that are no longer needed, which need to be removed from the system. There will be new alerts for new conditions arising from the growth of the application, so new runbooks will need to be created for these situations.

Runbooks should be created for any alert that fires that does not have an associated runbook.  
  
Post-mortem is also a good practice to show how teams react to incidents. A truthful, blamelessness post-mortem can reveal areas in which a service team may need to improve.  
  
Another good practice to test the knowledge of the team as well as the quality of your alerts and runbooks is incident drills. Teams may want to pick a date to raise an arbitrary test alarm and see how the on-call personnel respond. This will not only keep the memory fresh but also help expose any issues with your incident workflow.

### Define Alerts for 5xx and Some 4xx Responses on Endpoints

**Service should have alerts to specifically target 5xx and certain 4xx responses on public-facing endpoints.**

Being available *all-the-time* or following a SLO is not enough. To maintain a service’s reliability, a service team must know when its service is starting to become unreliable. For public-facing endpoints, whether consumers are actual HPE customers or other HPE services, teams must put in place alerts to monitor those endpoints.  
  
For example, teams leveraging K8s may have a couple of exposed nginx endpoints. These should be monitored for 5xx and 4xx errors and when the number of alerts breach a set threshold, an incident should be raised accordingly. The commonly seen 502 or 503 error code of nginx are good candidates for monitoring. One use case is when your K8s pods are still healthy— which doesn't trigger any alert in Prometheus Alertmanager—but if the nginx proxy somehow lost communication with the pods, the client may receive a 502 error instead.

Combining these alerts with an external monitoring system, such as those mentioned above (Pingdom, New Relic, AWS Route 53, etc.), provides the added benefit of gaining crucial insights into your service quality from an independent point of view independent of your infrastructure.

Teams can start with reviewing their exposed endpoints and adding alert rules for those endpoints. Use of external monitoring systems is encouraged to make this practice even more robust.

### Conduct Periodic Major Incident Fire Drills

**Teams should build incident response habits through periodic major incident fire drills.**

The above-mentioned requirements have talked about the importance of building monitoring and incident management systems. The somewhat ironic truth is that major incidents don't happen every day once your application matures enough. Because if they did, it would mean your entire application stack needed some serious work. So, while it's best practice to have incident workflows in place for major outages, we hope we don't have to deal with any. And for the most part, this is true.  
  
This leads to a dilemma as to how we can retain our sharpness and effectiveness when dealing actual major incidents when they do happen. The realistic answer is through practice. Teams may want to exercise by playing a dry-run incident scenario on a frequent basis. This means you will pick up a certain date and time weekly, biweekly, or monthly to have a non-real incident created and assess how everyone in the team plays their part from the moment it shows up until it's completely resolved.  
  
The more time a team spends time on this type of incident training, the better it is prepared when real incidents come. Through practice, service teams will likely discover what they do well or not, what is missing, what needs updates, or what could be further improved. Like the concept of muscle memory, the imprinted actions/reactions stick with the entire team.

Teams should set aside a specific date and time each week or month for incident fire drills. Each fire drill may only need to focus on one particular incident. Industry experience shows that it might be best to make this a fun social event with the purpose to learn.

**Original publication date (yyyy-mm-dd):** 2025-01-27

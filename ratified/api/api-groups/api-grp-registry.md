# Registry of API Groups

The following API groups can be used as a part of a URI as described in the [naming convention](../naming.md)
> To add a new API group, please follow [this process](add-api-grp.md).

## External API Groups

| Group name                 | Description                                                                                              | Owning service                                  |
|----------------------------|----------------------------------------------------------------------------------------------------------|-------------------------------------------------|
| authorization              | RBAC and Oauth2 APIs                                                                                     | Platform IAM                                    |
| backup-recovery            | Backup and recovery management                                                                           | Backup and Recovery (Data Services)             |
| billing                    | Billing mgmt and retrieval of invoices in the platform                                                   | Platform billing                                |
| block-storage              | Block storage management                                                                                 | Block Storage (Data Services)                   |
| compute-ops                | Compute device management incl. monitoring and firmware updates.                                         | Compute Ops Management                          |
| compute-ops-mgmt           | Placeholder for new API group for Compute-Ops                                                            | Compute Ops Management                          |
| data-services              | Core data services incl. issues, audit, and aggregated common resources for all data services            | Data Services (Data Services)                   |
| devices                    | Device management in the platform                                                                        | Platform devices                                |
| disaster-recovery          | Zerto disaster recovery management                                                                       | Disaster Recovery (Data Services)               |
| events                     | Webhook and event management                                                                             | Platform Automation                             |
| file-storage               | File storage management                                                                                  | File Storage (Data Services)                    |
| identity                   | Users, user groups, api clients, and identity provider management                                        | Platform IAM                                    |
| k8s-svc                    | Operate Kubernetes for container orchestration                                                           | HPE Kubernetes Service (Data Services)          |
| notifications-center       | Notifications center                                                                                     | Platform Notifications Center                   |
| object-storage             | Object storage management                                                                                | Object Storage (Data Services)                  |
| organizations              | Organizations, organizational workspaces, and management groups                                          | Platform IAM                                    |
| private-cloud-business     | Private Cloud Business Edition, multicloud VM management                                                 | Private Cloud Business Edition (Data Services)  |
| reporting                  | Data ingestion, transformation, and export functionality                                                 | Platform reporting                              |
| service-catalog            | Getting and provisioning services in the platform                                                        | Platform service-registry and service-provision |
| storage-fabric-mgmt        | Storage fabric management incl. manage, monitor, and validate data center storage fabric configurations  | Storage Fabric Management (Data Services)       |
| storage-fleet              | Storage device management                                                                                | Data Ops Manager (Data Services)                |
| subscriptions              | Subscription management in the platform                                                                  | Platform subscriptions                          |
| support-accounts           | Coveo and Qualtrics authentication tokens service, support hub user preferences management               | Support Identity                                |
| support-assets             | Asset management services                                                                                | Support Entitlement                             |
| support-cases              | Support case management services for case life cycle management                                          | Support Case Management                         |
| support-chat               | Conversational chat to aid users with support content and actions within the platform                    | Support Chat/Virtual Agent                      |
| support-contracts          | Contract management services                                                                             | Support Entitlement                             |
| support-feedback           | Services for feedback and ratings on portal and Support knowledge content items                          | Support Knowledge Management                    |
| support-knowledge          | Services for Support documentation such as manuals, alerts, KB articles, and other support media         | Support Knowledge Management                    |
| support-metadata           | Services for cross-domain Support metadata such as products and controlled vocabularies                  | Support Knowledge Management                    |
| support-software           | Services for Support software including firmware, drivers, and applications                              | Support Knowledge Management                    |
| sustainability-insight-ctr | Sustainability metrics and reporting                                                                     | Sustainability Insight Center                   |
| tags                       | Viewing of resource tags                                                                                 | Platform tags                                   |
| virtualization             | Management of virtual machines and other virtual resources in public clouds and on-premises systems      | Virtualization (Data Services)                  |
| wellness                   | Wellness events and support case automation                                                              | Platform wellness dashboard                     |
| workspaces                 | Legacy / non-organizational workspace management                                                         | Platform IAM                                    |

## Internal API Groups

| Group name                 | Description                                               | Owning service   |
|----------------------------|-----------------------------------------------------------|------------------|
| internal-gts-compliance    | GTS compliance functionality                              | gts-compliance   |
| internal-orders            | Global functionality for license and device orders        | Platform orders  |
| internal-global-search     | Global search service for the HPE GreenLake Platform      | Global Search    |
| internal-semantic-search   | Semantic search component of the Global search Service    | Global Search    |
| internal-reporting         | Reporting Service for HPE GreenLake Platform              | Reporting        |
| internal-alerting          | Alerting Service for HPE GreenLake Platform               | Alerts           |
| identity-directory-service | Authenticate a { service-id, secret } pair as being valid | Service Identity |

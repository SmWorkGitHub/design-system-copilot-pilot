---
seo:
  title: UI Standards - Usability, User Interaction, and Feedback  | HPE GreenLake Platform
toc:
  enable: true
---

# Usability, User Interaction, and Feedback

This section of the UI standard covers essential usability aspects such as system status visibility, feedback responsiveness, error prevention, and user control, emphasizing the importance of adhering to HPE's design principles and best practices. By following these standards, developers can create consistent, user-friendly interfaces that enhance usability and accessibility across all applications.

## System Status Visibility

* MUST ensure that the system (at the product level) effectively communicates comprehensive status of all elements and assets, prioritized by importance. This can typically be accomplished with a central dashboard. Also consider floating, non-modal widgets that effectively summarize global system state, as well as feeds, and tickers for timely system status information.

* SHOULD support easy access to more detailed system status and lower-level entity/asset information with a minimum of required navigation steps (e.g., no more than three clicks).

## System Feedback and Responsiveness

* MUST provide clear and concise feedback for all user actions, including visible success messages, error alerts, and progress indicators where applicable (e.g., a confirmation message should appear after a user submits a form, or an error message should specify what input needs correction).

* MUST consider different user states and provide appropriate visual cues to indicate which state the user is currently in (e.g., edit mode).

* SHOULD respond to user inputs in less than one second, with appropriate feedback – e.g., busy indicator. For system responses that are longer than five seconds, we MUST provide appropriate, task-relevant communication such as a progress indicator or textual description of the expected response timing.

* SHOULD employ “live filtering” of results as users engage with keyword search and/or filtering on all data collections.

## Error Prevention, Recovery, and Contextual Help

* MUST optimize designs to prevent users from entering error states, using "pre-commit"/UI validation where possible and appropriate system feedback in all cases.

* MUST ensure notifications, messaging, and error handling are user-friendly (UI-centric, clear, and actionable) and align with the [HPE Design System](https://design-system.hpe.design/) voice and tone. This includes providing specific next steps (e.g., “Please re-enter your password to continue”) and avoiding technical jargon that may confuse users.
  Example: When a form submission fails due to a server error, the error message should state: “We couldn’t process your request. Please try again or contact support if the issue persists.”

* SHOULD utilize TCaaS (Technical Content as-a-service) for [in-app contextual help](https://github.com/glcp/contextual-help/) content /support inline strings. Using TCaaS allows teams to leverage existing HPESC localization (L10n) processes. For reference, see the [TCaaS API spec with definitions](https://pages.github.hpe.com/TS-RnD/doc-api.html#/) (VPN required).

## Minimize Short-term Memory Load

* MUST reduce cognitive load as much as possible by fusing or co-locating data frequently used together to minimize the need to retain data in memory across navigation steps.

* MUST provide access to necessary contextual instructions, without requiring disruptive navigation/search.

* MUST minimize user input and provide defaults where possible. This not only reduces cognitive/memory load but also reduces user input errors.

* MUST follow the "know me, show me" principle – do not make users enter data we already have.

## Navigation and Workflows

* MUST optimize workflows to reduce complexity and improve user experience, efficiency, and productivity.

* MUST provide clear, comprehensive menu/navigation options in addition to feature/function search. Users should be able to easily view all system function capabilities without time-consuming navigation and browsing.

* MUST optimize UX workflows to reduce the number of steps required for all tasks and especially for common tasks.

* MUST apply the 80/20 rule to prioritize the most impactful tasks and simplify decision-making.

* Screen readers MUST recognize navigation indicators.

* MUST have meaningful page titles for browser tabs to provide accurate and concise descriptions of the page's purpose. (GLP Example: Device Subscriptions \| Devices \| HPE GreenLake).

* SHOULD provide directed/focused (e.g., wizard) workflow options for new users and out-of-the-box use cases.

* SHOULD provide shortcuts, including keyboard commands, to support more experienced users.

## User Control (Internal Locus of Control) and Trust

* MUST allow users to control their experience and workflows.

* MUST ensure that the system conveys accurate and consistent information (i.e., one version of the truth) and that the user can rely on the source of information provided.

* SHOULD, where appropriate, provide customizability options, such as configurable dashboards, adjustable layouts, or personalized shortcuts, to enable users to optimize their workflows and interactions (e.g., allow users to reorder dashboard widgets or save frequently used search queries for quick access).

* SHOULD avoid disabling UI elements; instead, enable controls and accompany them with explanatory messages. Where this is not optimal, provide clear explanations why elements are disabled/unavailable. Maintain consistency within applications.

* SHOULD support users’ flexibly maintaining control of their workflow and not being constrained by their inability to delay, depart, and resume their task. Allow users to asynchronously manage their tasks and be informed of task progress and outcomes (e.g., errors).

## Consistency and Predictability

* SHOULD adhere to the HPE Design System ([Grommet](https://v2.grommet.io/)) components as the reference baseline for user interactions, behaviors, states, and [accessibility](https://design-system.hpe.design/foundation/accessibility) purposes (e.g., use standard button styles for all action buttons and consistent focus indicators for accessibility).

  * If teams decide to use other frameworks, they MUST ensure equivalent UX patterns or components are used. This is important when services use MFEs (micro frontend) to inject in their own contents that will render adjacent to other heterogeneous/homogeneous objects.

* SHOULD use consistent terminology within an application and avoid jargon.

* SHOULD ensure consistent [navigation patterns](https://design-system.hpe.design/templates/navigation?) and user interaction workflows or structures, while supporting GLP global navigation architecture.

* SHOULD adhere to the HPE Design System's fundamental [foundation design](https://design-system.hpe.design/foundation) principles and design [pattern guidelines](https://design-system.hpe.design/components) for a consistent and [inclusive user experience](https://design-system.hpe.design/foundation/human-centered).

## Design for System Lifecycle and Scale Variations

* MUST optimize for all product lifecycle stages (day 0 - 2+): out-of-box installation, system setup and configuration, ongoing monitoring and management, and product end-of-life (e.g., provide user-friendly wizards for initial setup and clear instructions for decommissioning end-of-life products).

* MUST design product UI using appropriate designs, patterns, and components to scale as required to support normal use cases of increased numbers of objects, entities, records, and items.

**Original publication date (yyyy-mm-dd):** 2024-12-18

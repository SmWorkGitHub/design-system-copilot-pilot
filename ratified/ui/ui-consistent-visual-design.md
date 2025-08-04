---
seo:
    title: UI Standards - Consistent Visual Design | HPE GreenLake Platform
toc:
  enable: true
---

# Consistent Visual Design

To ensure a cohesive and consistent visual design across all HPE GreenLake platform products, it is essential to adhere to the HPE design tokens guidelines. This section of the UI standard outlines the standards and best practices for implementing these design tokens, maintaining brand compliance, and ensuring seamless integration with various frameworks and themes.

<!-- The GitHub Markdown linter requires 2 spaces to indicate nesting indentation in unordered lists. Redocly requires 4 spaces. So I am temporarily disabling that aspect of Markdown linting in the next line. -Steve Anderson>
<!-- markdownlint-disable MD007 -->
- HPE design tokens: Your single source of truth for visual design styling.
    - HPE design tokens are the approved source for maintaining visual consistency and [HPE brand compliance](https://design-system.hpe.design/foundation/distinctive-brand-assets?). These tokens define key design elements, such as [color](https://github.com/grommet/grommet-theme-hpe/blob/master/src/js/themes/colors.js) palettes, [typography](https://design-system.hpe.design/foundation/typography?), [icons](https://icons.grommet.io/?theme=hpe), and styling, ensuring all HPE products look and feel cohesive.
        - Using HPE design tokens with Grommet
            - [HPE Pop Theme](https://github.com/grommet/grommet-theme-hpe/blob/master/src/js/themes/hpePop.js)
                - Use this theme for HPE marketing websites, digital experiences, or when creating in-app features with a marketing focus.
                - It provides a [marketing-oriented](https://design-system.hpe.design/foundation/distinctive-brand-assets?#flexible-ingredients) look and feel aligned with HPE brand.
            - [HPE Theme](https://github.com/grommet/grommet-theme-hpe)
                - Use this theme for all aaS HPE GLP products and other applications.
                - It ensures visual consistency for product-oriented designs.
        - Using HPE design tokens with other frameworks
            - If your service or product uses another component framework, apply the HPE Design Tokens to your framework of choice (e.g., Angular, web components, React) using the framework’s preferred style system format to maintain HPE’s branding and styling guidelines. This requires a manual process of mapping design tokens to your local components and templates to ensure proper styling alignment.
        - HPE design tokens are continuously updated and MUST be implemented without requiring significant engineering effort to adopt the latest theme for PATCH and MINOR updates. This process should remain frictionless, enabling products to pivot quicker and stay compliant.
            - For MAJOR updates, which may introduce breaking API changes, teams should expect additional effort while striving to adopt these changes efficiently.
- Products MUST follow best coding practices to allow styling to be inherited and ensure the use of the latest HPE Theme React file (or in specific cases, the appropriate design token service style system format).
    - For detailed developer guidance:
        - Refer to the [Design System Developer Guidance](https://design-system.hpe.design/foundation/developer-guidance) page for theme implementation.
        - Visit [Using design token in code](https://design-system.hpe.design/design-tokens/using-design-tokens-in-code) for instructions on applying design tokens.
        - Explore the [HPE Design System](https://design-system.hpe.design/) site for a broader overview.
- Platform and products SHOULD support theme overrides to enable experiences branded as another company’s product, especially in the context of OEM relationships. For example, an MSP may require branding that aligns with their partners and customers, rather than defaulting to the HPE GreenLake product brand. This functionality should include support for pluggable themes.
- Service and product teams MUST use the [HPE GreenLake web component header](https://design-system.hpe.design/templates/global-header?) for all integrating HPE GreenLake services within GLP, ensuring the appropriate configuration and exposing common functionality. This requirement does not apply to hardware device UIs.
- Service and product teams MUST ensure consistency in the use of salient atoms, button visuals, form elements, and interactive components across all applications.
- Service and product teams MUST apply the latest product application [HPE Theme](https://github.com/grommet/grommet-theme-hpe) for styling. If not using this theme, use the [HPE design token service](https://www.npmjs.com/package/hpe-design-tokens) to ensure seamless alignment with UI toolkit rendering and adherence to HPE application branding guidelines.
- Service and product teams MUST leverage assets from the HPE Design System Figma organization library to create accessible and consistent designs. Include relevant stylesheets, design tokens, atoms, molecules, organisms, and page templates where applicable for your business unit.
<!-- markdownlint-enable MD007 -->

**Original publication date (yyyy-mm-dd):** 2024-12-18

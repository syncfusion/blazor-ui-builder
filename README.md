# Blazor UI Builder

**Blazor UI Builder** is an AI-powered agent skill that transforms your UI requirements into production-ready Blazor components. It leverages Syncfusion's extensive Blazor component library to generate accessible, responsive, and theme-consistent user interfaces.

## Table of Contents

- [Prerequisites](#prerequisites)
- [How it works?](#how-it-works)
- [Quick setup](#quick-setup)
- [Usage](#usage)
- [Best practices](#best-practices)
- [License](#license)
- [Support & Feedback](#support--feedback)
- [See also](#see-also)

## Prerequisites

Before using this skill, ensure you have:

| Requirement | Description |
|-------------|-------------|
| **Blazor Project** | Active Blazor WebAssembly or Blazor Server project (.NET 8+) |
| **Microsoft .NET SDK 8.0 or later** | .NET CLI tools installed |
| **Node.js version 18 or later** | npm package manager installed |
| **Agent Package Manager (APM)** | Agent Package Manager installed. [Installation Guidelines](https://microsoft.github.io/apm/quickstart/#1-install-apm) |
| **Syncfusion License** | [Commercial](https://www.syncfusion.com/sales/unlimitedlicense), [Free Community](https://www.syncfusion.com/products/communitylicense), or [Free Trial](https://www.syncfusion.com/account/manage-trials/start-trials) |

## How it works?

The skill uses an **8-stage AI orchestration** workflow with **2 user decision gates**:

```
User Request
    ↓
[Stage 1: Intent Analysis]
    ↓
[Stage 2: Project Detection]
    ↓
[Stage 3: Layout Analysis & Component Mapping]
    ↓
[Stage 4: Theming & Design System]
    ↓
[Stage 5: Code Generation]
    ↓
[Stage 6: Validation]
    ↓
[Stage 7: Dependencies]
    ↓
[Stage 8: Code Insertion]
    ↓
✓ Complete
```

### Key architecture

| Aspect | Description |
|--------|-------------|
| **Stateless Design** | Conversation history maintains state across stages |
| **Pure AI Reasoning** | Each stage reads guidance documents, analyzes context, makes decisions |
| **2 User Decision Gates** | Stage 4 (design system) + Stage 6 (validation result) |
| **6 Fully Automated Stages** | 1, 2, 3, 5, 7, and final code insertion |
| **Progressive Disclosure** | Stage guides loaded on-demand to minimize context |

## Quick setup

```bash
# Install for GitHub Copilot
apm install syncfusion/blazor-ui-builder -t copilot

# Install for Claude Code
apm install syncfusion/blazor-ui-builder -t claude

# Install for Cursor
apm install syncfusion/blazor-ui-builder -t cursor

# Install for Codex
apm install syncfusion/blazor-ui-builder -t codex
```

## Usage

1. **Select the orchestrator agent** in the AI chat (`syncfusion-blazor-ui-builder`)
2. **Provide your UI request** in natural language/ image.

**Example prompt:**
```
Build a CMS dashboard with KPI cards, a user data table with sorting and filtering, and a sidebar navigation menu
```

The agent will then execute the 8-stage workflow, starting with intent analysis and ending with code insertion into your project.


## Best practices

- Maintain consistency in file structure, naming, and coding standards.
- Use advanced AI models (e.g., Claude Sonnet 4.6+) for better code quality.
- Review everything before production-replace placeholders and verify logic, security, and compatibility.

## License

Syncfusion® Blazor Components is available under the Syncfusion® Essential Studio program, and can be licensed either under the Syncfusion® Community License Program or the Syncfusion commercial license.

To be qualified for the Syncfusion® Community License Program, you must have gross revenue of less than one (1) million U.S. dollars (USD 1,000,000.00) per year and have less than five (5) developers in your organization, and agree to be bound by Syncfusion's terms and conditions.

Customers who do not qualify for the community license can contact sales@syncfusion.com for commercial licensing options.

You may not use this product without first purchasing a Community License or a Commercial License, as well as agreeing to and complying with Syncfusion's license terms and conditions.

The Syncfusion® license that contains the terms and conditions can be found at
[https://www.syncfusion.com/content/downloads/syncfusion_license.pdf](https://www.syncfusion.com/content/downloads/syncfusion_license.pdf)

## Support & Feedback

Product support is available through the following media.

- [Support ticket](https://support.syncfusion.com/support/tickets/create) - Guaranteed response in 24 hours | Unlimited tickets | Holiday support
- [Community forum](https://www.syncfusion.com/forums/blazor-components)
- [Request feature or report bug](https://www.syncfusion.com/feedback/blazor-components)
- [Live chat](https://www.syncfusion.com/support)

## See also

* [Blazor Documentation](https://blazor.syncfusion.com/documentation/introduction)
* [Blazor Components](https://www.syncfusion.com/blazor-components)
* [Blazor Live Demos - Server](https://blazor.syncfusion.com/demos/)
* [Blazor Live Demos - WebAssembly](https://blazor.syncfusion.com/wasm/demos/)
* [Blazor Playground](https://blazorplayground.syncfusion.com/)
* [Blazor Smart/AI Samples](https://github.com/syncfusion/smart-ai-samples)

<p>Copyright © 2001-2026 Syncfusion®, Inc. All rights reserved.</p> 

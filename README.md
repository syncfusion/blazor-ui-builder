# Blazor UI Builder

**Blazor UI Builder** is an AI-powered agent skill that transforms your UI requirements into production-ready Blazor components. It leverages Syncfusion's extensive Blazor component library to generate accessible, responsive, and theme-consistent user interfaces.

## Table of Contents

- [Prerequisites](#prerequisites)
- [How It Works](#how-it-works)
- [The 8 Stages](#the-8-stages)
- [Usage](#usage)
- [Output Artifacts](#output-artifacts)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
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

### Quick Setup

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

## How It Works

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

### Key Architecture

| Aspect | Description |
|--------|-------------|
| **Stateless Design** | Conversation history maintains state across stages |
| **Pure AI Reasoning** | Each stage reads guidance documents, analyzes context, makes decisions |
| **2 User Decision Gates** | Stage 4 (design system) + Stage 6 (validation result) |
| **6 Fully Automated Stages** | 1, 2, 3, 5, 7, and final code insertion |
| **Progressive Disclosure** | Stage guides loaded on-demand to minimize context |

## The 8 Stages

### Stage 1: Intent Analysis

**Purpose:** Parse and validate the user's natural language request.

**What it does:**
- Identifies primary intent: `generate_component`, `generate_page`, or `modify_existing`
- Extracts component type (e.g., "login form" → form/login)
- Extracts modifiers (e.g., "dark theme" → styling:dark)
- Resolves ambiguities with a single clarifying question if needed

**Output:** Component type + modifiers confirmed

**User Interaction:** None (AI analyzes automatically)

### Stage 2: Project Detection

**Purpose:** Auto-detect project structure to ensure generated code integrates seamlessly.

**What it does:**
- Detects Blazor hosting model (WebAssembly or Server)
- Detects .NET version and target framework
- Detects CSS strategy (CSS Modules, Tailwind, Bootstrap, custom)
- Detects component directory structure
- Detects Syncfusion Blazor package versions from `.csproj`
- Detects code formatting rules and conventions

**Output:** Detected settings summary for user confirmation

**User Interaction:** None (AI analyzes automatically)

### Stage 3: Layout Analysis & Component Mapping

**Purpose:** Analyze requirements and map to specific Syncfusion Blazor components with validation.

**Component Validation & Fallback Flow:**

The agent will validate all requested components against the Syncfusion Blazor library:

- **Component Found**: Proceeds with mapping
- **Component NOT Found**: Agent asks:
  - "I don't have [ComponentName] in Syncfusion Blazor library. Would you like me to:
    - **A) Check official Syncfusion documentation** online to verify availability?
    - **B) Suggest an alternative component** that provides similar functionality?
    - **C) Provide additional details** about what this component needs to do?"

**What it does:**
- Validates all requested components exist in Syncfusion Blazor library
- Analyzes user requirements for layout structure
- Creates `component-mapping.json` with element definitions
- Runs ComponentMapper script (BM25 algorithm) to map elements to Syncfusion components
- Maps icon hints to EJ2 icons using IconMapper

**Output:**
- `component-mapping.json` (saved to project root)
- Component + Icon mapping results for Stage 5
- **Validation Status**: All components verified in Syncfusion Blazor library

**Requirements:**
- Minimum 3 Syncfusion components mapped (with Sf prefix: SfGrid, SfTextBox, SfButton, etc.)
- Script execution: `py components_search.py <project-root>/component-mapping.json`
- All components must pass Syncfusion library validation before proceeding

**User Interaction:** Clarification if unknown components are requested; otherwise fully automated

### Stage 4: Theming & Design System

**Purpose:** Lock design system decisions before code generation.

**Default Theme: Fluent 2 Light**

**What it does:**
- **Default Selection**: Fluent 2 Light (unless explicitly overridden)
- Confirms CSS framework (Tailwind, Bootstrap, Material, or Greenfield)
- Selects Syncfusion theme alignment (**Fluent 2 Light [DEFAULT]**, Fluent 2 Dark, Bootstrap5, Material3, Tailwind3)
- Defines color system (OKLCH recommended, with Fluent 2 Light color palette as default)
- Establishes spacing scale (4pt or 8pt grid)
- Sets typography hierarchy (modular scale ratio)
- Plans responsive breakpoints (mobile-first)
- Documents token architecture
- Verifies Syncfusion theme CSS imports for correct location (wwwroot/index.html for WASM, Components/App.razor for WebApp)

**Output:** Design system decisions locked (including confirmed default Fluent 2 Light theme) and ready for Stage 5

**Framework-Specific References:**
- [Tailwind Implementation](.apm/skills/syncfusion-blazor-ui-builder/references/tailwind-implementation.md)
- [Bootstrap Implementation](.apm/skills/syncfusion-blazor-ui-builder/references/bootstrap-implementation.md)
- [Material Implementation](.apm/skills/syncfusion-blazor-ui-builder/references/material-implementation.md)
- [Greenfield Implementation](.apm/skills/syncfusion-blazor-ui-builder/references/greenfield-implementation.md)

**User Interaction:** - **Decision Gate #1** -> Confirm design system decisions

### Stage 5: Code Generation

**Purpose:** Generate production-ready Blazor components with all design tokens applied.

**What it does:**
- Reads ALL component skill documentation first (getting-started.md for each)
- Reads syncusion-themes.md for proper theme imports
- Generates `.razor` component files with Syncfusion components
- Generates CSS stylesheets following the design system
- Generates C# code-behind classes with component logic
- Applies WCAG 2.1 AA accessibility
- Applies responsive design (mobile-first)
- Includes sample parameter definitions and lifecycle hooks

**Output:** Generated files ready for review

**Requirements:**
- Component skill documentation must be read BEFORE generating code
- All Syncfusion imports must match exact syntax from component skills
- Single overall Syncfusion style import must be included in the main layout.

**User Interaction:** None (AI generates automatically)

### Stage 6: Validation

**Purpose:** Validate generated code against web standards.

**What it does:**
- Validates WCAG 2.1 AA compliance (semantic HTML, ARIA, keyboard nav, contrast)
- Checks security (no XSS, no hardcoded secrets, sanitized inputs)
- Verifies performance (proper component lifecycle, optimized event bindings)
- Checks responsive design (mobile-first, flex/grid, media queries, 44x44px touch targets)

**Output:** Binary result — **PASS ✓** or **FAIL ✗**

**Auto-fixes:** Applies fixes where possible automatically

**User Interaction:** - **USER DECISION #2** -> Confirm or override validation result

### Stage 7: Dependencies

**Purpose:** Detect required packages and resolve version conflicts.

**What it does:**
- Scans generated code for Syncfusion imports
- Lists all required `Syncfusion.Blazor.*` packages
- Checks existing `.csproj` for conflicts
- Detects Syncfusion version from `.csproj` (uses detected version for consistency)
- Prepares dotnet add package command

**Output:** Installation command ready

**User Interaction:** Confirm dotnet add or run manually

### Stage 8: Code Insertion

**Purpose:** Insert generated files into the project and verify build.

**What it does:**
- Creates component directory structure
- Inserts generated files (.razor, .razor.css, code-behind .cs)
- Updates imports in App.razor if needed
- Verifies build succeeds without errors

**Output:** File paths + success status

**User Interaction:** None (AI executes automatically)


## Usage

1. **Select the orchestrator agent** in the AI chat (`blazor-ui-build-orchestrator`)
2. **Ensure skills are added**:
   - `syncfusion-blazor-ui-builder` skill
   - `syncfusion/blazor-ui-components-skills` - For installation refer [here](https://github.com/syncfusion/blazor-ui-components-skills/blob/master/README.md).
3. **Provide your UI request** in natural language/ image.

**Example prompt:**
```
Build a CMS dashboard with KPI cards, a user data table with sorting and filtering, and a sidebar navigation menu
```

The agent will then execute the 8-stage workflow, starting with intent analysis and ending with code insertion into your project.

## Output Artifacts

### Generated Component Structure

```
Components/[ComponentName]/
├── [ComponentName].razor           # Blazor component with Syncfusion
├── [ComponentName].razor.cs        # Code-behind class
├── [ComponentName].razor.css       # Component-scoped styles
└── [ComponentName].razor.js        # Interop if needed
```

### Supporting Files

| File | Purpose |
|------|---------|
| `component-mapping.json` | Layout structure for component mapping |
| `wwwroot/styles.css` | Custom CSS design system (Greenfield only) |
| `appsettings.json` | Syncfusion license key configuration (if provided) |

---

## Best Practices

- Maintain consistency in file structure, naming, and coding standards.
- Use advanced AI models (e.g., Claude Sonnet 4.6+) for better code quality.
- Review everything before production-replace placeholders and verify logic, security, and compatibility.

## Troubleshooting

### General Issues

| Issue | Solution |
|-------|----------|
| "Project type not detected" | Ensure `.csproj` exists with Blazor SDK reference |
| "Syncfusion license banner appears" | Add license key via Stage 2 prompt or appsettings.json |
| "Build fails after insertion" | Check `references/build.md` for conflict resolution |
| "Component not rendering" | Verify using `@if`, `@foreach` directives, and parent correctly imports |
| "Component has styling issues" | Check component skill getting-started.md for correct style imports |

### Component Availability Issues

| Issue | Solution |
|-------|----------|
| "Component [Name] not in Syncfusion" | Agent will suggest alternatives or ask for clarification |
| "I want to use custom component" | Syncfusion Blazor library is required. Suggest closest Syncfusion equivalent |
| "How do I check online?" | Agent provides link to Syncfusion Blazor documentation |
| "Can I use another UI library?" | No - this skill is specifically for Syncfusion Blazor components only |

### Theme Issues

| Issue | Solution |
|-------|----------|
| "How to change theme from Fluent 2?" | Specify desired theme in Stage 4; options: Fluent 2 Dark, Bootstrap5, Material3, Tailwind3 |
| "Theme CSS not loading" | Verify Stage 4 theme CSS import location (wwwroot/index.html for WASM, Components/App.razor for WebApp) |
| "Fluent 2 license required?" | No - Fluent 2 is included with Syncfusion Blazor package |

### Common Stage Errors

| Stage | Error | Recovery |
|-------|-------|----------|
| Stage 3 | "Component not in Syncfusion" | Agent asks for clarification or suggests alternatives |
| Stage 4 | "Framework not detected" | Override detected settings in Stage 2 |
| Stage 5 | "Missing Syncfusion imports" | Read component skill documentation first |
| Stage 6 | "Validation fails" | Auto-fixes applied; user can override |

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

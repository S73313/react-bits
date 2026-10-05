# MSTRMND Product and Release Strategy

Status: discovery draft  
Scope: assets verifiably present in this repository as of October 5, 2026

## 1. Purpose

This document inventories what exists today, distinguishes products from delivery channels, and proposes a release strategy that can expand across platforms without creating a separate product for every wrapper.

The central recommendation is:

> Define one MSTRMND product and one portable experience contract first. Treat web, desktop, mobile, extensions, design tools, and AI clients as adapters around that product.

This avoids prematurely maintaining many branded shells with no platform-specific value.

## 2. What exists today

### Product surface

The repository currently contains the React Bits project, not a MSTRMND-branded product.

| Capability                               | Current state                  | Evidence                                                                                                           |
| ---------------------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| Interactive component catalog            | Working                        | 116 components across text animations, animations, components, and backgrounds                                     |
| Component variants                       | Working                        | JavaScript/TypeScript × CSS/Tailwind; 462 generated registry items                                                 |
| Documentation website                    | Working                        | Vite + React application with landing, catalog, detail, favorites, installation, MCP guidance, and showcase routes |
| Live previews and configurable examples  | Working                        | Demo and source trees for each component family                                                                    |
| Manual source installation               | Working                        | Copyable component code and dependency instructions                                                                |
| Registry installation                    | Working                        | shadcn-compatible JSON output and jsrepo build configuration                                                       |
| AI-assisted discovery                    | Integration only               | Documentation configures the third-party shadcn MCP server for Claude Code, Cursor, and VS Code                    |
| Favorites                                | Local only                     | Browser `localStorage`; no user account or cloud synchronization                                                   |
| Preference persistence                   | Local only                     | Language, style, package manager, and install mode are stored in the browser                                       |
| Community showcase                       | Working but externally coupled | Static entries and remotely hosted images                                                                          |
| Backend, accounts, billing, or analytics | Not present                    | No application backend is defined in this repository                                                               |
| PWA installation and offline mode        | Not present                    | A minimal web manifest exists, but no service worker or offline strategy exists                                    |
| Native mobile application                | Not present                    | No iOS, Android, Capacitor, React Native, or native project                                                        |
| Desktop application                      | Not present                    | No Tauri or Electron project                                                                                       |
| Browser extension                        | Not present                    | No extension manifest or extension runtime                                                                         |
| IDE extension                            | Not present                    | No VS Code, JetBrains, or editor extension package                                                                 |
| Figma or other design-tool plugin        | Not present                    | No design-tool plugin manifest or runtime                                                                          |
| First-party MCP server                   | Not present                    | Current MCP documentation points to the shadcn MCP server                                                          |
| Automated release pipeline               | Not present                    | No repository release workflow is defined                                                                          |

### Current architecture

```text
Component metadata
  ├─ Demo website and documentation
  ├─ Four source variants per component (where supported)
  ├─ shadcn-compatible JSON registry
  └─ jsrepo registry output

Browser application
  ├─ React + Vite
  ├─ Chakra UI + Tailwind
  ├─ Browser-local favorites and preferences
  └─ GitHub API and external showcase assets
```

The current implementation is a web-first component distribution and discovery system. It is not yet a multi-platform application architecture.

## 3. Rights and branding boundary

The repository is licensed under MIT plus the Commons Clause and is copyrighted by David Haz. It permits use in an application, website, or product, but prohibits selling, sublicensing, or redistributing the components themselves, including in a bundle or port.

Before a MSTRMND release:

1. Confirm that MSTRMND has the right to publish the intended derivative.
2. Preserve the required copyright and license notice.
3. Do not present the existing component library, registry, or ports as original MSTRMND assets.
4. Obtain separate permission if the plan is to rebrand or redistribute the library itself.
5. Inventory third-party fonts, images, videos, logos, examples, dependencies, and community submissions.

The safer product boundary is an original MSTRMND application that uses permitted components, not a renamed distribution of React Bits.

## 4. Proposed product definition

### Working product statement

MSTRMND is a creative experience workspace for discovering, composing, adapting, and exporting interactive interface experiences across development and design workflows.

### Primary user jobs

1. Discover an interaction or visual treatment.
2. Preview and configure it against real content.
3. Save it to a personal or team collection.
4. Export or install an implementation for the target stack.
5. Reuse the same intent in code, design, and AI-assisted workflows.
6. Track provenance, compatibility, license, and version.

### Product layers

| Layer               | Responsibility                                                | Branding                                |
| ------------------- | ------------------------------------------------------------- | --------------------------------------- |
| MSTRMND Workspace   | Search, preview, compose, save, collaborate, and export       | MSTRMND                                 |
| Experience Catalog  | Metadata, assets, compatibility, provenance, and versioning   | MSTRMND catalog with source attribution |
| Experience Runtime  | Portable schema and rendering contracts                       | MSTRMND                                 |
| Adapters            | Web, desktop, mobile, browser, IDE, design tool, CLI, and MCP | MSTRMND                                 |
| Third-party content | Licensed components and source implementations                | Original attribution retained           |

## 5. The lower-level abstraction

A lower-level abstraction is justified only if at least three delivery channels need the same authored experience and lifecycle.

### Proposed `MSTRMND Experience Manifest`

The manifest should describe intent and capabilities rather than framework source code:

```ts
type ExperienceManifest = {
  id: string;
  version: string;
  title: string;
  description: string;
  provenance: {
    author: string;
    sourceUrl?: string;
    license: string;
  };
  capabilities: Array<'pointer' | 'touch' | 'keyboard' | 'motion' | 'audio' | 'webgl' | 'network' | 'filesystem'>;
  parameters: Record<string, ParameterDefinition>;
  targets: Partial<Record<Target, TargetImplementation>>;
  assets: AssetReference[];
  accessibility: AccessibilityContract;
  performance: PerformanceBudget;
};
```

Potential targets are `web-react`, `web-component`, `ios`, `android`, `figma`, `video`, and `static`. A target must point to a real implementation; the manifest must not imply that arbitrary React effects can be converted automatically into native or design-tool equivalents.

### What belongs in the shared core

- Stable IDs, versions, and migration rules
- Search and taxonomy
- Parameter schemas and defaults
- Capability declarations
- Asset references and integrity metadata
- License and provenance metadata
- Accessibility requirements
- Performance budgets
- Adapter compatibility
- Export recipes

### What remains platform-specific

- Rendering and animation implementations
- Input and lifecycle handling
- Permissions and sandbox behavior
- Store packaging and signing
- Platform navigation and accessibility APIs
- Performance tuning

### Extraction rule

Do not begin by rewriting the existing catalog. Prove the manifest with 3–5 representative experiences:

- A text animation
- A DOM/CSS interaction
- A canvas or WebGL background
- A navigation component
- A reduced-motion/static fallback

If those examples cannot share a useful contract without leaking React internals, keep the product web-first and use a thinner catalog schema.

## 6. Platform strategy

“Available everywhere” should mean continuity of the product job, not identical functionality on every platform.

| Platform                   | Product role                                                    | Reuse potential           | Recommendation                                              |
| -------------------------- | --------------------------------------------------------------- | ------------------------- | ----------------------------------------------------------- |
| Responsive web             | Full workspace and canonical catalog                            | Highest                   | First release                                               |
| Installable PWA            | Fast access, saved collections, limited offline previews        | High                      | Add after web identity and caching are stable               |
| CLI / registry adapter     | Install and export implementations                              | High                      | Keep as a first-class developer channel                     |
| MCP server                 | Search, inspect, configure, and install through AI clients      | High                      | Build after catalog API and permissions are stable          |
| VS Code / Cursor extension | In-editor preview and insertion                                 | Medium-high               | Validate demand after CLI/MCP                               |
| Browser extension          | Capture inspiration or inspect compatible experiences on a page | Medium                    | Ship only with a clear browser-native workflow              |
| macOS / Windows / Linux    | Focused workspace, local files, offline assets                  | High with Tauri/web shell | Package after the web product proves desktop-specific value |
| iOS / Android              | Browse, save, review, and lightweight parameter editing         | Medium                    | Avoid a simple web wrapper; define touch-native jobs first  |
| Figma plugin               | Search, place static/motion references, sync tokens and specs   | Medium-low                | Requires purpose-built renderer/exporter                    |
| Adobe/Canva/other plugins  | Tool-specific creation workflows                                | Low initially             | Scope individually after Figma validation                   |
| Native SDKs                | Embed production-native experiences                             | Low                       | Separate product line; do not promise in initial launch     |

## 7. Release sequence

### Phase 0 — Product and rights gate

- Approve the product statement, audience, and MSTRMND naming architecture.
- Decide whether React Bits is an internal dependency, attributed catalog source, or separately licensed content partner.
- Complete a content and dependency rights inventory.
- Reserve domain, package scope, application IDs, extension IDs, and store names.
- Select the initial 3–5 experiences for the manifest proof.

Exit criterion: the team can state what is being sold or distributed without describing it as a rebranded component library.

### Phase 1 — MSTRMND web alpha

- Establish MSTRMND visual identity and information architecture.
- Separate catalog metadata from the current React UI.
- Add provenance and license presentation to every catalog item.
- Add stable catalog and experience versioning.
- Replace browser-only state with an account-ready repository boundary; a backend is optional until synchronization is required.
- Add automated lint, build, accessibility, and registry validation.
- Make deployment reproducible and add preview environments.

Exit criterion: the web workspace has an original MSTRMND value proposition and can be released without relying on implicit ownership claims.

### Phase 2 — Portable developer channels

- Publish the supported registry adapter under an approved identity.
- Create a first-party CLI around catalog search, inspection, and installation.
- Create a read-first MCP server; require confirmation for file mutations or installs.
- Define compatibility and deprecation policies.
- Sign releases, publish checksums, and generate software bills of materials.

Exit criterion: CLI, MCP, and web resolve the same catalog IDs and versions with consistent provenance.

### Phase 3 — PWA and desktop

- Add a complete manifest, icons, install UX, cache policy, and offline behavior.
- Define update, rollback, crash reporting, and data migration behavior.
- Package desktop only after identifying local workflows such as file access, local preview servers, export, or offline asset management.
- Test reduced motion, GPU fallback, battery use, and low-memory behavior.

Exit criterion: packaged clients provide measurable value beyond opening the website.

### Phase 4 — Workflow plugins

- Start with one editor integration and one design-tool integration.
- Keep plugins thin; use catalog APIs and the shared manifest.
- Minimize requested permissions and document all data movement.
- Build store-specific review, screenshots, privacy disclosures, and support processes.

Exit criterion: each plugin completes a platform-native job and can be supported independently.

### Phase 5 — Mobile and native evaluation

- Validate mobile-specific jobs with prototypes.
- Decide between responsive PWA, shared web shell, or native UI per job.
- Treat native renderers/SDKs as independent implementations with their own compatibility tests.

Exit criterion: mobile retention or workflow evidence supports the additional implementation surface.

## 8. Release architecture

The preferred repository shape, if multiple channels are approved:

```text
apps/
  web/
  desktop/
  mobile/                 # only after mobile scope is approved
plugins/
  vscode/
  browser/
  figma/
packages/
  catalog-schema/
  experience-manifest/
  catalog-client/
  web-renderer/
  ui/
  telemetry-contract/
tools/
  cli/
  mcp-server/
content/
  first-party/
  licensed/
```

This is a target architecture, not a recommendation to restructure immediately. Extract packages when a second consumer exists.

### Versioning

- Product applications: independent semantic versions per channel.
- Manifest schema: semantic version plus explicit migrations.
- Catalog items: immutable published versions.
- Renderers/adapters: declare supported manifest version ranges.
- Content corrections: new versions when output changes; metadata-only revisions may remain non-breaking.

### Release rings

1. Internal
2. Design partners
3. Public preview
4. Stable

Every channel should support staged rollout, rollback, telemetry opt-out, and a documented end-of-support policy before stable release.

## 9. Product and operational requirements

### Required for any public release

- MSTRMND brand system and naming rules
- Privacy policy, terms, support contact, and security contact
- License notices and per-item provenance
- Accessibility target and reduced-motion behavior
- Content security and dependency scanning
- Automated build and release checks
- Error reporting with privacy controls
- Defined data retention and deletion behavior
- Store metadata, screenshots, and review ownership
- Incident, rollback, and deprecation procedures

### Quality gates

- Build and lint pass from a clean checkout
- Registry output is reproducible
- No broken routes, assets, or install commands
- Keyboard and screen-reader review
- Reduced-motion and GPU fallback review
- Performance budgets per experience
- Offline/update tests for packaged clients
- Permission review for every plugin
- License/provenance report generated for every release

## 10. Decisions still required

| Decision                                                              | Why it matters                                                                         |
| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| What is the first paid or strategic user job?                         | Determines whether the product is a workspace, catalog, toolchain, or content business |
| Is MSTRMND the company, platform, or product name?                    | Controls package names and store identity                                              |
| What original MSTRMND content exists outside this repository?         | This inventory currently covers only the checked-out repository                        |
| What rights exist for the React Bits source and brand?                | Blocks rebranding or redistribution decisions                                          |
| Who is the first audience: developers, designers, creators, or teams? | Changes the initial channel and feature set                                            |
| Is cloud sync required for the first release?                         | Determines backend, authentication, and privacy scope                                  |
| Which platform has a unique workflow beyond the web app?              | Prevents low-value wrappers                                                            |
| What is free, paid, or enterprise?                                    | Affects licensing, billing, entitlements, and store policy                             |
| Which geographies and age groups are supported?                       | Affects privacy, content, and store compliance                                         |

## 11. Recommended immediate scope

1. Use this document as the inventory baseline.
2. Add every other prototype, repository, design file, automation, plugin, and deployed URL to an appendix.
3. Run a rights review before any MSTRMND rebrand.
4. Write a one-page product brief selecting one audience and one primary job.
5. Prototype the Experience Manifest against five representative experiences.
6. Release an original MSTRMND web alpha before committing to store packaging.
7. Select the next channel from observed workflows, not platform coverage alone.

## Appendix A — Inventory intake template

Use one row for every artifact not represented in this repository.

| Field                                | Value                                              |
| ------------------------------------ | -------------------------------------------------- |
| Product or prototype name            |                                                    |
| Repository / design / deployment URL |                                                    |
| Owner                                |                                                    |
| Intended user                        |                                                    |
| User job                             |                                                    |
| Current status                       | concept / prototype / alpha / production / retired |
| Platforms                            |                                                    |
| Technology                           |                                                    |
| Data and external services           |                                                    |
| Authentication                       |                                                    |
| Distribution channel                 |                                                    |
| License and third-party content      |                                                    |
| Brand currently shown                |                                                    |
| Known users or usage                 |                                                    |
| Maintenance owner                    |                                                    |
| Recommended disposition              | merge / package / rewrite / archive / investigate  |

## Appendix B — Working release scorecard

Score each candidate from 0–3 before adding a platform:

| Criterion             | 0            | 1                 | 2                   | 3                              |
| --------------------- | ------------ | ----------------- | ------------------- | ------------------------------ |
| User evidence         | None         | Anecdotal         | Repeated requests   | Demonstrated usage             |
| Platform-native value | Wrapper only | Minor convenience | Meaningful workflow | Requires platform capability   |
| Shared-core reuse     | None         | Low               | Moderate            | High                           |
| Rights readiness      | Blocked      | Unclear           | Reviewable          | Cleared                        |
| Operational readiness | None         | Manual            | Partially automated | Release and rollback automated |
| Support capacity      | None         | Unassigned        | Shared owner        | Dedicated owner and policy     |

A platform should not enter implementation with a rights-readiness score below 3 or a platform-native-value score below 2.

# Radlab V1 Spec

## Summary

Radlab is a DNA-styled fork of Collaborator.

V1 keeps Collaborator's core capabilities and interaction model, but adds one new first-class canvas primitive: a `component` node that renders DNA registry-backed UI components directly inside the app without relying on a separate playground dev server.

The primary workflow goal is fast context assembly:

- place components, files, terminals, and browser nodes on a shared infinite canvas
- select and arrange them spatially
- connect nodes with explicit React Flow-style edges
- submit work through linked terminal/browser nodes with selected component context delivered automatically

V1 is DNA-first and may bind directly to the local DNA repo path.

## Product Statement

Radlab is an infinite-canvas agent workspace for DNA development.

It should feel like Collaborator operationally, but visually and functionally oriented around DNA component work:

- same broad shell model and capabilities as Collaborator
- DNA styling tokens, fonts, and selective RDNA component reuse
- native component nodes for registry-backed UI work
- linked terminal and browser workflows for fast prompt/context handoff

## Foundations

Radlab is built on top of Collaborator's architecture and source patterns.

This is not a port of the current `tools/playground` Next.js app.

The Collaborator stack is the foundation:

- Electron desktop shell
- React 19
- Tailwind CSS 4
- electron-vite
- xterm.js + tmux-backed persistent terminals
- Monaco editor
- BlockNote / TipTap
- existing infinite canvas interaction model

## Visual Direction

Radlab uses DNA styling, not stock Collaborator styling.

### Required

- DNA tokens
- `pixelcode` and `mondwest`
- selective RDNA component reuse where useful

### Excluded

- `joystix`

### Principle

The UI should still read as Collaborator structurally. V1 is a DNA-styled Collaborator fork, not a visual redesign from first principles.

## Workspace Scope

V1 is DNA-first.

Assume the DNA repo lives at:

`/Users/rivermassey/Desktop/dev/DNA`

V1 may bind directly to that path instead of solving generic workspace configuration up front.

Generic non-DNA workspace support is not a V1 requirement.

## V1 Goals

### 1. Preserve Collaborator capabilities

V1 should preserve the core Collaborator workflow:

- infinite canvas
- pan/zoom
- drag
- resize
- multiselect
- z-ordering
- terminal nodes
- code/note/image/file workflows
- built-in browser nodes
- existing canvas ergonomics wherever practical

### 2. Add native `component` nodes

V1 adds a new node type:

- `component`

`component` nodes must:

- consume the DNA registry successfully
- render isolated DNA components directly in-app
- support canonical components plus variants, iterations, modes, and state surfaces currently represented across RadOS and playground
- expose a useful props/config surface for component preview on canvas

### 3. Make node connections operational

Connections are not decorative.

When a `component` node or selection is connected to a terminal or browser node, that connection should be meaningful at submission time.

The goal is direct context delivery, not manual copy/paste.

### 4. Optimize for context assembly

The main value of V1 is rapid context + location assembly:

- arrange the relevant components
- arrange the relevant files
- link them to the relevant terminal
- submit work
- have the target node receive the needed context automatically

## Terminal Linking Behavior

V1 should support automatic terminal context delivery.

The user should not need to manually paste prompts.

Accepted delivery strategies:

- inject generated prompt/context directly into the linked terminal input
- write structured context to a temp or workspace-local file and send a command referencing it
- optionally do both, with the file as the durable source of truth

Current preferred strategy:

- use both

Design intent:

- React Flow-style connections define which nodes are upstream context providers
- terminal submission resolves current upstream selection and packages it automatically
- the linked terminal is able to understand what it should talk about quickly

## Browser Linking Behavior

V1 keeps Collaborator's built-in browser nodes.

Browser nodes participate in the same context graph:

- selected component/file context may be routed to linked browser nodes
- browser nodes are part of the quick context + location assembly model

V1 does not require inventing a new browser system; it should build on the existing browser-node capability in Collaborator.

## DNA Registry Integration

V1 should support both of these modes conceptually:

- direct runtime reads/imports from DNA
- generated manifest/registry ingestion

For V1, direct integration is preferred.

### Preferred V1 behavior

- read directly from the live DNA repo
- import real components where feasible
- use generated metadata only where the runtime needs an adapter layer

## Explicit Non-Goals for V1

The following are not required for V1:

- fully reproducing all existing playground automation
- preserving the current playground CLI exactly
- solving adopt/writeback flows
- shipping full generate/adopt orchestration
- supporting arbitrary external repos as first-class targets

V1 is about:

- forking Collaborator
- getting it working with DNA components
- adding the native component node
- making the linked canvas workflow real
- understanding what in Collaborator is useful versus bloat

## Relationship to Existing DNA Playground

The current DNA playground is a reference for domain behavior, not the host architecture.

Useful concepts to carry forward:

- DNA registry consumption
- isolated component preview
- variants / iterations / modes / state surfaces
- component-centric review workflows
- quick context handoff to agents

Host-specific pieces of the current playground do not define V1:

- standalone Next app shell
- separate dev server requirement
- localhost API dependency as the primary integration model

## Technical Direction

### Preferred shape

Radlab should begin as a fork-based codebase using Collaborator source patterns directly.

Near-term implementation should prioritize:

1. getting the fork running locally under the new product name
2. documenting the V1 scope in-repo
3. locating the minimum shell changes required for a `component` node type
4. binding the app directly to the DNA repo for registry/component loading
5. implementing connected terminal/browser context delivery

### Product naming

- New name: `radlab`

## Open Questions for Later

These are intentionally deferred, not unresolved blockers for V1 startup:

- should DNA remain hard-bound or become configurable after V1
- whether `component` nodes should become a general plugin/tile API later
- how much of Collaborator is true product value versus removable bloat
- whether generate/adopt flows belong in the app, in a local service layer, or in a separate automation package
- how much of the current playground UI should eventually be reintroduced as native Radlab functionality

## Immediate Next Steps

1. Convert the upstream clone into the local `radlab` working copy conventions
2. Verify the fork builds and runs locally
3. Audit the current Collaborator canvas/tile architecture
4. Identify where tile types are declared and rendered
5. Design the `component` node data model
6. Design the terminal/browser context handoff protocol
7. Bind the app directly to the DNA registry and render one real component


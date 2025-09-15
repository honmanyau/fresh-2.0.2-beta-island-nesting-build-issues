# Island-nesting-related build issues in Fresh 2.0.2 beta

## Description

The PRs in this repository demonstrates server simple cases of island nesting that cause unexpected build behaviours with Vite:

- [#1](https://github.com/honmanyau/fresh-2.0.2-beta-island-nesting-build-issues/pull/1) shows how builds can fail when there are nested islands in `/islands` and no colocated islands inside `/routes`.

- [#2](https://github.com/honmanyau/fresh-2.0.2-beta-island-nesting-build-issues/pull/2) shows that builds no longer fail if we simply add an **unused** island to `/routes/(_islands)` (while keeping all changes in #1).

- [#3](https://github.com/honmanyau/fresh-2.0.2-beta-island-nesting-build-issues/pull/3) shows a similar mode of failure when we move all islands from `/islands` to `/routes/(_islands)`.


## Important note

The issues demonstrated in the PRs in this repository are similar, but different, to what I observed in an existing code base when I was trying to migrate from `2.0.0-alpha.41` (Vite wasn't a thing in Fresh then) to `2.0.2`.

In that project what I observed was a shared **non-island** component causing a the same kind of build error (`Rollup failed to resolve import ".../_PageInfo-DPCxoH3G.js" from "fresh:server-snapshot".`) in the following structure:

- components
    - Info.tsx (import: ./SharedComponent.tsx)
    - SharedComponent.tsx
- islands
    - Dialog.tsx (import: ../components/SharedComponent.tsx)

Where `<Dialog>` is used both nested and un-nested inside both `/islands` and `/routes/**/(_islands)`; and `Info` is only used in components in `/routes/**/(_components)` directories.

The only way to work around the build issues for now is to **not** share `SharedComponent.tsx` (simply by duplicating the component).

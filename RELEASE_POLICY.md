# Release Policy

## Scope

Applies to all libraries under the WebFiori organization:

- collections, cache, log, queue, event, container, err
- jsonx, ui, file, http, database
- cli, mail
- framework
- app, vuetify-core, components

Each library is versioned and released independently.

## Versioning

All libraries follow SemVer (MAJOR.MINOR.PATCH):

- **Patch (x.y.Z):** Bug fixes, CI/tooling updates, documentation corrections. No new API surface.
- **Minor (x.Y.0):** New features, new classes/methods, backward-compatible behavioral changes. May include deprecation notices.
- **Major (X.0.0):** Removal of deprecated APIs, breaking signature changes, dropped PHP version support, architectural changes.

## Release Cadence

| Type | Trigger | Schedule |
|------|---------|----------|
| Patch | Individual fix lands and tests pass | As needed (no batching) |
| Minor | Feature is complete and tested | When ready (no fixed schedule) |
| Major | Breaking changes accumulated | Minimum 12 months apart, soft target 18 months |

**Major release rules:**

- Never ship a major just because time passed — only when there are meaningful breaking changes to justify it.
- Deprecate in minors first. Majors remove what was deprecated.
- All libraries don't have to major-bump together. If only `http` has breaking changes, only `http` gets a new major. The framework bumps its constraint accordingly in its next minor/major.

## Deprecation Policy

1. Mark deprecated APIs with `@deprecated` tag and a note pointing to the replacement.
2. Deprecated APIs remain functional for at least one minor release cycle.
3. Removal happens only in the next major version.

## Support & Maintenance Windows

After a new major version ships:

| Window | Duration | Scope |
|--------|----------|-------|
| Active bug fixes | 6 months after next major ships | Bug fixes backported to old major |
| Security only | 6–12 months after next major ships | Critical security patches only |
| End of life | 12 months after next major ships | No further updates |

Example:

```
v3.0.0 ships: June 2026
v4.0.0 ships: ~Dec 2027
v3.x bug fixes: until June 2028
v3.x security fixes: until Dec 2028
v3.x EOL: Dec 2028
```

## Branch & Tag Strategy

| Branch | Purpose |
|--------|---------|
| `main` | Latest stable release. Tags are cut from here. |
| `dev` | Integration branch for next release. PRs merge here. |
| Feature branches | Short-lived, branched from `dev`. |

**Release flow:**

1. Work happens on `dev` (or feature branches → `dev`).
2. When ready to release, merge `dev` → `main`.
3. Tag from `main` (e.g., `v2.2.4`).
4. Push tag. CI publishes to Packagist automatically via release-please or manual GitHub Release.

## PHP Version Support

- Support at least the **5 latest PHP versions** at any time.
- Dropping an older PHP version is a **major** version bump.
- Adding support for a new PHP version is a **patch** (CI only) or **minor** (if it enables new features).

Currently: PHP 8.1, 8.2, 8.3, 8.4, 8.5.

## Framework ↔ Library Relationship

- The framework pins sub-libraries with **patch-flexible** constraints: `"webfiori/http": "v6.0.*"`
- A library's minor/patch release is automatically available to framework users via `composer update`.
- A library's major release requires a framework minor (new constraint) or major (if the framework's own API changes as a result).

## Pre-release Versions

Used only for the framework itself when coordinating large cross-library changes:

- Format: `X.Y.0-RC.N` or `X.Y.0-beta.N`
- Sub-libraries do **not** use pre-release tags — they go straight to stable.

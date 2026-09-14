# renovate-configs

Shared [Renovate](https://docs.renovatebot.com/) presets for Health Flare's
Flutter apps, so the mechanical bits of dependency-update policy (schedule,
labels, the Gradle `flutter-plugin-loader` lookup workaround, lockfile
maintenance, vulnerability-alert labeling, and the blanket "no major ever
auto-merges" rule) live in one place instead of being copy-pasted and
drifting between repos.

## Presets

### `flutter-app.json5`

Base config for a Health Flare Flutter app. Referenced from a repo's
`renovate.json5` as:

```json5
{
  extends: ['github>Health-Flare/renovate-configs//flutter-app.json5'],
}
```

(Renovate only auto-discovers a bare `default.json` when you extend
`github>owner/repo` with no path — a `.json5` file, or any other filename,
needs the explicit `//path` form above.)

This preset intentionally does **not** cover:

- Whether patch/minor updates are proposed at all, and which of them
  auto-merge. Each app's own risk tolerance here is deliberate and
  documented in that app's `renovate.json5` / `CLAUDE.md` — don't try to
  unify it here.
- `packageRules` for a specific dependency group (e.g. the `riverpod`
  family that must bump together) or per-manager automerge grouping (e.g.
  GitHub Actions). These interact with each app's own automerge rules in an
  order-sensitive way (Renovate applies `packageRules` in array order, and
  later matches override earlier ones for the same field), so they stay
  local to each app rather than risking a silent behavior change from
  merge ordering between this preset and each repo's local rules.
- Platform/CI wiring (e.g. self-hosted Gitea's `RENOVATE_PLATFORM` workflow)
  — that's infrastructure per-repo, not shared policy.

## Repos using this config

- [Health-Flare/InnerFlare](https://github.com/Health-Flare/InnerFlare)
- [Health-Flare/app](https://github.com/Health-Flare/app)

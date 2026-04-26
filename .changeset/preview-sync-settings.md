---
"wrangler": minor
---

`wrangler preview` now syncs your local previews config to the platform's shared Preview settings by default

When you run `wrangler preview`, the previews block in your Wrangler config is now automatically synced to the Worker's shared Preview settings on the platform — your config file is the source of truth, and the dashboard reflects it after each preview deployment.

Previously, running `wrangler preview` only applied your previews config as per-deployment overrides. The platform's shared settings (visible in the dashboard) would not update unless you separately ran `wrangler preview settings update`. This created a gap between what you deployed and what the platform knew about, and required users to remember a second command. The separate `settings update` command was also additive-only — it could push settings but never remove them, leaving stale state on the platform.

With this change:

- `wrangler preview` shows a diff of any changes to shared Preview settings and asks for confirmation before applying them
- Destructive changes (removing settings present on the platform but absent from your config) trigger a stronger confirmation prompt that explicitly mentions the change affects shared state
- `wrangler preview settings update` now performs full replacement instead of additive merge, making it possible to remove settings from the CLI for the first time
- A new `--no-sync` flag is available on `wrangler preview` for cases where you explicitly don't want to mutate shared state
- A new `--skip-confirmation` flag (alias `-y`) is available for non-interactive contexts

If your `wrangler.jsonc` has no `previews` block, behavior is unchanged.

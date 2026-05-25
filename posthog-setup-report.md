# PostHog post-wizard report

The wizard has completed a deep integration of PostHog analytics into your TanStack Start project (Skild). The integration adds client-side event tracking, a reverse proxy for reliable ingestion, and the PostHogProvider initialized at the root of the app so all routes have access to the PostHog client.

**Files changed:**

- `src/routes/__root.tsx` — Added `PostHogProvider` wrapping the full app body. Configured with `api_host: '/ingest'` (reverse proxy), env var-based token and host, `capture_exceptions: true`, and `defaults: '2025-05-24'`.
- `vite.config.ts` — Added dev-server proxy rules for `/ingest/static`, `/ingest/array`, and `/ingest` routes pointing to the PostHog US region ingestion and asset hosts.
- `src/components/SkillCard.tsx` — Added `usePostHog` hook; captures `skill_install_command_copied` (with skill title, category, and install command) on clipboard copy, and `skill_card_opened` (with skill title and category) on the Open link click.
- `src/routes/index.tsx` — Added `usePostHog` hook; captures `registry_browse_clicked` on the "Browse Registry" CTA and `publish_skill_clicked` on the "Publish Skill" CTA.
- `src/components/Navbar.tsx` — Added `usePostHog` hook; captures `sign_in_clicked` when the Sign In button is clicked.
- `.env` — Created with `VITE_PUBLIC_POSTHOG_PROJECT_TOKEN` and `VITE_PUBLIC_POSTHOG_HOST`.

| Event | Description | File |
|---|---|---|
| `skill_install_command_copied` | User copies the install command for a skill from the skill card | `src/components/SkillCard.tsx` |
| `skill_card_opened` | User clicks the Open link on a skill card to view the skill detail | `src/components/SkillCard.tsx` |
| `registry_browse_clicked` | User clicks the Browse Registry CTA button on the homepage hero | `src/routes/index.tsx` |
| `publish_skill_clicked` | User clicks the Publish Skill CTA button on the homepage hero | `src/routes/index.tsx` |
| `sign_in_clicked` | User clicks the Sign In button in the navbar | `src/components/Navbar.tsx` |

## Next steps

We've built some insights and a dashboard for you to keep an eye on user behavior, based on the events we just instrumented. Visit your PostHog project to create an "Analytics basics" dashboard with the following recommended insights:

- **Registry engagement funnel** — Funnel from `registry_browse_clicked` → `skill_card_opened` → `skill_install_command_copied`
- **Publish intent** — Trend of `publish_skill_clicked` over time
- **Sign-in conversion** — Trend of `sign_in_clicked` over time
- **Top skill commands copied** — `skill_install_command_copied` broken down by `skill_title`
- **Skill open rate** — `skill_card_opened` broken down by `skill_category`

[Open PostHog Project](https://us.posthog.com/project/279253/dashboard)

### Agent skill

We've left an agent skill folder in your project at `.claude/skills/integration-tanstack-start/`. You can use this context for further agent development when using Claude Code. This will help ensure the model provides the most up-to-date approaches for integrating PostHog.

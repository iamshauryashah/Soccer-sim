# League Lab — Cloudflare deployment

A self-contained soccer league evolution simulator. The original 20-club,
four-strategy simulation is preserved. HTML, CSS, and JavaScript are bundled
in `public/index.html`. No Python, build step, runtime dependencies, database,
or secrets are required.

Choose either Workers or Pages. You do not need both.

## Cloudflare Workers (Git repository deployment)

Upload the contents of this project directory to the root of your repository:

- `wrangler.jsonc`
- `public/index.html`
- `README.md`
- `.gitignore`

In Cloudflare Workers & Pages, create/import a Worker from that repository.
Use these settings:

| Setting | Value |
| --- | --- |
| Root directory | Repository root (leave default) |
| Build command | Leave blank |
| Deploy command | `npx wrangler deploy` |

The Wrangler configuration points to `public`, so Cloudflare can deploy the
static assets without a Worker JavaScript entry point. If using an existing
Worker, change `name` in `wrangler.jsonc` to match its name.

### Workers from a terminal

Run these commands inside the extracted project directory:

```sh
npx wrangler login
npx wrangler deploy
```

For local development, run `npx wrangler dev`.

## Cloudflare Pages (Git repository deployment)

Import the same repository as a Pages project. Use these settings:

| Setting | Value |
| --- | --- |
| Framework preset | None |
| Root directory | Repository root (leave default) |
| Build command | `exit 0` |
| Build output directory | `public` |

The root `wrangler.jsonc` is for Workers deployment; do not add
`pages_build_output_dir` to it. The Pages dashboard settings above define the
Pages build output separately.

### Pages direct upload

For a new Pages Direct Upload project, upload the **public folder only**.
It contains the website, with `index.html` at its top level. Do not upload the
entire project folder as the website assets.

Alternatively, from the project directory:

```sh
npx wrangler login
npx wrangler pages deploy public --project-name soccer-league
```

Select or create your Pages project when prompted.

## Editing and saved progress

Edit `public/index.html`, then redeploy. The simulator runs entirely in the
browser. Leagues are saved using localStorage per browser and site address.
Visitors each have their own league. Switching between Pages, Workers, or a
custom domain starts a separate save because browser storage is origin-specific.

## Verification

The simulator's logic was checked across 100 seasons (19,000 matches), including
19 matches per club, point totals, relegation, unique club names, and inheritance
of the top three clubs' strategies. Browser checks covered simulation, past
standings, saved progress after reload, and the reset cancellation control.
The deployment package has not been deployed to your Cloudflare account.

## Official Cloudflare references

- Workers static assets: https://developers.cloudflare.com/workers/static-assets/
- Workers configuration: https://developers.cloudflare.com/workers/static-assets/binding/
- Pages static HTML: https://developers.cloudflare.com/pages/framework-guides/deploy-anything/
- Pages direct upload: https://developers.cloudflare.com/pages/get-started/direct-upload/

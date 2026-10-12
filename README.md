# Vidyakosha — RCE Academic Resource Hub

Vidyakosha is a full-stack academic resource portal for students of Ramachandra College of Engineering, Eluru. It organizes curricula, syllabus links, study materials and verified supplementary resources by regulation, branch, year, semester and subject.

The repository contains the application source and public academic seed data. It does not include deployment credentials, production database contents or account-specific hosting identifiers.

## Features

- R23 navigation for nine engineering branches and four academic years
- 524 curriculum entries sourced from official RCE branch documents
- Subject pages with syllabus sections and links to original RCE PDFs
- Supplementary resources from NPTEL, MIT OpenCourseWare, OpenStax and official technical documentation
- Search, filters, bookmarks, recently viewed subjects and responsive layouts
- Separate labels for official sources, supplementary material and practice exams
- D1-backed administration, resource requests and audit history
- R2-backed administrator uploads
- ChatGPT Sites identity support with a server-side administrator allowlist

R23 semester VII–VIII subject tables, R26 curricula and authentic RCE past papers remain marked as unverified when an authoritative source is unavailable. Practice exams from other institutions are never presented as RCE papers.

## Technology

- React 19 and TypeScript
- Next-compatible routing through Vinext and Vite
- Cloudflare Workers runtime
- Cloudflare D1 with Drizzle ORM
- Cloudflare R2 for uploaded files

## Project structure

```text
app/          pages, API routes and the student/admin interfaces
data/         curriculum and supplementary-resource seed data
db/           Drizzle schema and D1 access
drizzle/      database migrations
lib/          catalog, validation and service logic
public/       logo and static assets
scripts/      install, build and local QA helpers
```

## Local setup

Requirements: Node.js 22.13 or newer and npm.

```bash
git clone <your-repository-url>
cd vidyakosha
npm ci
npm run build
```

Apply the initial migration to the local D1 database:

```bash
node --import ./scripts/sites-env.mjs ./node_modules/wrangler/bin/wrangler.js d1 execute DB --local --config dist/server/wrangler.json --persist-to .wrangler/state --file drizzle/0000_free_blade.sql
```

Start the development server:

```bash
npm run dev
```

Open `http://127.0.0.1:5173`.

For a production-like local check after building:

```bash
npm start
```

## Configuration

The application expects these bindings and variables:

| Name | Purpose |
| --- | --- |
| `DB` | Cloudflare D1 database binding |
| `BUCKET` | Cloudflare R2 bucket binding |
| `ADMIN_EMAIL` | Email allowed to use the administration area |
| `NEXT_PUBLIC_SITE_URL` | Canonical public origin used in page metadata |

Copy `.env.example` to `.env.local` for local values. Do not commit real credentials or private configuration.

Local ChatGPT Sites authentication is simulated only on loopback. Set `VIDYAKOSHA_PREVIEW_ADMIN=1` when starting the local server to use the email configured by `ADMIN_EMAIL` as the preview administrator.

## Deployment

The checked-in `.openai/hosting.json` declares logical D1 and R2 bindings without an account-specific project ID. When deploying with ChatGPT Sites, the platform adds the project identity and provides trusted sign-in headers.

For direct Cloudflare deployment, provision a D1 database and R2 bucket, apply the migration, bind them as `DB` and `BUCKET`, and configure `ADMIN_EMAIL`. The current sign-in flow relies on trusted ChatGPT Sites identity headers; replace it with your own authenticated identity provider before enabling administration or resource-request writes on another host.

## Academic data and attribution

Official curriculum links remain hosted by RCE. Supplementary resources remain hosted by their original publishers. Each entry records its source, type and access date. Review `data/subjects.json`, `data/supplementary.json` and the in-app Sources & Verification page before updating academic records.

This is an independent student resource hub and is not an official RCE or JNTUK website. Copyright remains with the original publishers.

## Validation

```bash
npx tsc --noEmit
npm run build
```

Optional local checks are available in `scripts/qa-local.mjs` and `scripts/qa-storage.mjs` after the local Worker and database have been prepared.

## Contributing

When adding academic content:

1. Link to the original authoritative or institutional source.
2. Record the source type and access date.
3. Do not label templates, model papers or third-party exams as RCE previous papers.
4. Keep unverified curricula visibly marked as awaiting verification.


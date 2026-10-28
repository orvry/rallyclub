# Rally Club Legal Center — Supabase deployment package

This bundle includes a branded legal landing page, standalone Terms of Service and Privacy Policy HTML, app icon assets, and a Supabase Edge Function that returns the pages as real HTML documents.

## Important: legal review before publishing

The Terms and Privacy Policy are **draft templates, not legal advice**. Before launch:

1. Replace every highlighted `[insert ...]` placeholder with the correct legal operator details, official contact email, address where required, and governing law/venue.
2. Verify the actual information collected by the Rally Club app, public visibility settings, account deletion behavior, data retention, Supabase project region, service providers, analytics, email, and any future payments.
3. Review the documents for the intended jurisdictions. If Japan's APPI applies, confirm required notices, handling of participant information and minors, and international-transfer disclosures against the actual setup.
4. Do not claim optional security, privacy, or deletion controls exist until they have been implemented and tested.

## Files

- `index.html` — static legal-center landing page.
- `terms.html` — standalone Terms of Service.
- `privacy.html` — standalone Privacy Policy.
- `assets/rally-club-app-icon.png` — 1024×1024 app icon with transparent outer corners.
- `assets/rally-club-icon-512.png` — 512×512 icon.
- `assets/apple-touch-icon.png` — 180×180 iOS home-screen icon.
- `assets/favicon-32.png`, `assets/favicon-64.png` — browser icons.
- `supabase/functions/rally-club-legal/index.ts` — public HTML endpoint with the pages embedded in source.
- `supabase/config.toml` — public function configuration (`verify_jwt = false`).

## Deploy to Supabase

Supabase Storage is not suitable for rendering an uploaded `.html` file as a website because Storage returns HTML files as plain text. This bundle uses a read-only Edge Function to return `Content-Type: text/html` instead. See the official Supabase docs linked below.

Prerequisites: a Supabase project, Supabase CLI installed locally, and access to the project.

```bash
# From this extracted folder
supabase login
supabase projects list
supabase link --project-ref YOUR_PROJECT_REF
supabase functions deploy rally-club-legal
```

The function URL will be:

```text
https://YOUR_PROJECT_REF.supabase.co/functions/v1/rally-club-legal
```

Direct policy URLs:

```text
https://YOUR_PROJECT_REF.supabase.co/functions/v1/rally-club-legal?page=terms
https://YOUR_PROJECT_REF.supabase.co/functions/v1/rally-club-legal?page=privacy
```

The function is public because the Terms and Privacy Policy are public documents. It has no database operations and embeds no API keys or secrets. Test each URL in a private browser window after deployment.

## Notes

- These are function URLs, not custom-domain URLs. Configure a branded domain separately if desired.
- The Edge Function embeds an inlined copy of the page HTML so it works without static-file reads or external asset hosting.
- Static `.html` files are provided for local preview or a conventional static host. Their icons are embedded as data URLs so each page is self-contained.

## Official references

- Supabase Storage serving files: https://supabase.com/docs/guides/storage/serving/downloads
- Supabase Storage quickstart (HTML is returned as plain text): https://supabase.com/docs/guides/storage/quickstart
- Supabase Edge Function deployment: https://supabase.com/docs/guides/functions/deploy
- Supabase function configuration: https://supabase.com/docs/guides/functions/function-configuration
- Japan Personal Information Protection Commission, laws and policies: https://www.ppc.go.jp/en/legal/

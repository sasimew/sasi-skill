# UTM and GA4 Attribution Skill

## Purpose

Create a shareable link that identifies where a visitor came from in GA4,
without adding a new analytics tag or redesigning the website.

This skill uses the existing SASI.ASIA GA4 measurement ID:

G-S0N3M3191G

## Quick Answer: Does UTM Require Deployment?

### No deployment required

No deployment is required when all of these are true:

- The destination page already has the existing GA4 tag.
- You are only creating and sharing a URL with UTM parameters.
- You are not changing website code.
- The link points to an existing live page.

Example:

https://sasi.asia/?utm_source=jenosize&utm_medium=referral&utm_campaign=jenosize_profile&utm_content=website_link

GA4 reads the UTM parameters from the landing URL and attributes the session.

### Deployment required

Deploy the website when any of these apply:

- The GA4 tag is missing from the destination page.
- The destination page or tracking code was changed.
- A custom event is being added or changed.
- The UTM link is being embedded into a website button or card.
- A redirect, short link, landing route, or server rule is being created.
- The UTM values need to be preserved through custom JavaScript or a form flow.

For a normal UTM-only link, do not deploy just to create the URL.

## UTM Naming Standard

Use lowercase, stable, descriptive values.

| Parameter | Meaning | Example |
|---|---|---|
| utm_source | Referring partner or platform | jenosize |
| utm_medium | Traffic type | referral |
| utm_campaign | Business campaign name | jenosize_profile |
| utm_content | Link placement or variation | website_link |
| utm_term | Paid-search keyword only | optional |

Do not put email addresses, phone numbers, names, client data, or secrets into
UTM values. UTM values can appear in browser history, analytics reports, logs,
and copied URLs.

## Recommended Jenosize Link

https://sasi.asia/?utm_source=jenosize&utm_medium=referral&utm_campaign=jenosize_profile&utm_content=website_link

For a contact-page destination:

https://sasi.asia/contact.html?utm_source=jenosize&utm_medium=referral&utm_campaign=jenosize_profile&utm_content=contact_link

## Creating A New UTM Link

1. Choose the exact live destination page.
2. Set source to the referring partner, in lowercase.
3. Set medium to referral, social, email, or another accurate channel.
4. Choose a stable campaign name.
5. Use content to distinguish placement or creative.
6. URL-encode spaces and reserved characters.
7. Test the final URL in a private browser window.
8. Confirm the destination loads over HTTPS.
9. Record the final link and naming in the campaign notes.
10. Do not add duplicate GA4 tags.

### Shell helper

Replace the values before sharing:

~~~
python3 - <<'PY'
from urllib.parse import urlencode

params = {
    "utm_source": "jenosize",
    "utm_medium": "referral",
    "utm_campaign": "jenosize_profile",
    "utm_content": "website_link",
}
print("https://sasi.asia/?" + urlencode(params))
PY
~~~

## GA4 Reporting

Open GA4 for the property connected to G-S0N3M3191G.

1. Open Reports.
2. Open Acquisition.
3. Open Traffic acquisition.
4. Select or add the dimensions:
   - Session source
   - Session medium
   - Session campaign
5. Filter for:
   - Session source = jenosize
   - Session medium = referral
   - Session campaign = jenosize_profile
6. Review sessions, engaged sessions, engagement rate, key events, and
   conversions.

The expected acquisition label is:

jenosize / referral

The expected campaign is:

jenosize_profile

## Realtime Verification

After opening the UTM link:

1. Open GA4 Realtime.
2. Confirm an active user appears.
3. Open the user's event details when available.
4. Confirm page_view and the campaign attribution.
5. Wait for standard reports to process before judging final totals.

GA4 reports can have processing delay. Realtime is for a quick technical check,
not the final campaign report.

## QA Checklist

- The URL uses HTTPS.
- utm_source is lowercase and identifies the partner.
- utm_medium accurately describes the channel.
- utm_campaign is stable and consistent.
- The destination page returns HTTP 200.
- The destination page includes G-S0N3M3191G.
- No duplicate GA4 measurement tag exists.
- No personal data is present in the URL.
- The link works in a private browser window.
- GA4 Realtime shows the visit.
- The link is recorded for future reuse.

## Common Mistakes

- Using different spelling or capitalization for the same source.
- Using social as the medium for a partner referral.
- Adding UTM parameters to internal navigation links.
- Putting an email address or customer identifier in the URL.
- Expecting standard GA4 reports to update instantly.
- Adding a second GA4 tag when the existing tag already works.
- Testing from a browser with an existing session and assuming attribution is new.
- Redirecting through a server rule that strips the query string.

## Future Skill Update Flow

When this skill or another skill changes:

~~~
cd /path/to/sasi-skill
gh auth status
git status -sb
git diff --check
rg -n "ghp_|github_pat_|BEGIN .*PRIVATE|password|api_key|token\\s*=" \
  --glob '!.git/**' .
git add README.md *.md
git diff --cached --name-status
git commit -m "skill: update UTM and GA4 attribution guide"
git pull --rebase origin main
git push origin main
~~~

Never commit a GitHub PAT. Keep authentication in GitHub CLI and the local
operating system keychain.

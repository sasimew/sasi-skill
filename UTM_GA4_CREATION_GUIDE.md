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

## How UTM Reaches GA4

UTM is not a second GA4 tag and it is not normally hardcoded into the
website's analytics code. UTM is a query string attached to the shared URL.
The existing GA4 tag reads the landing page visit and GA4 uses the UTM values
as acquisition information.

### End-to-end flow

~~~text
1. Jenosize shares the tagged URL
   https://sasi.asia/?utm_source=jenosize&utm_medium=referral&...

2. The visitor clicks the URL

3. The browser requests the page and keeps the query string in location.search

4. The page loads the existing gtag configuration for G-S0N3M3191G

5. gtag sends the initial page_view to the GA4 property

6. GA4 reads the UTM values and assigns campaign attribution to the session

7. Later events from that session can be analysed with the same acquisition
   dimensions, subject to GA4 attribution and session rules

8. GA4 Realtime can show the visit quickly; standard reports process later
~~~

### URL-to-GA4 mapping

| URL value | GA4 acquisition dimension | Example result |
|---|---|---|
| utm_source=jenosize | Session source | jenosize |
| utm_medium=referral | Session medium | referral |
| utm_campaign=jenosize_profile | Session campaign | jenosize_profile |
| utm_content=website_link | Session manual ad content | website_link |

The website only needs one working GA4 implementation on the destination page.
The UTM link and the existing `page_view` are enough for standard campaign
reporting. The optional `jenosize_landing` event currently used on SASI.ASIA
is an extra diagnostic event that can show which partner landing event fired;
it is not required for normal UTM attribution and must not be duplicated.

### Conditions for attribution to work

- The destination page must load `G-S0N3M3191G` exactly once.
- The URL query string must reach the page without being stripped by a redirect.
- The visitor must open the tagged URL before the relevant session begins.
- The visitor's browser must allow the GA4 request; ad blockers or consent
  controls can prevent measurement.
- Internal links should not carry the partner UTM values, or later visits may
  be misclassified as coming from the partner.

If only the URL is created and shared, no deployment is required. Deployment
is required when the tagged URL is embedded into website code, a redirect is
changed, or the GA4/custom tracking implementation is changed.

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

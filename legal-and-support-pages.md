# Legal + Support Pages for the iOS Submission — Work Sheet

Working doc for the four web pages the App Store submission depends on:
`/support`, `/contact`, and app-fitted versions of `/privacy` and `/terms`.

Not legal advice. Everything below is drafting input and App Store requirements;
have counsel review before publishing, especially the arbitration, affiliate, and
state-privacy-rights language carried over from the existing pages.

---

## Why these four pages

| App Store Connect field | Required | Today |
| --- | --- | --- |
| Support URL | **Yes** | `link.ai/support` → 404, `link.ai/contact` → 404 |
| Privacy Policy URL | **Yes** | `link.ai/prototypes/privacy-policy.html` → 200, but written for the whole platform |
| Marketing URL | No | `link.ai` → 200 |
| Custom EULA | No (Apple's standard EULA applies if omitted) | `link.ai/prototypes/terms-of-service.html` → 200, linked in-app |

The app links both legal pages from Settings — `src/app/(drawer)/(tabs)/more.tsx:53-54` —
so whatever gets published has to be the URL used there too.

Hard requirements for all four pages: publicly reachable with **no login and no paywall**,
readable on a phone-width viewport, and stable URLs (Apple re-checks them on every update).
A support URL that 404s or that requires an account is a Guideline 1.5 rejection.

Current source of truth for contact details, carried from the existing pages:
Link Management Systems, Inc., 202 W 140th St, Los Angeles, CA 90061,
phone (310) 695-6510, `privacy@link.ai` (privacy + DMCA, agent John Guints),
`contact@link.ai` (general). `/data-deletion` is already live on `beta.link.ai`
and `app.link.ai`.

---

## What the app actually does — the fact base

Write the pages against this list, not against the platform-wide feature set. The
privacy answers filed in App Store Connect must match it exactly; a mismatch is a
Guideline 5.1.2 rejection.

**Features shipped in the iOS app** (from `src/app`): dashboard, property links with
QR sharing, buyer tours (build, schedule, review, publish, stops), leads, saved
searches, valuations, auto-emails, flows/workflows, an AI copilot chat scoped to a
listing, notifications and notification settings, profile with photo upload, and
in-app account deletion.

**Accounts are invite-only.** No sign-up screen — a brokerage provisions the agent
(`agentInvites`). Login is Auth0: email + password, Google, and Apple.

**Data the app collects**
- Identity from Auth0: name, email, user ID. Auth0 stores tokens in the iOS Keychain.
- Profile photo, uploaded through `expo-image-picker` (photo-library permission only;
  camera and microphone are explicitly disabled in `app.json`).
- Business content the agent enters or generates: links, leads, tours, saved searches,
  valuations, emails, copilot conversations. Lead records contain **consumer** personal
  information the agent enters or that a consumer submits through a shared link or tour.
- Expo push token, when notifications are allowed.

**Data the app does not collect:** no location (no `expo-location`, no geolocation call
anywhere in `src`), no analytics, attribution, advertising, or crash-reporting SDK, no
cookies (native app), no payments in the app, no SMS sent from the app, no contacts,
calendar, camera, mic, or health data. Hence no ATT prompt and no
`NSUserTrackingUsageDescription`.

**Subprocessors reachable from app data** (verified in `../link-tools`): Auth0 (identity),
Convex (application database and functions, deployment `proper-falcon-809`), Expo push
service → Apple APNs (notifications), OpenAI via `@ai-sdk/openai` (copilot, tour and
content AI), Mapbox (geocoding and property search), AWS SES (outbound email).

**Deletion behaviour** (`../link-tools`, `agents.requestAccountDeletion` +
`crons.purgeDeletedAgents`): deletion takes effect immediately — push tokens cleared,
invite row removed, email and token identifier moved to sentinels so login can no longer
resolve the account — and remaining PII on the agent record is anonymised by a daily cron
30 days later. Open point that the policy must state truthfully: the agent's **business
records (links, leads, tours, branding) are not deleted** by the purge; they are excluded
from public pages instead. Say what is true, or change the code first.

---

## Page 1 — `/support`

Purpose: the reviewer and real users both land here. It must answer "how do I get help"
without an account.

Must contain:
1. Product name and one line on what it is: "LINK Tools — the iOS app for LINK agents."
2. **How to get help**, with at least one channel Apple can see: email address and phone,
   plus expected response time (state real business hours, e.g. Mon–Fri 9am–5pm PT).
3. **Access note:** accounts are provisioned by the brokerage; there is no public sign-up.
   Tell a user with no account who to ask.
4. **Self-serve answers** for the things support actually gets asked:
   - Sign-in trouble (Apple / Google / email + password, password reset).
   - Turning notifications on or off (OS Settings → LINK Tools → Notifications, and the
     in-app notification settings screen).
   - Deleting an account, linking to `/data-deletion` and stating the immediate-effect
     plus 30-day purge timeline.
   - Where profile photo, links, leads, tours, valuations live — one line each.
5. **Requirements:** iPhone, iOS version floor, and that the app is iPhone-only.
6. **Reporting a bug:** what to include — app version and build, iOS version, screenshot,
   time of the problem, account email.
7. Links to `/privacy`, `/terms`, `/contact`, `/data-deletion`.
8. "Last updated" date.

Draft copy to edit:

> ### LINK Tools Support
> LINK Tools is the iPhone app for LINK agents — property links, buyer tours, leads,
> saved searches, valuations, and the AI copilot, on the same account you use on the web.
>
> **Contact us:** support@link.ai · (310) 695-6510 · Mon–Fri, 9am–5pm Pacific.
> We reply to email within one business day.
>
> **No account yet?** LINK Tools accounts are created by your brokerage — there is no
> public sign-up. Ask your broker or office admin to invite your work email, then sign in
> with Apple, Google, or your email and password.
>
> **Common questions**
> - *I can't sign in.* Use the same method you used the first time. Email + password
>   resets are handled on the sign-in screen. If your invite was sent to a different
>   address than the one you are trying, contact your brokerage.
> - *I'm not getting notifications.* Check iOS Settings → LINK Tools → Notifications, then
>   Notification settings in the app. The app works fully with notifications turned off.
> - *I want to delete my account.* Profile → Danger zone → Delete account, or see
>   [Data deletion](/data-deletion). Deletion takes effect right away and remaining
>   personal information is purged within 30 days.
> - *Something looks wrong.* Email us with your app version and build (Settings → About),
>   your iOS version, a screenshot, and roughly when it happened.
>
> **Requirements:** iPhone running iOS 16.4 or later. LINK Tools is an iPhone app; it is
> not offered for iPad.
>
> [Privacy Policy](/privacy) · [Terms of Service](/terms) · [Contact](/contact)
> Last updated <date>.

Before publishing: confirm `support@link.ai` is a real monitored mailbox. If it is not,
use `contact@link.ai` — a support URL pointing at a dead address is worse than none.
The iOS floor is **16.4**, from the generated project
(`ios/Podfile:25`, `IPHONEOS_DEPLOYMENT_TARGET = 16.4`); re-check it if the SDK is bumped.

## Page 2 — `/contact`

Purpose: company-level reachability, and where the legal and privacy addresses live.

Must contain:
1. Legal entity name, mailing address, phone.
2. Routed email addresses, so mail does not land in one inbox:
   `support@link.ai` (product help) · `privacy@link.ai` (privacy requests and CCPA/CPRA
   rights) · `privacy@link.ai` DMCA notices, attn. Designated Copyright Agent John Guints
   · `contact@link.ai` (everything else). Keep these consistent with whatever the final
   `/privacy` and `/terms` say — the existing pages already use `privacy@link.ai` and
   `contact@link.ai`.
3. The affiliate note carried from the current Terms: mortgage is First Link Mortgage
   (NMLS #2527988), escrow is Linked Escrow (DFPI No. 96DBO-189708), each with its own
   contact route, and Link Management Systems is not a broker, lender, or escrow agent.
4. If the page has a form: state what happens to what is submitted, do not require a phone
   number, and if a phone number is collected add the consent line the current privacy page
   already uses for messaging (TCPA). Do not add a marketing-consent checkbox that is
   pre-checked.
5. Business hours and timezone.
6. Links to `/support`, `/privacy`, `/terms`, `/data-deletion`.

Keep it static HTML if the form needs backend work — Apple needs the page, not the form.

---

## Page 3 — app-fitted `/privacy`

Base on the current `privacy-policy.html` (updated July 17, 2026) and keep its structure.
Two constraints drive every edit: the policy must cover the app, and it must **not claim
behaviour the app does not have** — the App Privacy labels are checked against it.

Section-by-section:

| Current section | Do this |
| --- | --- |
| Scope; Financial Services Affiliates | **Keep as is.** GLBA carve-out for First Link Mortgage / Linked Escrow still applies. |
| 1. Information You Provide | **Rewrite for the app.** Name, email, password, profile photo, professional licence and brokerage affiliation, plus the business content agents enter: links, leads, tours, saved searches, valuations, emails, copilot messages. Say plainly that lead records contain consumer information the agent enters or a consumer submits through a shared link or tour. Drop payment information for the app (no in-app purchases) or scope it to the web platform. |
| 1. Collected Automatically | **Trim hard.** The app collects no location, no analytics, no cookies, no advertising identifiers. What is true: server-side log data from API requests, device and OS information carried by those requests, and the Expo push token when notifications are allowed. Delete the location line and the "usage data and analytics" line as they apply to the app, or scope them explicitly to the website. |
| 1. Third Parties | Keep MLS / real estate data providers. Keep social platforms but narrow it to **login** — Google and Apple sign-in through Auth0, which returns name, email, and a user ID (and note Apple's Hide My Email relay). Drop "analytics partners" unless the website really has them; the app has none. |
| 2. How We Use Your Information | Keep, and delete or scope "personalize and improve your experience" / promotional communications so they do not conflict with the no-tracking declaration. Add: to send push notifications you have allowed, and to generate AI drafts you ask for. |
| 3. Text Messaging | The app sends no SMS. Keep only if the platform does, and scope it to the website. |
| 4. Sharing of Information | **This is the section most likely to cause a rejection.** The current page says third-party advertising and analytics cookies may constitute CPRA "sharing" for cross-context behavioural advertising. That is false for the app, and it contradicts Tracking = No in App Privacy. Either add an explicit sentence — "The LINK Tools mobile app contains no advertising or analytics SDKs; we do not share information from the app for cross-context behavioural advertising" — or split the sharing section per surface. Then add the real subprocessor list: Auth0, Convex, Expo push service and Apple APNs, OpenAI, Mapbox, AWS SES, each with its one-line purpose. |
| 5. Data Retention | Add the deletion mechanics: deletion requested in the app takes effect immediately; remaining personal information on the account record is anonymised within 30 days; records retained longer where real estate, mortgage, or escrow rules require. State truthfully what survives — today the agent's business records are retained and excluded from public pages rather than deleted. |
| 6. Security | Keep. Add that authentication tokens are stored in the iOS Keychain. |
| 7. Your Choices and Rights | Add in-app deletion: Profile → Danger zone → Delete account, plus `/data-deletion` for anyone without app access. Add the notification opt-out (OS permission and the in-app toggle) and the photo-library permission being optional and revocable in iOS Settings. Keep the state-rights subsections. |
| 8. Cookies | Scope to the website. One line that the app uses no cookies. |
| 9. Third-Party Links | Keep. The app opens links in an in-app browser (`expo-web-browser`). |
| 10. Children's Privacy | Keep. The app is 4+ rated but professional-use, 18+ per the Terms; keep the under-13 and under-16 language consistent with that. |
| 11. Changes | Keep. |
| 12. Contact Us | Keep. Add that requests can also come from the app's Support link. |
| **New: AI features** | Add a short section: copilot and content-generation features send the relevant listing, tour, or conversation text to OpenAI to produce a draft; drafts are the agent's responsibility to review (mirrors Terms §11). State whether that content is used for model training — the honest answer for API use is no. |
| **New: App Store data summary** | A three-row table matching the App Privacy answers: Contact Info (name, email) — app functionality, linked, not tracking; User Content (photos, agent-entered content) — app functionality, linked, not tracking; Identifiers (user ID) — app functionality, linked, not tracking. Having it in the policy makes review trivial. |

Publish at a clean, stable path (`link.ai/privacy`, not `/prototypes/...`), redirect the
prototype URL to it, and update `PRIVACY_URL` in `src/app/(drawer)/(tabs)/more.tsx`.

## Page 4 — app-fitted `/terms`

Base on the current `terms-of-service.html`. Most of it transfers unchanged: acceptance,
affiliate/regulated-services disclosures (§2.1, §9, §10), professional licensing, permitted
use, user content, IP, DMCA, AI-generated content (§11 — directly relevant to copilot),
disclaimers, limitation of liability, indemnification, termination, governing law and
arbitration, fair housing, changes, severability, contact.

Edits and additions for the app:

1. **§2 Description of Services** — add the app: property links and QR sharing, buyer
   tours, leads, saved searches, valuations, auto-emails, flows, AI copilot, notifications.
2. **§3 Account Registration** — the app has no self sign-up; accounts are provisioned by
   the brokerage. Add that brokerages may revoke access and that deletion is available in
   the app.
3. **New: Apple-required EULA terms.** If this page is used as a custom EULA (it is, since
   the app links it), Apple's Schedule 1 minimum terms must appear:
   - The agreement is between the user and Link only, **not with Apple**.
   - Licence scope: non-transferable licence to use the app on Apple-branded devices the
     user owns or controls, per the Usage Rules of the App Store Terms of Service.
   - Apple has **no obligation to provide maintenance or support**.
   - Apple has no warranty obligation; any failure to conform to a warranty is Apple's
     only refund exposure, to the extent permitted by law.
   - Link, not Apple, is responsible for product liability, legal-compliance, and
     third-party **intellectual property** claims relating to the app.
   - The user represents they are not in a US-embargoed country and not on a prohibited
     parties list (export compliance).
   - Third-party beneficiary: **Apple and its subsidiaries may enforce these terms.**
   - Link's contact information for questions and complaints: address, phone, support email.
   Note that these sit alongside — and, for the app, are read together with — the
   arbitration clause in §18.
4. **§18 Arbitration** — keep, but check the 30-day opt-out address and that the notice
   route also works for app-only users.
5. **No in-app purchases** — state that the app is provided as part of the brokerage
   subscription and sells nothing in-app, so no Apple IAP obligation is implied.
6. **Age** — §1 says 18+; the App Store age rating is 4+. That is consistent (rating is
   content-based), but do not raise the rating without reason.

---

## Publish checklist

- [ ] `/support` live, no login, phone-readable, contact address monitored.
- [ ] `/contact` live with entity name, address, phone, routed emails, affiliate note.
- [ ] `/privacy` live at a stable path; prototype URL redirects to it.
- [ ] `/terms` live at a stable path with the Apple EULA terms added; prototype URL redirects.
- [ ] `PRIVACY_URL` and `TERMS_URL` in `src/app/(drawer)/(tabs)/more.tsx` point at the new paths.
- [ ] App Store Connect: Support URL = `/support`, Privacy Policy URL = `/privacy`,
      Marketing URL (optional) = `link.ai`, EULA = the `/terms` text if not using Apple's standard.
- [ ] App Privacy answers match the privacy policy's data table, line for line.
- [ ] All four pages carry the same "last updated" date and the same contact addresses.
- [ ] `curl -sI` each URL → 200 after deploy, from outside any VPN.

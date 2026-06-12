# CleanerZX Privacy Policy

**In effect from:** June 12, 2026

CleanerZX is an iPhone and iPad app published by **ZXApps** (the developer behind the apps offered from the contact address listed in [§19](#19-how-to-reach-us)). It helps you reclaim storage by spotting duplicate photos, oversized videos, screenshot clutter, and messy contact cards. This document explains, in plain language, what the app does with information on your device — and what it deliberately avoids doing.

In this document, "**CleanerZX**" or "the app" refers to the application; "**ZXApps**," "**we**," "**us**," or "**our**" refers to the publisher.

The short version is below in [§1](#1-the-shortest-possible-summary). The long version takes the same statements apart and explains them piece by piece.

---

## Index

1. [The shortest possible summary](#1-the-shortest-possible-summary)
2. [How CleanerZX is built](#2-how-cleanerzx-is-built)
3. [Inventory of on-device data the app reads](#3-inventory-of-on-device-data-the-app-reads)
4. [Things CleanerZX never reads, stores, or sends](#4-things-cleanerzx-never-reads-stores-or-sends)
5. [Every permission the app may ask for](#5-every-permission-the-app-may-ask-for)
6. [Where your data physically lives](#6-where-your-data-physically-lives)
7. [Network behavior of the app](#7-network-behavior-of-the-app)
8. [Notifications, widgets, and Live Activities](#8-notifications-widgets-and-live-activities)
9. [Subscriptions](#9-subscriptions)
10. [Apple frameworks the app relies on](#10-apple-frameworks-the-app-relies-on)
11. [Security posture](#11-security-posture)
12. [Use by minors](#12-use-by-minors)
13. [Controls you always have through iOS](#13-controls-you-always-have-through-ios)
14. [Notice for users in the EU, the EEA, the UK, and Switzerland](#14-notice-for-users-in-the-eu-the-eea-the-uk-and-switzerland)
15. [Notice for users in California](#15-notice-for-users-in-california)
16. [Notice for users elsewhere](#16-notice-for-users-elsewhere)
17. [Cross-border transfers](#17-cross-border-transfers)
18. [Updates to this document](#18-updates-to-this-document)
19. [How to reach us](#19-how-to-reach-us)

---

## 1. The shortest possible summary

If you stop reading after this section, take these eight facts with you:

1. CleanerZX runs almost entirely on your iPhone or iPad. ZXApps does not operate a server that stores your data.
2. The app never uploads your photos, videos, contacts, location, face data, files, or scan results to anyone.
3. The app uses **one** third-party SDK — Google's **Firebase Analytics** — to collect anonymous, aggregated usage statistics (which screens and features are used). It has no advertising SDK, no attribution SDK, and no third-party crash reporter. See [§3.7](#37-anonymous-usage-analytics) and [§7](#7-network-behavior-of-the-app).
4. The app does not use the advertising identifier (IDFA) and does no cross-app or cross-site tracking. Firebase Analytics uses a per-install identifier and the vendor identifier (IDFV) only to count usage, never to track you across other companies' apps or websites.
5. You are not asked to create an account, sign in, or share your email to use the app.
6. Permissions (Photos, Contacts, Location, Notifications) are optional and independent. Denying one only disables that one feature.
7. CleanerZX Pro is a subscription sold by Apple through the App Store. We do not see your payment details.
8. Deleting the app from your device deletes the local data the app stored on that device.

The remainder of this document expands on each of these claims so you can verify them.

---

## 2. How CleanerZX is built

CleanerZX is what the iOS community calls a **local-first** app. Practically, that means:

- All scanning and analysis runs on the same device that holds your photos, contacts, and files. The CPU, GPU, and Neural Engine on your iPhone or iPad do the work.
- The app does not maintain a backend service, user database, or login system.
- The app embeds exactly one third-party SDK: Google's **Firebase Analytics**, used only to collect anonymous, aggregated usage statistics so we can understand which features are useful and improve the app. It is described in [§3.7](#37-anonymous-usage-analytics) and [§7](#7-network-behavior-of-the-app). The app embeds **no** SDKs from advertising networks, attribution providers, marketing automation platforms, social media companies, or external crash-reporting services.
- The face-grouping feature uses an open-source face-embedding neural network called **SFace**, published by OpenCV under the Apache 2.0 license. The model ships inside the app bundle as Core ML weights, runs locally on your device's CPU and Neural Engine, and never communicates with any server. Image embeddings produced by that model stay on the device and are not exported. Open-source attribution is available in **Account → Licenses** inside the app.

The reason these architectural choices matter for privacy is straightforward: your personal content — photos, videos, contacts, files, face data, precise location, and scan results — never leaves your device. The only information that does leave is the anonymous, aggregated usage analytics described in [§3.7](#37-anonymous-usage-analytics).

---

## 3. Inventory of on-device data the app reads

The sections below list, by category, every type of information CleanerZX touches while it is running. **Everything in sections 3.1–3.6 is read and processed only on your device and is never uploaded.** The single exception is the anonymous usage analytics described in [§3.7](#37-anonymous-usage-analytics).

### 3.1 Your photo library

When you grant access to Photos, CleanerZX uses Apple's PhotoKit framework to look at your library and:

- find groups of photos that look the same or were taken within seconds of each other;
- find screenshots, screen recordings, large videos, and other clutter categories;
- compute on-device face embeddings (small mathematical vectors) so similar faces can be grouped together;
- read the latitude/longitude metadata stored inside each photo so place-based grouping can work offline;
- generate thumbnails for the in-app previews you tap on before deciding what to delete.

PhotoKit supports two modes — **Full Access** and **Limited Access**. Either works with CleanerZX. If you choose Limited Access, the app only sees the items you specifically picked.

### 3.2 Your contacts

When you grant access to Contacts, the app uses Apple's Contacts framework to find:

- exact duplicate contact cards;
- near-duplicate cards with the same phone number or email under different names;
- empty cards (no name, no number, no email);
- cards missing a name.

The app reads contact fields into memory for the length of a scan and presents them to you so you can choose which duplicates to merge. None of this content is copied off the device.

### 3.3 Your current location, on demand

The Library tab contains a Places map that shows you where your photos were taken. The map by itself does not need any iOS Location permission — it reads coordinates from photo metadata. The only time the app touches Core Location is when you tap the "center on me" control. At that point iOS asks you for **When In Use** access, and the resulting coordinate is used solely to recenter the map. The app does not record it, does not derive your home or work address from it, and does not perform background location.

### 3.4 Folders you point the app at

If you use the file-review feature, you can hand the app a folder via the iOS document picker. The app reads file size, modification date, and file type so it can list candidates for cleanup. It uses Apple's security-scoped bookmark APIs, which is the standard sandbox-safe way for an iOS app to look inside a user-selected folder.

### 3.5 Free space and total space

To draw the storage indicator on the home screen, the app reads the total and available capacity of the local volume using standard `URLResourceKey` values. This is the same API that the iOS Settings app uses.

### 3.6 App-internal state

CleanerZX keeps a small amount of bookkeeping on the device:

- whether onboarding has been completed,
- the most recent scan summary,
- caches that speed up the next scan,
- a copy of the subscription status reported by StoreKit,
- a few flags shared with the home-screen widget and Live Activity so they can stay in sync.

This is stored in the app's own iOS container and in an App Group container shared with the app's widget and Live Activity extensions. Nothing in this subsection leaves the device.

### 3.7 Anonymous usage analytics

CleanerZX uses **Google Firebase Analytics** to understand, in aggregate, how the app is used — for example, which screens are opened most often and how often a Quick or Deep scan is started. This helps us decide what to improve.

What is sent to Google for analytics:

- **Anonymous identifiers** — a random per-install "app instance ID" and your device's vendor identifier (IDFV). These identify an installation of the app, not you. They are **not** the advertising identifier (IDFA), which the app does not use.
- **Product-interaction / usage events** — for example `screen_view` (which screen was shown) and `scan_button_tap` (which scan type was tapped). These events do **not** contain your photos, videos, contacts, files, or the contents of your library.
- **Basic technical/diagnostic data** automatically recorded by Firebase — such as app version, device model, operating-system version, language, and a coarse region (country/region) derived from the IP address of the request. Google uses the IP address to derive coarse location and does **not** log it in Analytics.

This data is **not linked to your identity** and is **not used to track you** across other companies' apps or websites. Google processes it as our service provider/processor; its handling is also governed by Google's Privacy Policy (<https://policies.google.com/privacy>) and the Firebase data-processing terms. You can read how Firebase uses data at <https://firebase.google.com/support/privacy>.

Because this data is anonymous and not tied to you, deleting the app stops all further analytics collection from your device. See [§13](#13-controls-you-always-have-through-ios) for the controls available to you.

---

## 4. Things CleanerZX never reads, stores, or sends

For complete clarity, here are categories of data the app does **not** touch:

- Your name, email address, phone number, mailing address, or any other contact detail about you.
- Login credentials for any service. (You are never asked to sign in.)
- Your photos, videos, thumbnails, or face embeddings — none of these are uploaded, anywhere, ever.
- Your contact cards or any field inside them.
- Your home address, work address, frequently-visited places, or location history.
- The Apple advertising identifier (IDFA). (Firebase Analytics uses a per-install ID and the vendor identifier (IDFV) to count usage — see [§3.7](#37-anonymous-usage-analytics) — but never the IDFA.)
- Any cross-app tracking identifier or fingerprint, and no tracking of you across other companies' apps or websites.
- Full crash dumps or detailed diagnostic logs — there is no third-party crash reporter wired in. (Firebase Analytics does record basic technical signals such as app version and OS version, as described in [§3.7](#37-anonymous-usage-analytics).)
- Browsing history, search history, Safari activity, or anything from other apps.
- Biometric data, health data, financial data, or any "sensitive" personal data as those terms are defined under GDPR or CCPA.

The only information that ever leaves your device is the anonymous usage analytics in [§3.7](#37-anonymous-usage-analytics). The app uses no advertising cookies, pixels, or tracking beacons, and does not run a web view for advertising or tracking purposes.

---

## 5. Every permission the app may ask for

CleanerZX requests only the permissions listed below, and only at the moment a feature you tapped on requires them.

| iOS permission | Why it is needed | When the prompt appears |
| --- | --- | --- |
| Photos (read/write) | Scanning your library for duplicates, large files, and clutter; showing previews. | The first time you start a photo scan or open the library browser. |
| Photos — Add Only | Saving a compressed copy of a video back into your photo library. | Only when you choose to compress a specific video. |
| Contacts | Finding duplicate, empty, or incomplete contact cards. | The first time you open the Contacts feature. |
| Location — When In Use | Centering the Places map on where you currently are. | Only when you tap the current-location button on that map. |
| Notifications | Sending the cleanup reminders you opt into. | When you turn on reminders inside the app. |

Saying "no" to any prompt simply disables the feature that needed it. The rest of the app continues to work.

---

## 6. Where your data physically lives

Because everything is on-device, it helps to know which on-device location holds what:

| Bucket | What's inside | When it goes away |
| --- | --- | --- |
| Standard UserDefaults | App preferences, onboarding flag, coachmark flags, free-usage counter, cached subscription state. | When you uninstall the app or use iOS's "Reset" controls for it. |
| App Group container `group.com.zxapps.cleanerzx` | A small shared payload that lets the widget and Live Activity show the same numbers as the main app. | When you uninstall the app. |
| Local SwiftData store | The latest scan results, scan history, and review state. | When you clear scan history inside the app, when a new scan supersedes the old one, or when you uninstall the app. |
| In-app caches | Thumbnails, face groupings, place groupings, derived layouts. | When the app invalidates them, when iOS evicts them under storage pressure, or when you uninstall the app. |

Nothing in this table is uploaded. None of it is encrypted with a separate password — it relies on the same hardware-backed device encryption iOS provides whenever your device is locked.

---

## 7. Network behavior of the app

CleanerZX does not contact a ZXApps-owned server — there isn't one. Outbound network activity is limited to the following:

- **Firebase Analytics (Google)** receives the anonymous, aggregated usage analytics described in [§3.7](#37-anonymous-usage-analytics). This is the only third-party, non-Apple destination the app sends data to, and it never receives your photos, videos, contacts, files, or library contents.
- **StoreKit** contacts Apple's App Store servers when you view subscription options, purchase, restore, or when your entitlement is refreshed. This is operated by Apple, not by us.
- **MapKit** loads map tiles from Apple's mapping infrastructure when you open the Places map.
- **CLGeocoder** (Apple's reverse-geocoder) may turn a coordinate into a human-readable place name when the app needs to label a cluster on the Places map. This call goes to Apple, not to us.

The Apple-operated calls are subject to Apple's own privacy policy (<https://www.apple.com/legal/privacy/>); the Firebase calls are subject to Google's (<https://policies.google.com/privacy>).

Apart from the anonymous analytics above, the app makes **no** advertising requests, attribution callbacks, or webhook calls to any third party, and sends no personal content anywhere.

---

## 8. Notifications, widgets, and Live Activities

If you turn on reminders, CleanerZX schedules **local notifications** through iOS. These notifications are produced by your device and never go through a push server. The app does not register for remote push notifications and never transmits an APNs device token.

CleanerZX also provides a **home-screen widget** and a **Live Activity** that appears on the Lock Screen and in the Dynamic Island while a scan or compression is in progress. Both are rendered entirely on the device using Apple's WidgetKit and ActivityKit frameworks.

You can adjust notification preferences in **Settings → Notifications → CleanerZX**, and widget presence by editing your Home Screen.

---

## 9. Subscriptions

CleanerZX is free to install. Some advanced features are unlocked through an optional subscription called **CleanerZX Pro**, sold by Apple via the App Store and processed using Apple's StoreKit 2 framework.

What this means in practice:

- Apple — not us — sees your payment method, billing address, and Apple ID.
- The only thing the app receives from StoreKit is a signed entitlement saying "this Apple ID is currently entitled to CleanerZX Pro." No personal identifiers travel along with that signal.
- Auto-renewal, cancellation, refunds, and family sharing are governed by Apple's standard subscription policies. You can manage or cancel a subscription at any time from **Settings → [Your Name] → Subscriptions** on your device.

Apple's processing of your purchase is described in Apple's privacy policy and in the Apple Media Services Terms.

---

## 10. Apple frameworks the app relies on

For transparency, here is the full list of Apple-provided system frameworks CleanerZX uses for anything user-facing:

- **PhotoKit** — reading the photo library.
- **Contacts** — reading the contacts database.
- **Core Location** — only when you tap the current-location button on the Places map.
- **MapKit** — drawing the Places map.
- **CLGeocoder** — turning coordinates into place names for the Places map.
- **Core ML** — running the on-device face-embedding model.
- **Vision** — on-device face landmark detection and quality scoring used by the face-grouping feature.
- **CryptoKit** — computing local SHA-256 hashes of photo bytes to detect exact duplicates.
- **StoreKit 2** — subscriptions.
- **WidgetKit** and **ActivityKit** — the home-screen widget and Live Activity.
- **AppIntents** — letting the widget launch a scan when you tap it.
- **UserNotifications** — scheduling local reminders.
- **SwiftData** and **Foundation** — local storage and standard utilities.

All of the frameworks listed above are provided by Apple. The app's only **third-party** SDK is Google's Firebase Analytics, used solely for the anonymous usage analytics described in [§3.7](#37-anonymous-usage-analytics) and [§7](#7-network-behavior-of-the-app).

---

## 11. Security posture

Because your personal content never leaves the device (the only outbound data is the anonymous analytics in [§3.7](#37-anonymous-usage-analytics)), the security model is essentially the security model of iOS itself, plus a small amount of care on our side:

- **App sandbox.** iOS prevents other apps from reading CleanerZX's local container.
- **Hardware-backed encryption.** When your device is locked, on-device storage is encrypted using the Secure Enclave's hardware keys.
- **Security-scoped bookmarks.** Folders you grant via the document picker are accessed only inside Apple's security-scoped resource APIs and are not retained across uninstalls.
- **Least-privilege framework use.** Each Apple permission is requested only at the moment the matching feature is invoked.
- **No outbound transport of your content.** The app never opens a connection to a ZXApps server (there isn't one), and your photos, videos, contacts, files, and scan results never leave the device. The only outbound data is the anonymous usage analytics in [§3.7](#37-anonymous-usage-analytics), which contains no personal content and is handled by Google under its own security and privacy commitments.

No technical system is ever provably perfect. But the smaller the surface area, the smaller the risk; and CleanerZX's surface area is intentionally tiny.

---

## 12. Use by minors

CleanerZX is a general-audience utility, not directed at children. We do not knowingly collect personal data from children — and, because the app collects no data that identifies anyone (only anonymous usage analytics, see [§3.7](#37-anonymous-usage-analytics)), this restriction is largely automatic. In jurisdictions where the digital age of consent is 16 (such as much of the EU), users under that age should use the app with a parent's or guardian's involvement.

If you believe a minor has used CleanerZX in a way that raises a privacy concern, please write to the address in [§19](#19-how-to-reach-us).

---

## 13. Controls you always have through iOS

You don't need to file a written request to exercise control over what CleanerZX can see. iOS gives you everything you need:

- Open **Settings → Privacy & Security → Photos / Contacts / Location** to grant, narrow, or revoke each permission independently.
- Open **Settings → Notifications → CleanerZX** to mute notifications.
- Open **Settings → [Your Name] → Subscriptions** to cancel CleanerZX Pro.
- Long-press the CleanerZX icon → **Remove App → Delete App** to remove the app and its local data from the device. Deleting the app also stops all further anonymous analytics collection described in [§3.7](#37-anonymous-usage-analytics).
- Use **Settings → General → iPhone Storage → CleanerZX → Offload App** to keep your saved state but free up the binary.

These controls are immediate, do not require contacting us, and are honored by iOS itself.

---

## 14. Notice for users in the EU, the EEA, the UK, and Switzerland

This section provides additional information required by the EU General Data Protection Regulation (GDPR), the UK GDPR, and the Swiss FADP.

**Controller and processor.** ZXApps, the publisher of CleanerZX, acts as the controller of the limited personal data processed in connection with the app. For the anonymous usage analytics described in [§3.7](#37-anonymous-usage-analytics), **Google** acts as our processor through the Firebase Analytics service.

**Legal bases.** Where processing performed in connection with the app could fall under GDPR, the basis is:

- **Article 6(1)(b)** — performance of a contract — when you ask the app to do something for you (run a scan, compress a video, find duplicate contacts).
- **Article 6(1)(a)** — consent — for permissions you grant through iOS. You can withdraw consent at any time by toggling the permission off.
- **Article 6(1)(f)** — legitimate interests — for small operational items like remembering your preferences between launches, and for the anonymous, aggregated Firebase Analytics usage statistics we use to understand and improve the app. This analytics data is not linked to your identity and is not used for advertising or cross-app tracking.

**Your rights.** Subject to GDPR conditions, you have the rights of:

- access (Art. 15),
- rectification (Art. 16),
- erasure (Art. 17),
- restriction (Art. 18),
- portability (Art. 20),
- objection (Art. 21),
- not being subject to solely automated decision-making (Art. 22), which the app does not perform, and
- withdrawal of consent (Art. 7(3)).

In practice, because CleanerZX does not retain personal data on a server, the most effective ways to exercise these rights are also the fastest: revoke an iOS permission, clear scan history inside the app, or delete the app from your device. If you would prefer a written response from us about a specific GDPR right, write to the address in [§19](#19-how-to-reach-us) and we will respond within one month (extendable by two months for complex matters, with notice to you).

**Complaints.** You have the right to lodge a complaint with the supervisory authority in your country. Useful links:

- European Data Protection Board: <https://edpb.europa.eu/>
- UK Information Commissioner's Office: <https://ico.org.uk/>
- Swiss FDPIC: <https://www.edoeb.admin.ch/>

---

## 15. Notice for users in California

This section addresses additional disclosures expected under the California Consumer Privacy Act (as amended by the California Privacy Rights Act), referred to here as the **CCPA**.

**Categories collected, sold, or shared.** The only personal information transmitted off the device is the anonymous usage analytics described in [§3.7](#37-anonymous-usage-analytics) — limited to identifiers (a per-install ID / IDFV) and internet/usage activity (product-interaction events), collected for analytics purposes. CleanerZX does **not sell** and does **not share** personal information (as "sell" and "share" are defined under the CCPA), does not use it for cross-context behavioral advertising, and does not use or disclose sensitive personal information in any way that triggers the CCPA's "limit the use" right.

**Your CCPA rights.** You have the right to know, to delete, to correct, to opt out of the sale or sharing of personal information, to limit the use of sensitive personal information, and to be free from retaliation for exercising those rights. Because the only data collected is anonymous analytics that is not linked to your identity, we generally cannot connect it to a specific person in order to act on an individual request — but you are still welcome to submit a request to the email in [§19](#19-how-to-reach-us), and we will respond within the timeframes California law sets.

**"Shine the Light."** We do not share personal information with third parties for their own direct-marketing purposes.

**Authorized agents.** If you wish to use an authorized agent to submit a request, please send proof of the agent's authorization to the contact email.

---

## 16. Notice for users elsewhere

If you live in a jurisdiction not specifically addressed above — for example, Brazil (LGPD), Canada (PIPEDA), Australia (Privacy Act 1988), Japan (APPI), South Korea (PIPA), or others — many of the rights described in [§14](#14-notice-for-users-in-the-eu-the-eea-the-uk-and-switzerland) and [§15](#15-notice-for-users-in-california) have analogues in your local law.

Because CleanerZX is engineered to keep your data on your phone, the iOS-level controls in [§13](#13-controls-you-always-have-through-ios) are usually the most direct way to enforce those rights. For a written response from us, write to the email in [§19](#19-how-to-reach-us).

---

## 17. Cross-border transfers

Your personal content never leaves your device, so it is never transferred internationally. The anonymous usage analytics described in [§3.7](#37-anonymous-usage-analytics) is processed by **Google** on its global infrastructure, which may include servers in the United States and other countries. Google applies appropriate safeguards for such transfers (including the European Commission's Standard Contractual Clauses where applicable), as described in Google's Privacy Policy and Firebase data-processing terms.

If you contact us by email (to ask a question, exercise a right, or report a problem), the content of that email will pass through and be retained by the email service we use to read and reply to it.

---

## 18. Updates to this document

If we change this Privacy Policy, the **"In effect from"** date at the top will move. For material changes — for example, the addition of any third-party SDK, or any change that would alter the on-device-only nature of the app — we will additionally display a notice inside the app the next time you open it.

The current version of this document is always considered the version "in effect" for new sessions. You should re-read this document occasionally; continued use of CleanerZX after a revised date is taken as acceptance of the revised text, except where local law requires explicit consent.

---

## 19. How to reach us

For any question, request, or report related to this Privacy Policy:

- **Publisher:** ZXApps
- **Email:** zxapps1993@gmail.com
- **Subject prefix that helps us route faster:** "Privacy — CleanerZX —"

We do our best to reply within the timeframes required by applicable law and to answer in plain language.

---

*If you ever find a behavior of CleanerZX that does not match a statement in this document, please tell us. We treat any such mismatch as a bug to fix, not a definition to broaden.*

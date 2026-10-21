# AirSignal

**Last updated:** 21 October 2026  
**Website:** [airsignal.dev](https://airsignal.dev)

AirSignal is a hosted commercial push notification platform. You get a dashboard, a project-scoped REST API, and official client SDKs for **Flutter**, **Android**, and **Web**. A **WordPress** plugin uses that Web SDK on a WordPress site. AirSignal runs the delivery backend. You do not install or host the platform yourself, and it is not open source.

Raw FCM (Firebase) and browser push still need token handling, credentials, and a backend your team would otherwise operate. AirSignal hosts that pipeline. Each app has a public App ID (`app_...`) for the SDKs. Sends use the dashboard, a server-only REST key (`nsk_...`), or the WordPress plugin with that same key.

The first time you create an app, choose **Android** or **Web**. You can add the other platform on that same app later. One notification can reach Android and web subscribers together.

## Official SDKs

Flutter, Android, Web, and the WordPress plugin are available. The native iOS SDK is not.

| SDK | Status | Package | Guide |
|---|---|---|---|
| Flutter | Available | `airsignal_flutter` `^0.5.0` | [Flutter SDK](https://airsignal.dev/docs/sdks/flutter) |
| Android | Available | `com.detekfit:AirSignal` `0.1.1` on Maven Central | [Android SDK](https://airsignal.dev/docs/sdks/android) |
| Web | Available | [airsignal.js](https://cdn.airsignal.dev/sdks/web/v1/airsignal.js) | [Web SDK](https://airsignal.dev/docs/sdks/web) |
| WordPress | Available | [airsignal-wp-0.3.6.zip](https://cdn.airsignal.dev/sdks/wordpress/v1/airsignal-wp-0.3.6.zip) | [WordPress](https://airsignal.dev/docs/sdks/wordpress) |
| iOS (Swift) | Coming soon | — | [iOS SDK](https://airsignal.dev/docs/sdks/ios) |

- **Flutter.** One package. Do not add `firebase_messaging`, and do not ship `google-services.json` for AirSignal. Android push works. iOS push in the Flutter SDK is not ready.
- **Android.** One library (`minSdk 21`, Kotlin package `com.airsignal`). It owns push receive. Do not add a second push SDK, and do not ship `google-services.json` for AirSignal. On Android 13 and later, forward the notification permission result into the SDK.
- **Web.** Save the site URL on the app, put `AirSignalSDK.js` at the site root, and add the head script with your App ID. Chrome, Edge, Firefox, and Safari can display the notification. A host that cannot serve `/AirSignalSDK.js` cannot receive web push. After the page loads, the SDK shows a subscribe card. The Web platform can name checkboxes on that card. They are checked by default. The visitor can clear any of them. Each name left checked is saved on that subscriber as a tag with value `true`, so a segment can target it. How many names you can add follows **Tags per subscriber** on the plan. With no names, the card has no checkboxes and subscribe saves no tags. The small icon on a notification is the Default icon URL from the Web platform. Leave it empty and the notification has no small icon. After you create a web app, the install screen has **Web** and **WordPress** tabs. WordPress sites should use the plugin instead of uploading the worker.
- **WordPress.** Install the plugin on a WordPress site at the domain root over HTTPS, such as `https://example.com`. A site in a subfolder (`https://example.com/blog/`) is not supported. Configure Web on the app first, then paste the App ID and the project API key (`nsk_...`). Each field saves when the value is valid. A green check means it was saved. A red cross means it was not saved. A gray dash means nothing is stored yet. A saved API key stays hidden. You do not add the script or upload the worker yourself. The permission banner, including those checkboxes, is the one configured on the Web platform. Optional toggles send a push to everyone on that app when a post or page is published or updated, including a first publish from another plugin. Priority is **Normal** or **High**. Drafts do not send. The notification uses the site name, the title, the link, and a featured image when one is set.
- **Mobile transport** is FCM. Upload a Google service account JSON in the dashboard. Android display is handled by the SDK.
- **Web transport** uses the site origin and the public config from `GET /v1/config/web`.

There is no React Native SDK.

## What you can do

- An AI notification writer drafts your title and message in the dashboard. It is included on every plan.
- Compose a push in the dashboard: title, body, image, launch URL, custom data, up to three Android action buttons, priority, TTL, and drafts.
- Target subscriber ids, external ids, segments (including a broadcast to everyone), or test users.
- Schedule a send. Timezone delivery is included on Starter, Pro, and Enterprise. It is not included on Free.
- Intelligent Delivery picks the most likely time to send, so more people click the notification. It uses each device’s usual active hour within about 24 hours. It is included on Pro and Enterprise only, and it is not meant for alerts that must arrive immediately.
- Manage audience: users, device subscriptions, tags, aliases, and `externalId` login and logout.
- Read delivery reports and engagement charts (delivered, failed, clicks, and click-through rate). How far back the charts go depends on your plan.
- Build segments, within your plan’s segment cap.
- Invite a team. Roles are owner, admin, developer, and viewer. Who you can invite depends on your plan.

Push is the live channel. An email address can be stored on a subscriber. AirSignal does not send email, SMS, or in-app messages. Audience CSV import is not available.

## Plans

Prices in USD are on the [pricing page](https://airsignal.dev/pricing). The table is what each plan includes.

| | Free | Starter | Pro | Enterprise |
|---|---|---|---|---|
| Price | $0 | [Pricing](https://airsignal.dev/pricing) | [Pricing](https://airsignal.dev/pricing) | Custom — [sales@airsignal.dev](mailto:sales@airsignal.dev) |
| MAU (whole organization) | 5,000 | 20,000 included | 100,000 included | 1,000,000+ |
| Above that MAU | Sends stop | $0.001 per extra MAU | $0.0004 per extra MAU | Written agreement |
| Apps | 1 | 3 | 10 | Custom |
| Members | 1 (owner) | 5 | 10 | Custom |
| Active segments per app | 3 | 10 | 40 | Custom |
| Tags per subscriber | 5 | 15 | 50 | Custom |
| Analytics history | 14 days | 60 days | 180 days | 730 days, or custom |
| Push sends | Unlimited inside the MAU cap | Unlimited; extra MAU is billed | Unlimited; extra MAU is billed | Custom |
| Timezone delivery | No | Yes | Yes | Yes |
| Intelligent Delivery | No | No | Yes | Yes |
| Send API rate (`nsk_...`) | 5 requests/second | 25 requests/second | 100 requests/second | 500+ requests/second |

MAU means monthly active users across every app in the organization, measured from active devices. It is not a count of rows in your user list.

Free does not bill overage. On Starter and Pro, sends continue past the included MAU and the extra is billed on the next settlement. Unpaid overage can pause sends at the included MAU. The dashboard stays available.

SDK collect calls (`X-AirSignal-App-Id`) are limited to 100 requests per minute per IP on every plan.

## Credentials

| Credential | Use it in | Purpose |
|---|---|---|
| App ID `app_...` | Flutter, Android, Web, and the WordPress plugin | Collect only. Header: `X-AirSignal-App-Id` |
| REST key `nsk_...` | Your server, the dashboard, or the WordPress plugin | Send only. Header: `Authorization: Key nsk_...` |

Keep `nsk_...` off phones and websites. [API keys](https://airsignal.dev/docs/guides/api-keys) explains both credentials.

## Who it is for

AirSignal fits teams building a **Flutter** app, a **native Android** app, a **website** that can install the Web SDK, or a **WordPress** site at the domain root. Typical products are ones that need real-time alerts: habit trackers, delivery and logistics, rideshare and chat, fintech and crypto, marketing campaigns, media and news, and sites that want browser push.

You need an app or a site that can run one of the published SDKs. A native iOS-only app is not covered yet, because that SDK is still coming soon. React Native is not supported.

## Support and commercial terms

There is no self-hosted edition.

Enterprise capacity is arranged with [sales@airsignal.dev](mailto:sales@airsignal.dev) and a written order form. The [Enterprise support commitment](https://airsignal.dev/enterprise-sla) applies only to those customers. It does not promise an uptime percentage or automatic service credits.

Product questions: [support@airsignal.dev](mailto:support@airsignal.dev).

## Where to go next

| Topic | Link |
|---|---|
| Website | [airsignal.dev](https://airsignal.dev) |
| Documentation | [airsignal.dev/docs](https://airsignal.dev/docs) |
| Quickstart | [Send your first push](https://airsignal.dev/docs/getting-started/quickstart) |
| Android setup | [Android](https://airsignal.dev/docs/guides/android) |
| Web setup | [Web](https://airsignal.dev/docs/guides/web) |
| WordPress | [WordPress](https://airsignal.dev/docs/sdks/wordpress) |
| Google service account | [Service account setup](https://airsignal.dev/docs/guides/service-account) |
| API keys | [App ID and REST key](https://airsignal.dev/docs/guides/api-keys) |
| API reference | [airsignal.dev/docs/api](https://airsignal.dev/docs/api) |
| OpenAPI | [api.airsignal.dev/openapi.json](https://api.airsignal.dev/openapi.json) |
| Flutter SDK | [airsignal.dev/docs/sdks/flutter](https://airsignal.dev/docs/sdks/flutter) |
| Android SDK | [airsignal.dev/docs/sdks/android](https://airsignal.dev/docs/sdks/android) |
| Web SDK | [airsignal.dev/docs/sdks/web](https://airsignal.dev/docs/sdks/web) |
| WordPress plugin | [airsignal.dev/docs/sdks/wordpress](https://airsignal.dev/docs/sdks/wordpress) |
| iOS SDK | [airsignal.dev/docs/sdks/ios](https://airsignal.dev/docs/sdks/ios) |
| Privacy and security | [airsignal.dev/docs/privacy-and-security](https://airsignal.dev/docs/privacy-and-security) |
| Pricing | [airsignal.dev/pricing](https://airsignal.dev/pricing) |
| Sign in | [airsignal.dev/auth](https://airsignal.dev/auth) |
| Dashboard | [airsignal.dev/dashboard](https://airsignal.dev/dashboard) |
| API | [api.airsignal.dev](https://api.airsignal.dev) |
| Terms | [airsignal.dev/terms](https://airsignal.dev/terms) |
| Privacy | [airsignal.dev/privacy](https://airsignal.dev/privacy) |
| Cookies | [airsignal.dev/cookies](https://airsignal.dev/cookies) |
| Acceptable use | [airsignal.dev/aup](https://airsignal.dev/aup) |
| DPA | [airsignal.dev/dpa](https://airsignal.dev/dpa) |
| Subprocessors | [airsignal.dev/subprocessors](https://airsignal.dev/subprocessors) |
| Refunds | [airsignal.dev/refunds](https://airsignal.dev/refunds) |
| Enterprise support | [airsignal.dev/enterprise-sla](https://airsignal.dev/enterprise-sla) |
| All policies | [airsignal.dev/legal](https://airsignal.dev/legal) |

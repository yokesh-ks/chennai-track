# Product Requirements Document: Chennai Local Train Web App

**Document status:** Product baseline for discovery and MVP planning  
**Prepared by:** Manus AI  
**Date:** 18 September 2026  
**Product type:** Responsive web application and Progressive Web App (PWA)  
**Primary languages:** English and Tamil

## 1. Executive summary

Chennai’s suburban railway is a high-value component of a larger multimodal network that also includes the Mass Rapid Transit System (MRTS), Chennai Metro, Metropolitan Transport Corporation buses, and walking connections. Chennai’s official planning documents describe suburban corridors toward Gummidipoondi, Arakkonam, and Chengalpattu/Tambaram, while Chennai Metro’s interchange information shows that many rail-to-metro connections depend on short but important walking paths. [1] [2] [3]

The proposed product is a **trustworthy Chennai local train companion**. It will help a rider answer four questions quickly: **Which station should I use? Which train or route should I take? When should I leave or arrive? What should I do if service information changes?**

The MVP will provide a fast, bilingual route and timetable experience, station pages, saved journeys, service notices, station-access information, and an offline-capable schedule cache. It will show the data’s source and freshness. It will not pretend to offer live train movement, ticketing, or guaranteed platform information unless an authorised source is available and operationally reliable. Indian Railways’ public services already cover train enquiry and ticketing, and the former UTS web service now directs users to RailOne. [4] [5]

The product’s differentiator is not merely another timetable. It is a **Chennai-specific, rider-first information layer** that combines railway schedules with bilingual search, practical station context, multimodal interchange guidance, low-connectivity behaviour, accessibility, and transparent data provenance.

## 2. Problem statement

A rider planning a local-train journey currently has to combine several sources. Official timetable pages may be difficult to scan on a phone. General map products may not explain suburban train stopping patterns or station entrances clearly. Railway status services are valuable but are not designed specifically around Chennai’s daily local journeys. Ticketing is a separate workflow. Service changes may appear in notices that are hard to discover.

This fragmentation creates five user problems:

1. **Route uncertainty:** riders may know the destination but not the correct corridor, direction, interchange, or stopping service.
2. **Timetable uncertainty:** timetable documents and dynamic train-status services serve different purposes, and users may confuse scheduled time with live prediction.
3. **Station friction:** the last 200–600 metres between rail, metro, bus, and street access can determine whether a journey is practical. Chennai Metro documents multiple such interchange distances. [3]
4. **Language and connectivity barriers:** Tamil-first users and riders with slow or intermittent mobile data need a simpler, cached experience.
5. **Trust and safety concerns:** the product must distinguish official information, verified partner data, community reports, and stale or unverified content.

## 3. Goals and non-goals

### Goals

The product will:

- Let a new or returning rider find a train journey between two stations in under 30 seconds on a typical mobile device.
- Support English and Tamil from the first release, including station-name search across Tamil and English spellings.
- Present scheduled journeys with clear date, weekday, direction, stopping-pattern, and source metadata.
- Provide station pages with facilities, interchange links, walking guidance, accessibility notes, and emergency/help links where verified.
- Work acceptably on slow networks and retain the last successful schedule and saved journeys offline.
- Give users a clear path to official live status and ticketing services without falsely representing a third-party or unofficial integration as official.
- Create an extensible data model for later GTFS or GTFS-Realtime feeds. GTFS-Realtime supports service alerts, trip updates, and vehicle positions when a transit agency publishes such data. [6]

### Non-goals for MVP

The MVP will not:

- Sell tickets, store payment credentials, or reproduce railway ticketing flows.
- Guarantee live train location, platform assignment, coach position, crowding, or cancellation status without an authorised, monitored feed.
- Replace official railway enquiry, emergency, grievance, or ticketing channels.
- Provide door-to-door routing for every mode in Chennai on day one.
- Collect continuous location history or require account creation for basic search.
- Publish unverified incident reports as facts.

## 4. Product principles

**Trust before completeness.** A smaller set of accurate, dated information is better than a large set of ambiguous information.

**Fast path first.** The home screen should help the rider plan a trip before presenting secondary content.

**Bilingual by design.** Tamil is not a translation layer added after English. Search, labels, station aliases, errors, and help content must be designed for both languages.

**Scheduled is not live.** Every result must expose whether it is scheduled, estimated, reported, or stale.

**Progressive enhancement.** The core journey must work without location permission, account creation, high-end hardware, or continuous network access.

## 5. Target users and personas

| Persona | Context | Main need | MVP response |
|---|---|---|---|
| Daily commuter | Travels at a regular time between a home and work/college station | Reliable departure and arrival options | Saved journeys, next departures, schedule cache, alerts |
| Student or first-time rider | May know only landmarks or destination names | Understand corridor, direction, interchange, and station sequence | Plain-language route explanation, bilingual station search, map/list views |
| Tamil-first rider | Prefers Tamil labels and help | Search and understand without switching language | Tamil UI, aliases, translated system messages, readable typography |
| Woman or late-evening rider | Needs confidence about station access and help | Know station location, interchange, help, and service state | Verified facilities, walking links, emergency/help links, freshness labels |
| Older adult or low-vision rider | May need large text and simple interaction | Read and operate the app easily | WCAG 2.2 AA target, large type control, high contrast, keyboard and screen-reader support |
| Visitor | Does not know Chennai’s station naming conventions | Convert a destination into a practical local-train trip | Landmark aliases, station details, multimodal connections, English/Tamil toggle |
| Operations/content administrator | Maintains schedules, notices, station content, and provenance | Update information safely and auditably | Admin console, review workflow, versioning, source and expiry fields |

These personas are hypotheses. Before launch, validate them through at least 12 rider interviews, two station intercept sessions, and usability testing with Tamil-first and accessibility-focused participants.

## 6. Core user journeys

### Journey A: Find a train between two stations

1. The rider opens the home screen.
2. The rider enters origin and destination using English, Tamil, transliteration, or a landmark alias.
3. The app suggests canonical station names and shows the corridor or line.
4. The rider selects date and approximate departure or arrival time.
5. Results show candidate services with departure, arrival, duration, intermediate stops, service type, and data timestamp.
6. The rider opens a result to see the station sequence and walking/interchange context.
7. The rider saves the journey or shares a link.

**Acceptance criteria:** For a known station pair in the supported dataset, 90% of test users can reach a result within three interaction steps after selecting origin and destination. Results must display the timetable version and last-updated time.

### Journey B: Check the next departure at a station

1. The rider searches for or opens a saved station.
2. The app shows departures by direction and destination.
3. The app distinguishes scheduled departure from live estimate.
4. If no live feed is available, it says “Scheduled time” and provides the official status link where appropriate.

**Acceptance criteria:** No result may use words such as “on time,” “arriving,” or “live” unless an authorised live or verified operational source supports the claim.

### Journey C: Plan a multimodal interchange

1. The rider opens a station page.
2. The page shows connected metro, MRTS, bus, or landmark links only when verified.
3. The rider sees walking distance or a qualitative walking note, the source date, and an external map link.
4. The app warns that walking time and street conditions may change.

### Journey D: Recover from a service disruption

1. The rider sees a banner with the affected corridor, validity window, severity, source, and time of last verification.
2. The app explains whether the notice is an official railway notice, partner feed, or editorially verified report.
3. The rider can view alternate scheduled options or open the official status service.
4. Expired notices disappear from the default view but remain in an audit log.

### Journey E: Use the app with poor connectivity

1. The rider opens the app after a previous successful visit.
2. The app loads the shell, saved journeys, station search index, and last cached schedules.
3. It clearly labels cached data and shows when it was captured.
4. If a requested update is unavailable, the app offers retry and official links rather than a blank screen.

## 7. MVP feature requirements

| ID | Requirement | Priority | Acceptance criteria |
|---|---|---:|---|
| FR-01 | Origin/destination journey search | P0 | Supports canonical stations, Tamil labels, transliteration, aliases, swap, and empty/error states. |
| FR-02 | Timetable results | P0 | Shows departure, arrival, duration, stops, direction, service date, and schedule version. |
| FR-03 | Station pages | P0 | Includes station name variants, corridor, served destinations, timetable link, map link, and verified facilities. |
| FR-04 | Bilingual interface | P0 | All core navigation, search, errors, and result labels available in English and Tamil. |
| FR-05 | Data provenance | P0 | Every timetable, notice, and facility block shows source class and last-updated time. |
| FR-06 | Saved journeys and stations | P0 | Works without account; persists locally and can be deleted by the user. |
| FR-07 | Offline/PWA support | P0 | App shell, station index, saved journeys, and last successful schedule cache load offline. |
| FR-08 | Official service links | P0 | Links to Southern Railway timetable pages, NTES, RailOne, and relevant official help pages. |
| FR-09 | Notices and disruptions | P1 | Admin can publish, schedule, expire, localise, and audit a notice. |
| FR-10 | Multimodal interchange | P1 | Shows verified connections and walking context at priority stations. |
| FR-11 | Accessibility controls | P0 | Keyboard operation, focus visibility, screen-reader labels, contrast, text resize, and reduced-motion support meet the WCAG 2.2 AA target. [7] |
| FR-12 | Shareable journey URL | P1 | A shared URL restores origin, destination, date, and selected options without exposing personal data. |
| FR-13 | Feedback/report issue | P1 | User can report stale or incorrect information without creating an account. |
| FR-14 | Analytics | P1 | Captures privacy-preserving funnel events and data-quality outcomes. |
| FR-15 | Live status adapter | P2 | Feature-flagged adapter supports an authorised feed; disabled by default until access and monitoring are approved. |
| FR-16 | Ticketing deep link | P2 | Opens the current official ticketing path; no credential or payment data is handled by this app. |

## 8. Information architecture and UX

The top-level navigation should contain **Plan**, **Stations**, **Notices**, and **Saved**. The home screen should contain a prominent “From / To” planner, a “Next departures” entry point, saved journeys, and a compact status area.

A search result should use a dense but readable card. The first line should answer “which train and when.” The second line should show destination, number of stops, and duration. A metadata row should show “Scheduled,” “Live,” “Cached,” or “Reported,” followed by timestamp and source.

The app should support two display modes:

- **Simple list mode:** optimised for commuters and low-end devices.
- **Map/context mode:** shows the route and station sequence without making the map the only way to understand the journey.

Tamil requirements include Unicode-safe rendering, careful line wrapping, station aliases, transliteration search, and human review of railway terminology. Avoid translating official names in a way that makes them harder to recognise at stations; display the canonical English name and Tamil equivalent together where useful.

## 9. Data and source-of-truth strategy

The initial station and timetable dataset should be assembled from current Southern Railway publications and reviewed against official notices. Southern Railway maintains a dedicated Chennai suburban timetable page and publishes timetable documents; the page itself records its review date, so the product must store publication and retrieval dates rather than treating a URL as permanently current. [1]

The proposed source hierarchy is:

1. **Official railway publication or authorised feed:** source of truth for timetable and operational claims.
2. **Official Chennai Metro, CMDA, or transport-agency page:** source for interchange, planning, and facility context within that agency’s remit.
3. **Licensed or contractually authorised data provider:** source only within the permitted product and redistribution rights.
4. **OpenStreetMap:** source for map geometry and community-maintained points of interest, with required attribution and ODbL obligations. [8]
5. **Editorially verified secondary source:** useful for a temporary notice, but must be labelled and expire automatically.
6. **User report:** a lead for review, never an unverified operational fact.

Each data entity should include `source_url`, `source_type`, `retrieved_at`, `published_at`, `valid_from`, `valid_until`, `review_status`, `confidence`, and `locale_status`.

### Core entities

- `station`: canonical name, Tamil name, aliases, coordinates, corridor, served services, accessibility attributes, facilities, map identifiers.
- `route`: corridor, direction, service class, ordered station sequence.
- `trip`: service date pattern, departure/arrival times, route, train/service identifier if available.
- `timetable_version`: publication source, effective dates, checksum, import status.
- `notice`: affected corridor/station/trip, severity, cause, message in English and Tamil, source, validity window.
- `interchange`: rail-to-metro/MRTS/bus connection, walking distance or note, verification date, source.
- `feedback`: category, text, optional screenshot, language, created time, moderation state.

## 10. Real-time and disruption strategy

The MVP should launch with scheduled data and official links. A real-time layer should be designed as a replaceable adapter, not hard-coded into the UI. If an authorised GTFS-Realtime or railway feed becomes available, map its entities to:

- `TripUpdate` for predicted arrivals and departures.
- `VehiclePosition` for location, only where the feed’s semantics and accuracy justify displaying it.
- `Alert` for cancellations, station closures, delays, or changed service.

GTFS-Realtime documentation explicitly describes these use cases and warns that some fields are optional or experimental. [6] The app should therefore display feed timestamp, prediction age, confidence or freshness, and an “information may change” notice.

If no official feed is available, the app may show manually curated notices with a visible “last verified” timestamp. It must not infer train movement from user location, crowdsourced pings, or scraping that violates a service’s terms.

## 11. Technical architecture

### Recommended stack

- **Frontend:** React or Next.js with TypeScript, responsive design system, service worker, IndexedDB cache, and server-rendered or statically rendered public pages where useful.
- **Backend:** TypeScript/Node.js API with a relational database such as PostgreSQL and PostGIS for stations, corridors, and geometry.
- **Search:** PostgreSQL trigram/full-text search for MVP; add a dedicated search engine only if scale or Tamil relevance requires it.
- **Admin:** Role-based content and data-review console with draft, review, publish, expire, and rollback states.
- **Maps:** Licensed provider or self-hosted tiles. If using OpenStreetMap data, display required attribution and respect API, tile, and Nominatim usage policies. [8]
- **Observability:** structured logs, uptime checks, import health, stale-data alerts, error tracking, and audit logs.
- **Deployment:** CDN for static assets, managed application runtime, managed database, encrypted backups, and separate staging/production environments.

### Data pipeline

1. Ingest official timetable files and notices into a staging area.
2. Validate station identifiers, times, effective dates, duplicate services, and route order.
3. Run human review for material changes and Tamil text.
4. Publish a versioned dataset.
5. Rebuild search index and offline package.
6. Expose freshness and version metadata through the API.
7. Roll back if validation or post-publish monitoring fails.

### Performance targets

- First meaningful content under 2.5 seconds on a mid-range mobile device over a constrained 4G connection.
- Station search suggestions visible within 300 ms after local index lookup.
- P95 journey-search API response under 800 ms for the supported network.
- Core planner usable at 320 CSS pixels wide.
- No blocking dependency on map tiles for timetable search.

## 12. Accessibility, privacy, and security

Target **WCAG 2.2 AA**. WCAG 2.2 defines four principles—perceivable, operable, understandable, and robust—and provides testable success criteria for conformance. [7] Product acceptance must include keyboard-only testing, screen-reader testing, text resizing to 200%, visible focus, sufficient contrast, accessible names, non-colour status indicators, touch-target sizing, reduced motion, and clear error recovery.

Privacy requirements:

- Basic search must work without an account.
- Do not collect precise location unless the user explicitly enables a feature that needs it.
- Do not store journey searches tied to an identity by default.
- Provide a plain-language privacy notice in English and Tamil.
- Retain feedback only as long as needed for support, moderation, and quality analysis.
- Provide deletion and consent controls for optional analytics or notifications.
- Avoid third-party advertising trackers in the core journey.

Security requirements:

- Enforce HTTPS, secure headers, content-security policy, and dependency scanning.
- Use role-based access control and multi-factor authentication for administrators.
- Protect import and admin endpoints against replay, injection, CSRF, and bulk abuse.
- Encrypt secrets and personal data at rest where applicable.
- Log administrative changes with actor, timestamp, old value, new value, and reason.
- Publish a vulnerability-reporting contact and incident-response procedure.

## 13. Analytics and success metrics

The north-star outcome is **successful journey planning**, defined as a user finding a valid route and viewing its details without encountering a data-quality or comprehension failure.

Recommended metrics:

- Search-to-result success rate: target 85% in beta, 92% after two release cycles.
- Median time from opening planner to route details: under 30 seconds for known stations.
- Schedule freshness compliance: at least 99% of displayed timetable records have a valid source and effective date.
- Stale-data incident rate: fewer than 1% of sampled result sessions show expired or unlabelled data.
- Repeat use: 30-day return rate among opted-in or anonymous cohort measured without invasive tracking.
- Saved-journey usage: percentage of returning users who use a saved route.
- Offline usefulness: percentage of offline sessions that successfully load saved content.
- Accessibility defect closure: 100% of P0/P1 accessibility defects closed before public launch.
- Content quality: median time to review a submitted issue and percentage resolved.

Analytics should be event-based, minimised, and documented. Do not record free-text journey content or precise location unless separately justified and consented.

## 14. Launch and operating plan

### Phase 0: Discovery and data validation, 3–4 weeks

Confirm the supported station list, acquire or validate the current timetable, inspect official access terms, test Tamil terminology, and interview riders. Produce a data dictionary, content style guide, and source register.

### Phase 1: MVP beta, 6–8 weeks

Ship journey search, timetable results, station pages, bilingual UI, saved journeys, PWA caching, source metadata, accessibility baseline, and official links. Launch to a small Chennai rider cohort and monitor search failures and stale data.

### Phase 2: Public launch, 4–6 weeks

Add notices, priority interchange information, feedback moderation, shareable routes, richer station facilities, and operational dashboards. Establish a weekly timetable and notice review process.

### Phase 3: Live and multimodal expansion

Pursue authorised railway or agency feeds. Add a live adapter, disruption alerts, more MTC/Metro/MRTS connectivity, and optional push notifications. Ticketing should remain an official deep link until a formal integration and compliance review is complete.

## 15. Risks and mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Timetable changes faster than the app can update | Riders miss trains | Versioned imports, source timestamps, expiry rules, official links, stale-data alerts |
| No stable public live feed | False expectations | Scheduled/live distinction, feature flag, no inferred location, official deep link |
| Unauthorised scraping or redistribution | Legal and operational exposure | Obtain written permissions; use official pages and licensed feeds; retain source terms |
| Conflicting station names and transliterations | Search failures | Canonical IDs, alias table, Tamil review, landmark aliases, analytics on zero-result searches |
| Map-provider cost or usage limits | Unexpected operating cost | Abstract map provider; cache only where permitted; evaluate licensed/self-hosted options |
| Safety or disruption notice is wrong | User harm and reputational damage | Source classification, human review, validity window, audit trail, clear disclaimer |
| Low-end devices or weak connectivity | App unusable in real conditions | Small bundles, local search index, offline cache, no map dependency for core planner |
| Accessibility regressions | Exclusion and compliance risk | Automated checks plus manual testing with assistive technology in every release |

## 16. Open questions requiring decisions

1. Which exact suburban and MRTS services are in the launch boundary, and how will the product handle services that share station names or corridors?
2. Can Southern Railway or CRIS provide a licence, feed, or permitted interface for schedule and real-time data?
3. Is the product a private information service, an agency-backed service, or a commercial product? This affects branding, notices, and data rights.
4. Which map and geocoding provider is affordable, reliable, and permitted for expected traffic?
5. Which facilities—lifts, accessible toilets, footbridges, entrances, help points, parking, and bus stops—can be verified station by station?
6. What is the minimum Tamil terminology set, and who owns linguistic review after launch?
7. Which user feedback categories require direct escalation to a railway or transport agency?
8. What notification channels are acceptable, and what consent and retention model will be used?

## 17. Resource inventory

| Resource | Use in product | Status or caution |
|---|---|---|
| Southern Railway Chennai suburban timetable page [1] | Official timetable discovery and publication links | Treat publication date and effective period as data; do not assume page freshness. |
| Southern Railway revised timetable PDFs | Import and review schedule data | Confirm current version and redistribution rights before republishing content. |
| CMDA Comprehensive Mobility Plan [2] | Corridor, fare-history, multimodal, and planning context | Planning document; do not present historical values as current fares or operations. |
| Chennai Metro multimodal integration [3] | Verified interchange context and walking distances | Last-updated date is material; re-verify before relying on distances. |
| National Train Enquiry System [4] | Official train enquiry and status deep link | Dynamic data; do not copy live claims without authorised access. |
| RailOne / current official ticketing path [5] | Ticketing and railway service deep link | Former UTS website states that web service was discontinued and directs users to RailOne. |
| CRIS API developer portal | Potential authorised railway API integration | Access, licensing, quotas, and redistribution must be confirmed contractually. |
| GTFS static and GTFS-Realtime [6] | Internal canonical format and future feed adapter | A specification, not proof that Chennai data is publicly available. |
| WCAG 2.2 [7] | Accessibility requirements and test criteria | Target AA; combine automated and human testing. |
| OpenStreetMap copyright and licence [8] | Map data and station geometry | Attribution, ODbL, API, tile, and geocoding policies apply. |
| Chennai Metro project brief [9] | Historical network and corridor context | Use as contextual reference; reconcile older descriptions with current official sources. |
| Google Play/App Store listings for NTES and RailOne | Competitor and official product capability review | Store descriptions may change; capture review date and avoid treating user reviews as authoritative operations data. |
| Moovit and general map products | Benchmark route discovery and multimodal UX | Do not copy content; use only for comparative product research. |
| Tamil localization QA checklist | Language quality and inclusive UX | Maintain a glossary and review all operational terminology with native speakers. |

## 18. Definition of done for MVP

The MVP is ready for public beta when the supported station and timetable dataset has an owner, effective date, source URL, and rollback version; the four core journeys pass moderated usability testing; Tamil and English core flows are reviewed; PWA caching works on a slow network and offline; accessibility testing meets the agreed WCAG 2.2 AA baseline; every operational claim has provenance; privacy and security reviews are complete; and the product clearly routes riders to official railway services for live status and ticketing.

## References

[1]: https://sr.indianrailways.gov.in/view_section.jsp?lang=0&id=0,2,1429 "Southern Railway Chennai Suburban Train Time Table"

[2]: https://www.cmdachennai.gov.in/pdfs/ComprehensiveMobilityPlan-CMA.pdf "Comprehensive Mobility Plan for Chennai Metropolitan Area"

[3]: https://chennaimetrorail.org/multimodal-integration/ "Chennai Metro Rail Limited Multimodal Integration"

[4]: https://enquiry.indianrail.gov.in/ "National Train Enquiry System"

[5]: https://www.utsonmobile.indianrail.gov.in/ "UTS on Mobile service notice and RailOne transition"

[6]: https://gtfs.org/documentation/realtime/reference/ "GTFS Realtime Reference"

[7]: https://www.w3.org/TR/WCAG22/ "Web Content Accessibility Guidelines 2.2"

[8]: https://www.openstreetmap.org/copyright "OpenStreetMap Copyright and Licence"

[9]: https://chennaimetrorail.org/wp-content/uploads/2023/11/project_brief_updated_aug081.pdf "Chennai Metro Rail Project Brief"

# Canonical Architecture v1

Status: SOURCE OF TRUTH
Mode: DEOS ON
Rule: PROBE FIRST → MUTATE SECOND → VERIFY ALWAYS

## 1. Product goal
Build a premium, lightweight neurologist website for Ruhiyyə İbrahimli that combines:
- personal medical brand
- SEO/GEO traffic acquisition
- symptom/service/condition detail pages
- editorial blog
- fast WhatsApp conversion
- fully manageable content through Django Admin
- resilient modular frontend

The previous onurga.net SEO learnings should inform content architecture, but this repository is a clean implementation.

## 2. Non-negotiable architecture
Frontend and backend are separated by a typed contract.

Frontend:
- Next.js
- TypeScript
- Server Components first
- mock-data implementation before Django integration
- components never call raw backend URLs directly
- data access only through repository/adaptor layer
- every major homepage section isolated

Backend:
- Django
- Django REST Framework
- PostgreSQL from day one
- Django Admin is the CMS
- fixed section/content schemas, not an unrestricted page builder
- media validation by slot
- SEO fields and relations are native models

## 3. Failure isolation
A single failing section must never take down the whole page.

Homepage logical sections:
1. Hero
2. Symptoms
3. About
4. Services
5. WhyUs
6. BodyMap
7. Articles
8. FAQ
9. Reviews
10. ContactCTA
11. Footer

Each data-driven section must have:
- its own repository method
- its own loading/fallback strategy where needed
- schema validation
- graceful empty-state behavior

Rule:
Articles FAIL ≠ Homepage FAIL.

Route-level boundaries:
- app/error.tsx
- app/not-found.tsx
- segment-level loading/error boundaries where useful

## 4. Route map
- /
- /haqqinda
- /xidmetler
- /xidmetler/[slug]
- /simptomlar
- /simptomlar/[slug]
- /xestelikler
- /xestelikler/[slug]
- /bloq
- /bloq/[slug]
- /faq
- /elaqe

Optional later:
- /axtaris
- /etiket/[slug]

## 5. Homepage content rule
Admin can manage content and ordering only inside known section contracts.

Admin may control:
- enabled/disabled
- sort order
- title
- subtitle
- description
- image(s)
- CTA label/target
- selected related records

Admin may NOT:
- inject arbitrary React
- inject arbitrary HTML layout
- create unknown component types
- break responsive layout rules

## 6. Mobile design rules
- no decorative microcopy below readable size
- no essential text over doctor photography
- mobile hero may use a separate standing portrait
- body copy target: 17–19 px
- card title target: 20–24 px
- H2 target: 38–46 px
- H1 target: 48–60 px
- buttons: 17–19 px
- metadata should generally not go below 15–16 px
- generous whitespace; one primary message per section

## 7. Media slots
Every image field in admin must show the required slot specification.

Initial specifications:
- hero_desktop: 1440×1800, 4:5
- hero_mobile: 1200×1800, 2:3
- about: 1200×1500, 4:5
- service_detail: 1600×1200, 4:3
- symptom_detail: 1600×1200, 4:3
- condition_detail: 1600×1200, 4:3
- blog_cover: 1600×900, 16:9
- blog_inline: 1600×1067, 3:2
- review_avatar: 600×600, 1:1
- og_image: 1200×630, 1.91:1
- logo: SVG preferred

Wrong aspect ratios must produce an admin validation error, not silent acceptance.

## 8. WhatsApp conversion contract
All request/appointment CTAs ultimately route to WhatsApp.

Every CTA must know:
- source = website
- page_type
- page_slug
- cta_position
- optional topic/title

Example human-visible message:
Salam. Ruhiyyə İbrahimlinin saytından müraciət edirəm.
Mövzu: Əldə keyimə

Tracking event should record:
- page_type
- slug
- cta_position
- timestamp/client analytics context

Never hardcode separate WhatsApp URLs inside individual page components.

## 9. Detail-page composition
Shared blocks:
- Breadcrumbs
- DetailHero
- Intro
- RichContent
- Causes / Symptoms / Diagnosis / Treatment where applicable
- WhenToSeeDoctor
- FAQ
- RelatedServices
- RelatedSymptoms
- RelatedConditions
- RelatedArticles
- DoctorCard
- WhatsAppCTA
- Footer

Different page types reuse blocks but receive different data.

## 10. SEO/GEO requirements
Every indexable page supports:
- title
- meta description
- canonical
- OG image/title/description
- index/noindex
- breadcrumbs
- updated date
- author where applicable

Structured data candidates:
- Person / Physician
- ProfilePage
- Article
- BreadcrumbList
- WebSite
- WebPage
- MedicalWebPage where semantically appropriate

No mass low-quality AI content.

## 11. Performance principles
- Server Components first
- minimal client components
- no autoplay hero video
- images optimized and responsive
- map lazy/deferred
- analytics and third-party scripts minimized
- avoid heavy animation libraries unless clearly justified
- use CSS/IntersectionObserver/native scroll where possible
- respect prefers-reduced-motion

Targets:
- LCP <= 2.5 s
- INP <= 200 ms
- CLS <= 0.1

## 12. Implementation order
1. Freeze architecture/contracts/docs
2. Build complete frontend using typed mock data
3. Verify desktop/mobile/detail states
4. Freeze API contracts
5. Implement Django/PostgreSQL backend
6. Replace mock repository with API repository
7. Integration verification
8. SEO/performance/launch verification

Backend must not force a redesign of frontend contracts.

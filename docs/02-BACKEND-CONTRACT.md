# Backend Contract v1

## 1. Goal
Django must behave as a content/API implementation of already-frozen frontend contracts.

It must not redesign frontend component contracts.

Stack:
- Django
- Django REST Framework
- PostgreSQL

## 2. Core model groups

### Site configuration
SiteSettings
- siteName
- defaultSeo
- phone
- whatsappNumber
- social links
- default OG image

DoctorProfile
- fullName
- specialty
- shortBio
- fullBio
- desktopPortrait
- mobilePortrait
- credentials
- social links

HomepageSection
- sectionType enum
- enabled
- sortOrder
- title
- subtitle
- description
- CTA configuration
- selected/related content references where appropriate

Do not create arbitrary page-builder blocks.

### Medical content
Service
Symptom
Condition
Article
ArticleCategory
FAQ
Review
Location

### SEO/operations
Redirect
MediaAsset or validated media fields
optional analytics event ingest only if required later

## 3. Relations
Examples:
- Symptom M2M Condition
- Symptom M2M Service
- Symptom M2M Article
- Condition M2M Service
- Condition M2M Article
- Service M2M Article
- FAQ can relate to Service/Symptom/Condition/Page scope

Relations must support automated internal linking.

## 4. Shared editorial fields
Content entities should support:
- title
- slug
- shortDescription/excerpt
- content
- published
- sortOrder where relevant
- createdAt
- updatedAt

Medical/editorial records may support:
- author
- medicallyReviewedAt
- sources/references

## 5. Shared SEO fields
Reusable abstract model or composition:
- seoTitle
- metaDescription
- canonicalUrl
- ogTitle
- ogDescription
- ogImage
- index
- follow

## 6. Media validation
Admin form must show exact requirements per image slot.

Validation checks:
- file type
- max size
- aspect ratio tolerance
- minimum dimensions

Initial slot specs:
- hero_desktop 1440×1800 4:5
- hero_mobile 1200×1800 2:3
- about 1200×1500 4:5
- service_detail 1600×1200 4:3
- symptom_detail 1600×1200 4:3
- condition_detail 1600×1200 4:3
- blog_cover 1600×900 16:9
- blog_inline 1600×1067 3:2
- review_avatar 600×600 1:1
- og_image 1200×630 1.91:1

Admin should explain requirements before upload and reject materially wrong media.

## 7. Admin UX
Django Admin must be comfortable for non-developer use.

Required:
- clear field groups
- preview thumbnails
- search
- filters
- drag/order support if implemented safely
- published status
- helpful text for image dimensions
- slug assistance
- related-content selectors
- no raw technical fields unless needed

## 8. API shape
Prefer versioned API:
- /api/v1/site/
- /api/v1/home/
- /api/v1/services/
- /api/v1/services/{slug}/
- /api/v1/symptoms/
- /api/v1/symptoms/{slug}/
- /api/v1/conditions/
- /api/v1/conditions/{slug}/
- /api/v1/articles/
- /api/v1/articles/{slug}/
- /api/v1/faq/
- /api/v1/locations/

Home endpoint may aggregate homepage data, but frontend repositories must preserve section isolation and safe fallbacks.

## 9. WhatsApp
The public WhatsApp number comes from SiteSettings.

Frontend builds page-specific message and tracking context.

Backend should not require a database row for every WhatsApp click unless analytics architecture later explicitly requires it.

## 10. Security and correctness
- sanitize rich content
- validate slugs
- validate media
- no secrets in repository
- environment-based settings
- secure CORS/CSRF configuration
- production DEBUG off
- database credentials via environment
- admin access protected

## 11. PostgreSQL
Use PostgreSQL from initial migration.

No temporary SQLite production architecture.

## 12. Backend completion in five bounded prompts

Prompt 1 — Foundation
- Django project/apps
- PostgreSQL configuration
- models
- relations
- migrations
- env settings

Prompt 2 — Admin/CMS
- admin registrations
- field groups
- media dimension/aspect validation
- previews
- filters/search
- publishing controls

Prompt 3 — API
- serializers
- viewsets/views
- URLs
- list/detail/home endpoints
- pagination/filtering where useful
- API contract tests

Prompt 4 — SEO/operations
- SEO fields
- redirect model
- sitemap-supporting data
- structured-data source fields
- settings/location/footer data

Prompt 5 — Verification
- automated tests
- permissions
- validation edge cases
- API contract verification against frontend schemas
- production settings checks

Definition of done:
The mock repository can be replaced by ApiRepository without redesigning UI components.

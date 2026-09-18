# Frontend Contract v1

## 1. Folder architecture
Recommended baseline:

src/
- app/
- features/
  - hero/
  - symptoms/
  - about/
  - services/
  - why-us/
  - body-map/
  - articles/
  - faq/
  - reviews/
  - contact/
  - whatsapp/
- components/
  - ui/
  - layout/
  - seo/
  - shared/
- data/
  - repositories/
  - schemas/
  - adapters/
  - mock/
- lib/
  - api/
  - analytics/
  - seo/
  - media/
  - utils/
- config/
  - site.ts
  - media.ts
  - sections.ts
- types/

## 2. Data-access rule
UI components do not call fetch() against Django endpoints.

Bad:
fetch("https://api.example.com/services")

Good:
getServices()

Repository interface:
- MockRepository during frontend build
- ApiRepository after backend integration

UI must remain unchanged when switching repository implementation.

## 3. Runtime validation
Use TypeScript for compile-time typing and Zod (or equivalent lightweight runtime schema validation) at the data boundary.

Invalid optional section data:
- log/report
- render safe fallback or omit section

Invalid critical page data:
- use route-level error/not-found policy

UNKNOWN ≠ PASS.

## 4. Core data contracts

SeoMeta:
- title
- description
- canonical
- ogTitle
- ogDescription
- ogImage
- index
- follow

Media:
- url
- alt
- width
- height
- optional focalPoint

CTA:
- label
- kind: whatsapp | internal | external
- target
- trackingPosition

Service:
- id
- slug
- title
- shortDescription
- content
- icon
- heroImage
- seo
- isFeatured

Symptom:
- id
- slug
- title
- shortDescription
- content
- icon
- heroImage
- bodyRegion
- relatedServices
- relatedConditions
- relatedArticles
- seo

Condition:
- id
- slug
- title
- shortDescription
- content
- heroImage
- relatedSymptoms
- relatedServices
- relatedArticles
- seo

Article:
- id
- slug
- title
- excerpt
- content
- coverImage
- category
- author
- publishedAt
- updatedAt
- reviewedAt
- relatedSymptoms
- relatedConditions
- relatedServices
- seo

FAQ:
- id
- question
- answer
- sortOrder

Review:
- id
- displayName
- city
- text
- rating
- avatar
- isPublished

Location:
- name
- address
- mapUrl
- phone
- whatsapp
- workingHours

## 5. Section registry
Homepage section type is an enum, not arbitrary strings:
- HERO
- SYMPTOMS
- ABOUT
- SERVICES
- WHY_US
- BODY_MAP
- ARTICLES
- FAQ
- REVIEWS
- CONTACT

Each section receives only its own validated props.

## 6. Responsive rules
Primary breakpoints may follow Tailwind/default modern breakpoints, but design decisions must be content-led.

Mobile:
- minimum horizontal page padding: 20–24 px
- section vertical spacing: 72–96 px
- no unreadable microcopy
- no essential text over portrait
- separate mobile portrait supported

Desktop:
- controlled max-width container
- large editorial whitespace
- readable line length
- image/text composition should not compress typography

## 7. Animation
Default:
- CSS transitions/transforms
- IntersectionObserver
- native scroll snap where suitable

Avoid:
- global animation runtime for basic reveal effects
- layout-affecting hero animation
- animation that blocks first content paint

Required:
- prefers-reduced-motion support

## 8. WhatsApp component
Single reusable component:
WhatsAppCTA({ pageType, slug, ctaPosition, topic, label })

Responsibilities:
- create wa.me target
- generate readable prefilled message
- emit analytics event
- never expose tracking parameters as confusing patient-facing text

## 9. Rich content
One reusable RichContent renderer.

Allowed content semantics:
- h2
- h3
- paragraph
- strong/emphasis
- ordered/unordered lists
- quote
- image
- table
- internal/external link

Backend content must be sanitized.

## 10. SEO
Each route must expose a deterministic metadata builder.

No page component should manually recreate SEO logic.

Shared helpers:
- buildMetadata()
- buildBreadcrumbJsonLd()
- buildArticleJsonLd()
- buildPhysicianJsonLd()

## 11. Mock-first completion definition
Frontend is not considered ready for backend until these pass:
- Homepage
- Desktop responsive state
- Mobile responsive state
- Service listing/detail
- Symptom listing/detail
- Condition listing/detail
- Blog listing/detail
- FAQ
- Contact
- WhatsApp CTA source tracking
- Footer
- 404
- route error state
- section empty state
- loading states where needed
- metadata templates
- structured data templates
- keyboard navigation
- reduced motion
- no horizontal overflow

## 12. Dependency discipline
Add a dependency only when native platform/CSS/Next.js cannot solve the requirement cleanly.

Avoid large carousel/animation packages for trivial interactions.

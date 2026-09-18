# AI Studio — Frontend Build Prompt v1

Build the production frontend for the Ruhiyyə İbrahimli neurologist website.

Read and obey these repository documents first:
1. docs/00-CANONICAL-ARCHITECTURE.md
2. docs/01-FRONTEND-CONTRACT.md
3. docs/02-BACKEND-CONTRACT.md

Do not invent architecture that conflicts with them.

## Stack
- Next.js
- TypeScript
- App Router
- Server Components first
- lightweight CSS solution/Tailwind is acceptable
- runtime schema validation at data boundary
- mock repository first

## Current objective
Build the complete frontend with typed mock data. Do NOT implement Django in this task.

The frontend must later connect to Django only by replacing the data repository/adaptor layer.

## Visual direction
Premium, modern, editorial personal medical brand.
Avoid generic hospital blue/green.
Use warm ivory/off-white background, deep burgundy/plum accent and near-black typography.
Primary font direction: Poppins. Montserrat is acceptable only if it improves a defined component.

Desktop and mobile must have intentionally different compositions where needed.

## Critical mobile rule
Do not use tiny decorative text.
Remove microcopy that does not add real user value.
Readable body text is more important than fitting more content on screen.

Do not place important hero text over the doctor's portrait.
Support separate desktop and mobile hero images.
Mobile may use a standing portrait.

## Routes
Implement:
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

## Homepage sections
Implement as isolated features:
- Hero
- Symptoms
- About
- Services
- WhyUs
- BodyMap
- Articles
- FAQ
- Reviews
- Contact
- Footer

Do not create one giant homepage component.

## Failure isolation
One optional section failing validation must not crash the full homepage.
Provide safe section fallbacks/omission and route-level error handling.

## Detail pages
Use shared reusable components:
- Breadcrumbs
- DetailHero
- Intro
- RichContent
- WhenToSeeDoctor
- related content blocks
- FAQ
- DoctorCard
- WhatsAppCTA
- Footer

## WhatsApp
All appointment/request CTAs go to WhatsApp through one reusable component.

It must accept:
- pageType
- slug
- ctaPosition
- topic
- label

Example message:
Salam. Ruhiyyə İbrahimlinin saytından müraciət edirəm.
Mövzu: Əldə keyimə

Emit a separate analytics event containing source page and CTA position.

## SEO
Implement shared metadata helpers and JSON-LD helpers.
Support canonical, index/noindex, OG metadata, breadcrumb structured data, Article and Physician/Profile structures as appropriate.

## Performance
No hero video.
No unnecessary global client state.
No heavy animation library for basic reveals.
Prefer CSS + IntersectionObserver + native scrolling.
Lazy/defer map and non-critical media.
Respect prefers-reduced-motion.

Targets:
- LCP <= 2.5s
- INP <= 200ms
- CLS <= 0.1

## Media
Use the media-slot requirements from canonical docs.
Design components so desktop/mobile hero assets can be separate.

## Acceptance
Before declaring completion:
- run build/typecheck/lint
- verify all routes
- verify no horizontal overflow at common mobile widths
- verify keyboard navigation
- verify section failure behavior
- verify WhatsApp message source values
- verify metadata output
- verify mock/API repository boundary
- report exact unresolved items; UNKNOWN ≠ PASS

Do not start backend implementation.

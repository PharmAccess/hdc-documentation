# Copilot Instructions for HDC-Documentation (MamaToto)

## Project Overview
This is a Quarto-based documentation site for the MamaToto project, demonstrating Health Data Commons (HDC) platform integration for maternal healthcare. The project involves WhatsApp-based beneficiary enrollment with standards-based FHIR & OpenHIE architecture.

## Architecture & Key Components

### Quarto Configuration (`_quarto.yml`)
- **Website type**: Multi-section site with floating sidebars for "Phase 1" and "IOL" sections
- **Navigation**: Top navbar + contextual sidebars that activate based on content area
- **Theme**: Custom SCSS with PharmAccess branding colors in `theme/styles.scss`
- **Output**: Builds to `_output/` directory

### Content Organization Pattern
```
docs/
├── phase1/          # Beneficiary enrollment workflow docs
├── iol/            # Interoperability Layer (OpenHIM-based)
├── whatsapp.qmd    # WhatsApp integration details
├── shr.qmd         # Shared Health Record
└── [others].qmd    # Standalone documentation pages
```

### Document Structure Convention
All `.qmd` files use consistent frontmatter:
```yaml
---
title: "Clear Title"
description: "Brief description for navigation" # Optional but recommended
sidebar: phase1|iol  # For sectioned content only
format: html         # Usually omitted (inherits from _quarto.yml)
---
```

## Development Workflows

### Local Development
```bash
# Standard preview (auto-reload)
quarto preview

# Docker development environment
docker compose -f docker/compose.yml up
# Serves on localhost:4040
```

### Content Creation Guidelines
1. **New phase1 content**: Add to `docs/phase1/` and update `_quarto.yml` sidebar contents
2. **New IOL content**: Add to `docs/iol/` with appropriate subdirectory and update sidebar
3. **Standalone pages**: Place in `docs/` root and add to main navbar
4. **Code examples**: Use fenced code blocks with language specification for syntax highlighting

### Technical Documentation Patterns
- **System integration examples**: Include JSON configuration snippets (see `docs/phase1/technical-components.qmd`)
- **API examples**: Show both request/response payloads for FHIR, WhatsApp API interactions
- **Architecture diagrams**: Reference external systems (SHR, Carepay, IOL, WhatsApp Business API)

## Domain-Specific Context

### Key Systems Integration
- **IOL (Interoperability Layer)**: OpenHIM-based, handles Keycloak integration
- **SHR (Shared Health Record)**: FHIR-compliant central data repository
- **WhatsApp Business API**: Primary user interface for maternal health enrollment
- **Carepay**: Financial transaction system linked to clinical data

### Project Phases
- **Phase 1**: Focus on WhatsApp self-enrollment workflow for pregnant mothers
- **Future phases**: Hybrid maternal care with virtual/physical provider data sharing

## Styling & Branding
- Uses PharmAccess branding colors and fonts (Fira Sans Condensed, Barlow)
- Custom active link styling with accent color (`$paf-color-accent`)
- Title block and program details styling for homepage (`html/home/title-block.html`)

## Content Sections
- **Phase 1**: Complete workflow documentation with technical components, challenges, outcomes
- **IOL**: OpenHIM customizations, Keycloak integration, FHIR web challenges
- **WhatsApp**: Integration architecture, chatbot workflows, consent management

When creating new content, follow the established patterns for frontmatter, maintain consistency with the sidebar navigation structure, and include practical code examples relevant to health data interoperability standards.
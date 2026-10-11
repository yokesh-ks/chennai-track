---
name: my-custom-skill
description: Custom skill for Chennai Local project workflows
---

# My Custom Skill

## Overview
This skill provides custom workflows for the Chennai Local train web app project.

## Parameters
- **task** (required): Description of the task to perform
- **context** (optional): Additional context about the project state

## Steps
### 1. Analyze Requirements
Understand the task in context of Chennai Local project (bilingual, offline-first, PWA).

**Constraints:**
- You MUST consider Tamil/English bilingual requirements
- You MUST ensure offline capability (PWA)
- You MUST follow data provenance requirements

### 2. Implement Solution
Write code following project patterns (React/Next.js, TypeScript, IndexedDB).

**Constraints:**
- You MUST use existing component patterns
- You MUST add proper TypeScript types
- You SHOULD add accessibility attributes (WCAG 2.2 AA)

### 3. Verify
Test the implementation works offline and in both languages.

**Constraints:**
- You MUST verify offline caching works
- You SHOULD test Tamil text rendering

## Examples
```
task: "Create station search component with Tamil transliteration"
context: "Using existing station data model in src/types/station.ts"
```

## Troubleshooting
- If Tamil text doesn't render: Check font loading and Unicode support
- If offline fails: Verify service worker registration and IndexedDB schema
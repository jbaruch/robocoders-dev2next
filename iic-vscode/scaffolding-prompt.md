#new Workspace: Java Spring Boot + Preact

Create a new workspace for a modern web application following Intent Integrity Chain methodology.

Stack:

Backend: Latest Java (21+) with Spring Boot 3.5
Frontend: HTML/Preact (lightweight React alternative)
Build: Maven multi-module project
Methodology: Intent Integrity Chain (IIC)

CRITICAL: Context7 First
Before ANY scaffolding, you MUST:

Resolve Context7 library IDs:

resolve-library-id for "spring boot"
resolve-library-id for "preact"
resolve-library-id for "maven"


Retrieve documentation:

get-library-docs for each resolved ID
Focus on: project structure, scaffolding, build configuration


If ANY Context7 call fails → STOP immediately and report:

   ⛔ CONTEXT7 STOP CONDITION
   Issue: [cannot reach/not found/no docs]
   Library: [library name]
   Cannot scaffold workspace without authoritative documentation.


   Create backend/.instructions.md with YAML frontmatter:
yaml---
description: Backend development best practices for Spring Boot
applyTo: backend/**/*.java
---
Content should include Spring Boot best practices from Context7 documentation

Create frontend/.instructions.md with YAML frontmatter:
yaml---
description: Frontend development best practices for Preact
applyTo: frontend/**/*.{jsx,tsx,js,ts}
---
Content should include Preact best practices from Context7 documentation

Important Notes

This is SCAFFOLDING ONLY - no business logic
No domain-specific dependencies added yet
Minimal placeholder code only
Structure must follow IIC methodology exactly
All third-party library usage MUST reference Context7
.instructions.md files use YAML frontmatter with applyTo glob patterns

Remember: you don't even have the business requirements yet. You only create empty project.
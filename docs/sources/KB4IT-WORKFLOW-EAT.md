---
Author: Tomás Vírseda
Category: Post
Date: 2026-02-16 17:30:00
DocType: Explanation
Product: KB4IT
Tag: analysis, extraction, transformation, workflow
Topic: Development, Refactoring
---

# KB4IT: Worflow amended: Now it EATs

## Excerpt

Over the past few weeks, I refactored a spaghetti backend module into four services: Backend, Processor (using EAT approach), Compiler, and Deployer.

## Refatoring: Extraction, Analysis and Transformation

During the last weeks, I've spent quite sometime refactoring backend module's spaguetti code.
This leaded to split this service in four new modules: backend, processor, compiler and deployer.

1. **Backend**: keep on charge of checking the environment and coordinate some service intializations
2. **Processor**: this service follow an EAT approach:
   - **Extraction**: consumes sources and get all documents properties
   - **Analysis**: decides which documents, keys (and values for these keys) need to be compilated again
   - **Transformation**: sends key/values to theme to produce the final asciidoc sources
3. **Compiler**: once the transformation is finished, KB4IT starts compiling all documents produced
4. **Deployer**: finally, after compilation, the new site is deployed: target directory is refreshed and temporary dirs are cleaned. Cache is kept so the next execution, KB4IT can decide what must be compiled (if not forced)

Themes updated accordingly to use new capabilities.

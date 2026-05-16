# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**tagit-core-gradle** is a collection of shared Gradle scripts used by Tagit/Mobeix Java frameworks, libraries, and applications. These scripts are consumed remotely by other repositories via GitHub raw URLs, versioned by branch name (e.g., `release/v7.5`).

This repo contains **no buildable source code** — only Gradle scripts in the `gradle/` directory. There is no `build.gradle` or `settings.gradle` at the root.

## Script Architecture

### Core Scripts

| Script | Purpose |
|--------|---------|
| `tagit.gradle` | Foundation for all projects — repositories, compilation settings, Eclipse config, JAR manifest, dependency management. All other scripts build on this. |
| `tagit-rest.gradle` | Spring Boot REST apps (WAR/JAR). Applies `tagit.gradle` + `tagit-shared-lib.gradle`. Handles context.xml generation, Tomcat version, context path normalization. |
| `tagit-angular.gradle` | Angular UI apps. Node.js/NPM integration via node-gradle plugin. Packages Angular dist as JAR for classpath inclusion. |
| `tagit-shared-lib.gradle` | Activated by `-Dsharedlibs`. Excludes common libraries from WAR for Tomcat shared-lib deployment. Generates context.xml with DirResourceSet. |
| `publications.gradle` | Maven publishing to multiple Nexus repositories. Auto-selects SNAPSHOT/RELEASE repo based on version. Must be applied BEFORE `tagit-rest.gradle`. |
| `tagit-repo.gradle` | Standalone repository configuration (mirrors repo config from `tagit.gradle`). |
| `tagit-settings.gradle` | Settings-level build cache config (local dev + remote S3 for CI). |
| `tagit-sonar.gradle` | JaCoCo coverage + SonarQube analysis. Routes to different SonarQube instances based on package group (Mobeix, MCB, MRB, MFB, MDB, Tagit, Services). |
| `spotless/style.gradle` | Code formatting (Google Java Format AOSP, TypeScript headers). Currently disabled in `tagit.gradle`. |

### Dependency Graph

```
tagit.gradle (base)
  ├── tagit-rest.gradle → tagit-shared-lib.gradle
  ├── tagit-angular.gradle
  ├── tagit-sonar.gradle
  └── tagit-repo.gradle (alternative standalone repo config)

publications.gradle (apply BEFORE tagit-rest.gradle)
tagit-settings.gradle (settings.gradle level only)
```

## Key Build Properties

- `-PcontextPath=<path>` — Tomcat context path for WAR deployment
- `-Dsharedlibs` — Enables shared library mode (WAR exclusions + context.xml generation)
- `-PdebugExclusions` / `-PtraceExclusions` — Debug shared library exclusion logic
- `-Dcloud`, `-Donprem`, `-Dpartner`, `-Dlabs`, `-Dpoc`, `-Dlocal`, `-Dtraining` — Repository selection (defaults to central `nexus.tagitmobile.com`)

## Conventions

- Repository selection is controlled by system properties, checked in priority order: partner-cloud → cloud → labs → onprem → poc → services → partner → training → local
- All Nexus credentials come from project properties (`repoUser`, `repoPassword`)
- Tests are disabled by default in `tagit-sonar.gradle` and only run when explicitly invoked
- Java compilation uses UTF-8 encoding with `-parameters` flag for method parameter metadata
- Version strings are normalized to numeric parts only for SonarQube

## Versioning

Scripts are versioned by Git branch name (e.g., `release/v7.5.2`). Consumer projects reference scripts via:
```
apply from: "https://raw.githubusercontent.com/tagitmobile/tagit-core-gradle/${tagitGradleVersion}/gradle/<script>.gradle"
```

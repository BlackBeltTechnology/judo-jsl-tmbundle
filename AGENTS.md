# JUDO JSL TextMate Bundle - Project Documentation

## Project Overview

**Repository:** BlackBeltTechnology/judo-jsl-tmbundle
**License:** Eclipse Public License 2.0 (EPL-2.0)
**Java Version:** 11 (Zulu distribution)
**Build System:** Maven 3.9.4+ with Maven Wrapper (`./mvnw`)

1. Provides TextMate grammar-based syntax highlighting for the JUDO Specific Language (JSL) — the DSL used across the JUDO platform for defining data models, transfer objects, UI views, and actor-based access control.
2. Packages the grammar definition as a Maven JAR artifact for consumption by other JUDO tooling and IDE integrations.
3. Compatible with all TextMate-aware editors: VS Code, JetBrains IDEs (via TextMate Bundles plugin), Sublime Text, and others.

## Directory Structure

```
judo-jsl-tmbundle/
├── Syntaxes/                  # TextMate grammar definitions
│   └── jsl.tmLanguage         # Core grammar file (XML plist format)
├── doc/                       # Documentation and screenshots
│   ├── install-idea.md        # JetBrains IDE installation guide
│   └── images/                # Screenshots for install guide
├── .github/
│   ├── workflows/             # CI/CD GitHub Actions
│   │   ├── build.yml          # Main build pipeline
│   │   ├── release.yml        # Manual release trigger
│   │   ├── merge-pr-tagged.yml        # Auto-merge on tag
│   │   ├── create-release-on-master.yml
│   │   ├── create-release-tagged.yml
│   │   ├── bump-version.yml
│   │   ├── build-dependabot.yml
│   │   ├── delete-old-draft-releases.yml
│   │   ├── sync-labels.yml
│   │   └── jira-description-to-pr.yml
│   ├── CIFLOW.md              # CI/CD flow documentation
│   └── ISSUE_TEMPLATE/        # GitHub issue templates
├── info.plist                 # TextMate bundle metadata (name, UUID)
├── pom.xml                    # Maven build configuration
├── logback-test.xml           # Logging config (test)
├── .vscode/settings.json      # VS Code project settings
├── .zed/settings.json         # Zed editor project settings
└── openspec/                  # OpenSpec change management
```

## Core Modules

This is a **single-module** Maven project (no sub-modules). All functionality is contained in the root.

| Component | Type | Purpose |
|---|---|---|
| `Syntaxes/jsl.tmLanguage` | TextMate Grammar (XML plist) | Defines regex-based syntax highlighting rules for all JSL constructs |
| `info.plist` | TextMate Bundle Metadata | Bundle name ("JSL"), UUID, and contact information |
| `pom.xml` | Maven Build Config | Packages grammar into JAR, handles versioning and deployment |

### Grammar Structure

The `jsl.tmLanguage` file organizes JSL syntax into a hierarchy of named pattern repositories:

| Repository Pattern | Scope | What It Highlights |
|---|---|---|
| `#entity-block` | `meta.class.entity.jsl` | Entity declarations with fields, relations, identifiers, constraints, events |
| `#transfer-block` | `meta.class.transfer.jsl` | Transfer object declarations with actions, links, events |
| `#view-block` | `meta.class.view.jsl` | View declarations with tables, groups, links |
| `#row-block` | `meta.class.row.jsl` | Row declarations |
| `#actor-block` | `meta.class.actor.jsl` | Actor declarations with menus, access rules, claims |
| `#enum-block` | `meta.class.enum.jsl` | Enum declarations with members |
| `#model` | `meta.model.jsl` | Model declarations |
| `#import` | `meta.import.jsl` | Import statements |
| `#annotations` | `meta.annotation.jsl` | `@Annotation` syntax |
| `#types` | `meta.type.typeDeclaration.jsl` | Type declarations with primitives |
| `#primitives` | `storage.type.primitive.jsl` | Built-in types: Boolean, Date, Numeric, String, Timestamp, Time, Binary, Enumeration |
| `#keywords` | `keyword.control.jsl` | `self`, ternary operators |
| `#operators` | `keyword.operator.*.jsl` | Comparison, assignment, arithmetic (`div`, `mod`), logical (`not`, `and`, `or`, `implies`) |
| `#lambdaExpression` | `storage.type.function.arrow.jsl` | Lambda arrow `=>` |
| `#comments` | `comment.line.jsl` / `comment.block.jsl` | `//` line comments, `/* */` block comments |
| `#literals` | `constant.numeric.jsl` / `string.quoted.double.jsl` | Numbers and double-quoted strings |

## Technology Stack

### Core Technologies
- **TextMate Grammar** — XML plist-based syntax definition format, the de facto standard for editor syntax highlighting
- **Maven** — Build and artifact packaging

### Build & Quality
- **Maven 3.9.4+** with Maven Wrapper (`./mvnw`)
- **flatten-maven-plugin 1.1.0** — CI-friendly `${revision}` versioning
- **sign-maven-plugin 1.1.0** — GPG artifact signing
- **nexus-staging-maven-plugin** — Maven Central deployment via OSSRH

## Build Commands

```bash
# Clean build
./mvnw clean install

# Build with specific version
./mvnw -Drevision=1.2.3 clean install

# Deploy to JUDO Nexus
./mvnw -Drevision=1.2.3 -Psign-artifacts -Prelease-judong deploy

# Deploy to Maven Central
./mvnw -Drevision=1.2.3 -Psign-artifacts -Prelease-central deploy
```

> **Note:** There are no tests or Java source code in this project. The build copies `Syntaxes/jsl.tmLanguage` to `target/classes/syntaxes/` and packages it as a JAR.

### Maven Profiles

| Profile | Purpose |
|---|---|
| `sign-artifacts` | Signs artifacts with GPG using sign-maven-plugin |
| `release-dummy` | Deploys to local `/tmp/` directory (for testing) |
| `release-judong` | Deploys to JUDO Nexus (`nexus.judo.technology`) |
| `release-central` | Deploys to Maven Central via OSSRH (`oss.sonatype.org`) |
| `update-source-code-license` | Updates project license headers (EPL v2) |

## Key Configuration Files

| File | Purpose |
|---|---|
| `Syntaxes/jsl.tmLanguage` | The grammar definition — the main deliverable of this project |
| `info.plist` | TextMate bundle identity (name: "JSL", UUID: `84E8AD17-C93C-44CB-ABC0-4FC08A3E0B0A`) |
| `pom.xml` | Maven build with CI-friendly `${revision}` versioning |
| `.mvn/extensions.xml` | Maven build extensions |
| `.mvn/wrapper/maven-wrapper.properties` | Maven Wrapper distribution config |
| `logback-test.xml` | Logging configuration for test context |
| `.github/workflows/build.yml` | Main CI pipeline — builds, tests, deploys, tags, and releases |

## Development Environment

**Required:**
- Java 11 JDK (Zulu distribution recommended)
- Maven 3.9.4+ (or use `./mvnw`)

**Editors for testing grammar changes:**
- VS Code with a TextMate grammar testing setup
- JetBrains IDE with TextMate Bundles plugin (see `doc/install-idea.md`)

## Git Workflow

- **Main Branch:** `develop`
- **Release Branch:** `master` (contains latest released version)
- **Versioning:** CI-friendly `${revision}` property, default `1.0.0-SNAPSHOT`
- **Branch naming:** `feature/JNG-xxx_description`, `bugfix/JNG-xxx_description`, `release/X.Y.Z`
- **Commit rule:** Every commit must reference a JIRA ticket (`JNG-xxx`)
- **PR model:** GitHub forking model, PRs target `develop`

## Important Notes

1. The entire deliverable of this project is a single file: `Syntaxes/jsl.tmLanguage`. All other files are build/CI infrastructure.
2. The grammar uses XML plist format — when editing, be careful with XML escaping (e.g., `&lt;` for `<`, `&gt;` for `>` in regex patterns).
3. Scope names follow TextMate conventions (e.g., `keyword.control.jsl`, `entity.name.type.jsl`, `storage.type.primitive.jsl`). These determine which editor theme colors apply.
4. The JAR artifact is published to both JUDO Nexus and Maven Central, enabling other JUDO tools to bundle the grammar programmatically.
5. There are no unit tests — grammar correctness is verified by visual inspection in an editor with the bundle loaded.

## Related Documentation

- [README](README.md) — Project overview and installation
- [Contributing Guide](CONTRIBUTING.md) — How to submit issues and PRs
- [CI/CD Flow](.github/CIFLOW.md) — Detailed branching, versioning, and CI pipeline documentation
- [JetBrains Installation](doc/install-idea.md) — Step-by-step IDE setup guide

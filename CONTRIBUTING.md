# Contributing to JUDO JSL TextMate Bundle

## Development Environment

Ensure your environment matches the requirements from the [JUDO Community Contributing Guide](https://github.com/BlackBeltTechnology/judo-community/blob/develop/CONTRIBUTING.adoc):

- **Java:** JDK 11 (Zulu distribution recommended)
- **Maven:** 3.9.4+ (or use the included `./mvnw` wrapper)

## Code Structure

This project contains no Java source code — it is a TextMate grammar bundle. The core file is `Syntaxes/jsl.tmLanguage`, an XML plist that defines regex-based syntax highlighting rules for JSL files.

When editing the grammar:
- Each JSL construct (entity, transfer, view, etc.) is defined as a named pattern in the `<repository>` section
- Patterns use `begin`/`end` pairs for block constructs and `match` for single-line constructs
- Scope names follow TextMate naming conventions (e.g., `keyword.control.jsl`, `entity.name.type.jsl`)

## Submitting an Issue

Before creating a new issue, search the [issue tracker](https://github.com/BlackBeltTechnology/judo-jsl-tmbundle/issues) — your problem may already be reported or resolved.

To help us reproduce and fix bugs quickly, please include:

- Output of `java -version` and `mvn -version`
- The JSL code snippet that is incorrectly highlighted
- A screenshot showing the incorrect vs. expected highlighting
- Your editor name and version

File new issues using the [issue form](https://github.com/BlackBeltTechnology/judo-jsl-tmbundle/issues/new/choose).

## Submitting a Pull Request

This project follows [GitHub's standard forking model](https://guides.github.com/activities/forking/). Fork the repository and submit pull requests against the `develop` branch.

## Commands

```bash
# Run tests
./mvnw clean test

# Full build
./mvnw clean install
```

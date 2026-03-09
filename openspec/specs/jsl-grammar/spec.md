# jsl-grammar Specification

## Purpose

Defines TextMate grammar rules for the JUDO Specific Language (JSL) to provide syntax highlighting in TextMate-compatible editors (VS Code, JetBrains, Sublime Text). The grammar is defined in `Syntaxes/jsl.tmLanguage` as an XML plist.

## Architecture

The grammar is structured as a single root pattern (`#code`) that delegates to a repository of named sub-patterns. Each JSL construct type (entity, transfer, view, etc.) has a dedicated block pattern that uses `begin`/`end` regex pairs to match the construct's opening keyword through its closing brace. Inside each block, nested patterns match member declarations (fields, relations, actions, etc.).

Key structural elements:
- **Root**: `#code` includes all top-level patterns
- **Block patterns**: `#entity-block`, `#transfer-block`, `#view-block`, `#row-block`, `#actor-block`, `#enum-block`
- **Member patterns**: `#field-member`, `#relation-member`, `#identifier-member`, `#action-member`, `#link-member`, etc.
- **Shared patterns**: `#annotations`, `#comments`, `#keywords`, `#operators`, `#types`, `#primitives`, `#literals`, `#lambdaExpression`

## Requirements

### Requirement: Entity block highlighting

The grammar SHALL highlight `entity` declarations including the keyword, entity name, optional `extends` clause, and opening/closing braces.

#### Scenario: Simple entity declaration
- **GIVEN** a JSL file containing `entity MyEntity { }`
- **WHEN** the file is opened in a TextMate-compatible editor
- **THEN** `entity` is highlighted as `keyword.control.class.jsl`, and `MyEntity` is highlighted as `entity.name.type.class.jsl`

#### Scenario: Entity with extends
- **GIVEN** a JSL file containing `abstract entity Child extends Parent { }`
- **WHEN** the file is opened in a TextMate-compatible editor
- **THEN** `abstract` and `extends` are highlighted as keywords, both `Child` and `Parent` are highlighted as type names

### Requirement: Transfer block highlighting

The grammar SHALL highlight `transfer` declarations including the keyword, name, optional `maps` clause linking to an entity, and member declarations.

#### Scenario: Transfer with maps clause
- **GIVEN** a JSL file containing `transfer MyTransfer maps MyEntity { }`
- **WHEN** the file is opened in a TextMate-compatible editor
- **THEN** `transfer` and `maps` are highlighted as keywords, `MyTransfer` and `MyEntity` as type names

### Requirement: View and row block highlighting

The grammar SHALL highlight `view` and `row` declarations with their specific member types (table, group, link, column, text).

#### Scenario: View with table member
- **GIVEN** a JSL file containing a `view` block with a `table` member inside
- **WHEN** the file is opened
- **THEN** `view` is highlighted as a keyword, `table` is highlighted as a member keyword

### Requirement: Actor block highlighting

The grammar SHALL highlight `actor` declarations with access control members including `menu`, `access`, and `claim` keywords.

#### Scenario: Actor with menu
- **GIVEN** a JSL file containing `actor MyActor { menu MyMenu { } }`
- **WHEN** the file is opened
- **THEN** `actor` and `menu` are highlighted as keywords

### Requirement: Enum block highlighting

The grammar SHALL highlight `enum` declarations with their member values.

#### Scenario: Enum declaration
- **GIVEN** a JSL file containing `enum Status { Active; Inactive; }`
- **WHEN** the file is opened
- **THEN** `enum` is highlighted as a keyword, member names are highlighted appropriately

### Requirement: Model declaration highlighting

The grammar SHALL highlight `model` declarations at the top of JSL files.

#### Scenario: Model statement
- **GIVEN** a JSL file starting with `model MyModel;`
- **WHEN** the file is opened
- **THEN** `model` is highlighted as `keyword.control.class.jsl` and `MyModel` as `entity.name.type.class.jsl`

### Requirement: Import statement highlighting

The grammar SHALL highlight `import` statements used to reference other JSL models.

#### Scenario: Import statement
- **GIVEN** a JSL file containing `import some::package::Model;`
- **WHEN** the file is opened
- **THEN** `import` is highlighted as a keyword

### Requirement: Type declaration highlighting

The grammar SHALL highlight `type` declarations that define custom types based on primitives.

#### Scenario: Type with primitive
- **GIVEN** a JSL file containing `type MyString String(max-length: 255);`
- **WHEN** the file is opened
- **THEN** `type` is highlighted as a keyword, `String` as a primitive type, and `max-length` as a parameter name

### Requirement: Primitive type highlighting

The grammar SHALL highlight all JSL built-in primitive types: `Boolean`, `Date`, `Numeric`, `String`, `Timestamp`, `Time`, `Binary`, `Enumeration`.

#### Scenario: Primitive types in declarations
- **WHEN** any of the primitive type names appear in a JSL file
- **THEN** they are highlighted with scope `storage.type.primitive.jsl`

### Requirement: Annotation highlighting

The grammar SHALL highlight annotations prefixed with `@`.

#### Scenario: Annotation on entity
- **GIVEN** a JSL file containing `@MyAnnotation entity Foo { }`
- **WHEN** the file is opened
- **THEN** `@MyAnnotation` is highlighted with scope `meta.annotation.jsl`

### Requirement: Comment highlighting

The grammar SHALL highlight both single-line comments (`//`) and multi-line block comments (`/* */`).

#### Scenario: Line comment
- **GIVEN** a JSL file containing `// this is a comment`
- **THEN** the entire line from `//` onward is highlighted as `comment.line.jsl`

#### Scenario: Block comment
- **GIVEN** a JSL file containing `/* multi-line comment */`
- **THEN** everything between `/*` and `*/` is highlighted as `comment.block.jsl`

### Requirement: Operator highlighting

The grammar SHALL highlight comparison operators (`==`, `!=`, `<=`, `>=`, `<`, `>`), assignment (`=`), arithmetic (`+`, `-`, `*`, `/`, `div`, `mod`), and logical operators (`not`, `and`, `or`, `implies`).

#### Scenario: Logical operators
- **GIVEN** a JSL expression using `and`, `or`, `not`, `implies`
- **THEN** they are highlighted with scope `keyword.operator.logical.jsl`

### Requirement: Lambda expression highlighting

The grammar SHALL highlight the lambda arrow operator `=>`.

#### Scenario: Lambda in expression
- **GIVEN** a JSL file containing `items => item.name`
- **THEN** `=>` is highlighted as `storage.type.function.arrow.jsl`

### Requirement: Literal highlighting

The grammar SHALL highlight numeric literals and double-quoted string literals.

#### Scenario: String literal
- **GIVEN** a JSL file containing `"hello world"`
- **THEN** the string is highlighted as `string.quoted.double.jsl`

#### Scenario: Numeric literal
- **GIVEN** a JSL file containing the number `42` or `3.14`
- **THEN** the number is highlighted as `constant.numeric.jsl`

### Requirement: Entity member highlighting

The grammar SHALL highlight entity-specific members: `field`, `relation`, `identifier`, `constraint`, and `event` keywords within entity blocks.

#### Scenario: Field with required modifier
- **GIVEN** an entity block containing `field required String name;`
- **THEN** `field` and `required` are highlighted as keywords, `String` as a primitive type

### Requirement: Action and event lifecycle keywords

The grammar SHALL highlight lifecycle hook keywords: `on`, `instead`, `before`, `after`, `guard` within action and event declarations.

#### Scenario: Action with lifecycle hooks
- **GIVEN** a transfer block with `action void doSomething() on MyEntity::create instead { }`
- **THEN** `action`, `on`, `instead` are highlighted as keywords

### Requirement: Terminal signal highlighting

The grammar SHALL highlight the `error` keyword for terminal/error signal declarations.

#### Scenario: Error declaration
- **GIVEN** a JSL file containing `error MyError { }`
- **THEN** `error` is highlighted as a keyword and `MyError` as a type name

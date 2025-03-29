# Mapping PEST Grammar to TextMate Language Syntax

This guide explains how to translate TypeQL's PEST grammar rules into TextMate language syntax patterns for VS Code syntax highlighting.

## Grammar Reference Source

The file `grammar-reference.pest` in the `typeql-plugin-vscode/syntaxes/` directory is a copy of the official TypeQL PEST grammar from:
```
https://github.com/typedb/typeql/blob/master/rust/parser/typeql.pest
```

This file serves as the source of truth for the TextMate grammar. When the upstream PEST grammar is updated:
1. Update `grammar-reference.pest` with the new version
2. Use the diff to identify changes
3. Apply corresponding updates to the TextMate grammar in `tql.tmLanguage.json`

The presence of this file makes it easier to:
- Track grammar changes over time
- Ensure syntax highlighting stays in sync with the language
- Automate updates using AI assistance
- Validate grammar coverage

## Core Concepts

1. **PEST to TextMate Translation**
   - PEST rules define the language grammar
   - TextMate patterns define how to highlight syntax
   - Focus on capturing the semantic meaning rather than exact grammar structure

2. **Pattern Categories in TextMate**
   - `storage`: Types and modifiers (entity, attribute, value types)
   - `keyword`: Control flow, operators, annotations
   - `support`: Functions and built-in operations
   - `constant`: Language constants and numeric values
   - `string`: String literals and escaped characters
   - `variable`: Variable patterns and captures
   - `comment`: Comment syntax

## Translation Guidelines

### 1. Keywords and Reserved Words

PEST:
```pest
reserved = { WITH | MATCH | FETCH | UPDATE | ... }
WITH = @{ "with" ~ WB }
```

TextMate:
```json
{
    "name": "keyword.control.tql",
    "match": "\\b(with|match|fetch|update)\\b"
}
```

- Look for `reserved` and similar keyword collections in PEST
- Convert to `\\b(word1|word2)\\b` pattern in TextMate
- Use word boundaries (`\\b`) to ensure whole word matches

### 2. Operators and Symbols

PEST:
```pest
comparator = { EQ | NEQ | GTE | GT | LTE | LT }
EQ = @{ "==" }
```

TextMate:
```json
{
    "name": "keyword.operator.characters.tql",
    "match": "==|!=|>=|>|<=|<"
}
```

- Combine symbol-based operators into single regex pattern
- Escape special characters with backslash
- Order longer operators before shorter ones (e.g., `>=` before `>`)

### 3. Types and Storage

PEST:
```pest
kind = { ENTITY | ATTRIBUTE | RELATION }
value_type_primitive = { BOOLEAN | INTEGER | ... }
```

TextMate:
```json
{
    "name": "storage.type.tql",
    "match": "\\b(entity|attribute|relation)(?!-)\\b"
}
```

- Look for type definitions and value types in PEST
- Group related types under appropriate storage categories
- Add negative lookahead `(?!-)` to prevent partial matches

### 4. Variables and Identifiers

PEST:
```pest
var = ${ VAR_ANONYMOUS | var_named }
var_optional = ${ var ~ QUESTION }
```

TextMate:
```json
{
    "match": "\\$\\w+\\??(?:\\[\\])?",
    "name": "variable.parameter.tql"
}
```

- Translate PEST variable patterns to regex
- Include optional modifiers (?, [], etc.)
- Use capture groups for complex patterns

### 5. Functions and Support

PEST:
```pest
builtin_func_name = ${ ABS | CEIL | FLOOR | ... }
reducer = { COUNT | MAX | MIN | ... }
```

TextMate:
```json
{
    "name": "support.function.tql",
    "match": "\\b(abs|ceil|floor|count|max|min)(?!-)\\b"
}
```

- Combine built-in functions and reducers
- Use word boundaries for function names
- Include all variations (reducers, aggregates, etc.)

## Best Practices

1. **Priority Order**
   - Order patterns from most specific to most general
   - Put longer matches before shorter ones
   - Use negative lookaheads to prevent partial matches

2. **Regular Expression Tips**
   - Use `\\b` for word boundaries
   - Use `(?!-)` to prevent partial matches
   - Escape special characters with backslash
   - Use non-capturing groups `(?:...)` for efficiency

3. **Maintenance Strategy**
   - Keep patterns organized by semantic meaning
   - Comment complex regex patterns
   - Update version numbers when making changes

4. **Testing Approach**
   - Test with edge cases (e.g., partial matches)
   - Verify highlighting in different contexts
   - Check for conflicts between patterns

## Automated Update Process

1. **Scan PEST Grammar**
   - Look for `reserved`, `unreserved`, and keyword definitions
   - Identify type definitions and value types
   - Collect function names and operators

2. **Generate TextMate Patterns**
   - Group related elements into appropriate categories
   - Convert grammar rules to regex patterns
   - Combine similar patterns efficiently

3. **Update Language File**
   - Merge new patterns with existing ones
   - Update version number
   - Document changes in changelog

## Common Patterns to Watch

1. **New Keywords**: Check `reserved` and similar collections
2. **New Types**: Look for type definitions and value types
3. **New Operators**: Check operator and symbol definitions
4. **New Functions**: Look for function name definitions
5. **New Annotations**: Check for annotation patterns
6. **Variable Changes**: Watch for variable pattern updates

Remember: The goal is to capture the semantic meaning of the language elements for syntax highlighting, not to implement the full grammar rules. 
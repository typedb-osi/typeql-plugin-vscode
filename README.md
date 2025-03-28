# TypeQL
Syntax highlighter for TypeQL

![Screenshot](./screenshot.jpg)

## Installation
This plugin is published on the [Visual Studio Code Plugin Marketplace](https://marketplace.visualstudio.com/items?itemName=typedb.typeql):
Visual Studio Code > Programming Languages > TypeQL

## Features
Highlight:
- variables
- comments
- reserved keywords

## Documentation
For developers maintaining this plugin:
- [`grammar-reference.pest`](docs/grammar-reference.pest): Snapshot of truth for TypeQL grammar, manually kept up to date with the official [TypeQL PEST grammar](https://github.com/typedb/typeql/blob/master/rust/parser/typeql.pest)
- [`mapping-pest-to-TextMate.md`](docs/mapping-pest-to-TextMate.md): Guide for translating PEST grammar rules to TextMate syntax patterns (use this with your favorite AI editor)

## Issues
Report issues on [github](https://github.com/typedb-osi/typeql-plugin-vscode) or [Discord](https://vaticle.com/discord) #troubleshooting.

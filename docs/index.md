# Alto Language

Alto Language provides structured metadata for 62 programming and document
languages: extensions, aliases, exact filenames, relationships, and syntax
markers.

```php
use Alto\Language\Languages;

$language = Languages::fromFilename('src/Example.php');

echo $language?->name; // PHP
```

## Introduction

- [Installation](installation.md): install the dependency-free package.
- [Getting started](getting-started.md): resolve a file and inspect its language.

## Languages

- [Lookup](lookup.md): resolve slugs, aliases, extensions, and filenames.
- [Catalog](catalog.md): browse and filter the bundled definitions.
- [Definitions](definitions.md): inspect metadata and register application languages.

The package returns metadata. It does not inspect file contents or calculate a
confidence score.

## Package

- [Changelog](https://github.com/altophp/language/blob/main/CHANGELOG.md)
- [Contributing](https://github.com/altophp/language/blob/main/CONTRIBUTING.md)
- [Support](https://github.com/altophp/language/blob/main/SUPPORT.md)

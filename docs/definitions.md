# Language definitions

A `Language` is immutable metadata identified by a lowercase slug.

```php
use Alto\Language\Languages;

$typescript = Languages::get('typescript');

echo $typescript?->name;
echo $typescript?->parent; // javascript
```

Each definition contains `name`, `slug`, `type`, extensions, aliases, exact
filenames, optional release year, optional parent slug, and `CodeMarkers`.

## Syntax markers

```php
$markers = Languages::get('php')?->markers;

$markers?->lineComments;
$markers?->blockComments;
$markers?->docComment;
$markers?->stringDelimiters;
$markers?->heredoc;
$markers?->shebang;
$markers?->openingTag;
$markers?->typicalHeaders;
$markers?->blockStyle;
$markers?->defaultIndentation;
$markers?->indentStyle;
```

`BlockStyle` distinguishes braces, indentation, begin/end blocks, tags, or no
block convention. `IndentStyle` distinguishes spaces and tabs. These values
describe common conventions; they are not a parser grammar.

## Register an application language

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Language\CodeMarkers;
use Alto\Language\Language;
use Alto\Language\LanguageRegistry;
use Alto\Language\LanguageType;

$registry = new LanguageRegistry();
$registry->register(new Language(
    name: 'My Language',
    slug: 'my-language',
    type: LanguageType::Programming,
    extensions: ['.myl'],
    aliases: ['myl'],
    markers: new CodeMarkers(lineComments: ['//']),
));

$language = $registry->fromExtension('.myl');
printf("%s (%s)\n", $language?->name, $language?->slug);
echo json_encode($language?->markers->lineComments, JSON_THROW_ON_ERROR), "\n";
```

Output:

```text
My Language (my-language)
["\/\/"]
```

Bundled definitions are loaded lazily. A custom registration with the same
slug replaces the slug lookup; the first registration keeps ownership of an
extension, alias, or filename.

Inspect collisions with `$registry->conflicts()`. The result groups repeated
keys under `extension`, `alias`, and `filename`.

Keep application registries separate when one service should not see another
service's custom definitions. Check `conflicts()` before choosing an ambiguous
alias or extension. Registration changes lookup metadata, not a syntax parser.

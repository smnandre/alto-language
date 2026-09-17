# Getting started

Use `Languages::fromFilename()` when an application has a path and wants the
best matching bundled definition.

After [installation](installation.md), save this as `language.php` beside `vendor`
and run `php language.php`. An unknown identifier is a normal `null` result.

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Language\Languages;

$language = Languages::fromFilename('templates/home.html.twig');
if (null === $language) {
    echo "Unknown language\n";
    return;
}
printf("name=%s type=%s\n", $language->name, $language->type->value);
echo Languages::resolve('not-a-language')?->name ?? 'Unknown language', "\n";
```

Output:

```text
name=Twig type=template
Unknown language
```

Exact filenames such as `Dockerfile` and `.gitignore` are checked first.
Otherwise, the registry tests extensions from right to left. A compound name
therefore falls back until a registered suffix matches.

Keep `$language` from the first example when inspecting its metadata below.

## Inspect the result

```php
$language->slug;
$language->extensions;
$language->aliases;
$language->filenames;
$language->year;
$language->parent;
$language->markers;
```

`Language` and `CodeMarkers` are immutable and JSON-serializable:

```php
$json = json_encode($language, JSON_PRETTY_PRINT | JSON_THROW_ON_ERROR);
```

See [Lookup](lookup.md) for other identifiers and [Definitions](definitions.md)
for every available field.

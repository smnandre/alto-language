# Language lookup

The static `Languages` facade covers the common lookup paths.

```php
use Alto\Language\Languages;

$php = Languages::get('php');
$rust = Languages::fromExtension('rs');
$python = Languages::fromAlias('py');
$make = Languages::fromFilename('/project/Makefile');
```

Extensions accept an optional leading dot and are normalized to lowercase.
Aliases are case-insensitive. Exact filenames retain their registered case.
Every lookup returns `null` when no definition matches.

## Resolve an unknown identifier

```php
$language = Languages::resolve($identifier);
```

`resolve()` tries, in order:

1. Exact slug.
2. Alias.
3. Extension.
4. Exact filename or filename extension.

Use a specific method whenever the identifier type is already known. It makes
the fallback behavior explicit and avoids an accidental alias match.

## Use an injectable registry

```php
use Alto\Language\LanguageRegistry;

$registry = new LanguageRegistry();
$language = $registry->fromFilename('example.ts');
```

`LanguageRegistry` exposes the same lookup, catalog, and relationship methods
without global state. Prefer it in services that use dependency injection.

## Check the lookup boundary

This standalone example shows case and suffix behavior without reading any file:

```php
<?php

require __DIR__.'/vendor/autoload.php';

use Alto\Language\Languages;

foreach (['Dockerfile', 'dockerfile', 'home.html.twig', 'settings.unknown'] as $filename) {
    printf("%s => %s\n", $filename, Languages::fromFilename($filename)?->slug ?? 'unknown');
}
printf("alias JS => %s\n", Languages::fromAlias('JS')?->slug);
printf("extension .PHP => %s\n", Languages::fromExtension('.PHP')?->slug);
```

Output:

```text
Dockerfile => dockerfile
dockerfile => unknown
home.html.twig => twig
settings.unknown => unknown
alias JS => javascript
extension .PHP => php
```

If a lookup returns `null`, choose an application fallback or register metadata
for that language. Language does not examine content or calculate detection
confidence; use content detection in a separate component if the filename is not
enough.

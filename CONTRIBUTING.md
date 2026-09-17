# Contributing

Use PHP 8.4 or later and run `composer install`, then `composer qa` for style,
static analysis, and tests. `composer cs:fix` applies coding-style fixes.

For metadata changes, add lookup tests for slugs, aliases, extensions, exact
filenames, and collisions. Update the [catalog](docs/catalog.md) and its count
when definitions change. Discuss proposals in the
[issue tracker](https://github.com/altophp/language/issues).

# Twig Adapter

This is a fork of [github.com/frctl/twig](https://github.com/frctl/twig).

We use fractal as a styleguide system for our project and when we use twig as template engine we don't like to copy/paste and do extra work on every change when our templates moves.

So we add some extra path check to allow this adapter to understand a php twig path like:
@namespace/component.twig

For now it's more a workaround, but if you use it and have some issues let us know it

In php you'll have to add directory path and namespace, here an example of code:

```php
$loader    = new Twig_Loader_Filesystem();
$directory = new RecursiveDirectoryIterator('path/to/my/component/directory/');
$iterator  = new RecursiveIteratorIterator($directory);
$files     = new RegexIterator(
    $iterator,
    '/^.+\.twig$/i',
    RegexIterator::GET_MATCH
);

$matchFiles = [];
foreach($files as $file) {
    $matchFiles = array_merge($matchFiles, $file);
}

foreach (array_unique($matchFiles) as $file) {
    $directory = pathinfo($file, PATHINFO_DIRNAME);
    $namespace = basename($directory);

    $loader->addPath($directory, $namespace);
}
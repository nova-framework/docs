# Packages

Packages can generate these types: `module, theme, package`

Generate extended:

`php forge make:package --type=package --extended acme-corp/box`

Generate extended with default options (no wizard)

`php forge make:package --quick --type=package --extended acme-corp/box`

Each type of packages/module or theme will be created under their own folder type ie packages will be generated in a folder called `packages` then `themes` and `modules` for their counterpart.
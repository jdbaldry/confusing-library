# confusing-library

A contrived Jsonnet library that demonstrates the confusion that can arise from treating multiple libsonnet files as if they are a package.

This library expects users to import the library like a package with `import 'confusing-library.libsonnet'`.

```console
$ jsonnet confusing-library.libsonnet

{
   "bar": "bar",
   "foo": "bar"
}
```

The library is split into multiple files to help compartmentalize where particular code is. To look for code related to `foo` I look in `foo.libsonnet`.

The problem is that `foo.libsonnet` is not itself a self contained library, it has undeclared dependencies on `bar.libsonnet`.

```console
$ jsonnet foo.libsonnet

RUNTIME ERROR: field does not exist: bar
	foo.libsonnet:2:8-13	object <anonymous>
	During manifestation
```

This is not a problem in the `confusing-library.libsonnet` file as both files are imported and merged together but nothing prevents a user importing `foo.libsonnet` directly and getting a confusing error.

In this contrived example, the error is relatively easy to resolve but in more complex libraries, or libraries that themselves import other libraries it becomes hard for a user to understand where the value is meant to come from.

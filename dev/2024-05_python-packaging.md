# Python packaging (is a nightmare)

There is plenty of tutorial with simple packages. But once you need to include
assets or compile .proto files then you will encounter some traumatic times.

## Including/Excluding files

Seems that setuptools have defaults that [overrides your settings](https://stackoverflow.com/questions/72821586/setuptools-not-properly-excluding-tests). What a programming practice!
No matter how many ways exist for specifying what to include and exclude, setuptools
will always screw you over. The only reliable way is plain old MANIFEST.in. Do I hate that
it is just another file describing something that could be done in setup.py/setup.cfg/pyproject.toml?
Yes. Do I hate that it must be uppercase thus standing out in your project? Hell yes.

For example - setuptools always include tests in your package. Even if you exclude them in
the package definition. No, you have to have `MANIFEST.in` with the following content

```MANIFEST.in
prune tests*
```

## Building your assets

Since there are no hooks in a build process it is better to simply use an external
build tool and then simply include all results with `python -m build`. In case you
need to have your build steps in the python's package build process then stick with
me.

There is something called [in tree build](https://peps.python.org/pep-0517/#in-tree-build-backends).
It is a horrible piece of shit as most things in python. But let's dive in.

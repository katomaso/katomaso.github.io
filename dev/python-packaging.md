# Python packaging (is a nightmare)

There is plenty of tutorial with simple packages. But once you need to include
assets or compile .proto files then you will encounter some traumatic times.

## Building your assets

Since there are no hooks in a build process it is better to simply use an external
build tool and then simply include all results with `python -m build`. In case you
need to have your build steps in the python's package build process then stick with
me.

There is something called (https://peps.python.org/pep-0517/#in-tree-build-backends)[in tree build].
It is a horrible piece of shit as most things in python. But let's dive in.

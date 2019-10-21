GNU Guix
----------
Package manager, system distro, and more

Presentation can be found here:
https://www.cbaines.net/projects/guix/freenode-live-2017/presentation/
  - Has the speaker notes so it's probably better to refer to that instead of these notes...

Guix is kind of a sprawling project

Why Guix?
----------
- User control/Freedom

How does it work?
----------

7 key properties
  - Handle multiple versions/variants
      - If you want to use multiple versions of ruby or python or whatever, it's easier in Guix
  - Per system, user or more general profiles
      - Unlike apt which installs for everyone, you can have per-user installs
  - Tooling for reproducible builds
  - Hybrid source/binary package management
      - Instead of just binary packages, we also have source packages.
  - Support for multiple platforms
      - If you're on Fedora, you can't really install Debian packages. But you can install Guix
        packages on other systems
  - Comprehensive set of tooling
  - Extensibility through Guile

Packages are defined in Guile.

When you build a package, you get several directories in the store. The store is an important concept. Located in /gnu/store, with packages stored in subdirectories within.

Directories in the store have names with 3 components, delimited with dashes:
  - The hash
      - The hash represents the *process* of building the package, not the contents of the package
        itself
      - Building the package with a different compiler would produce a different hash
  - The package name
  - The package version

In the package definition, there's no info about a compiler, nothing about the build process.
Building a package creates a derivation, which stores all the information about the build process.
  - The source code of the package
  - The dependencies used to build it 
  - Environment variables, etc
    
(All of the above are usually defined in terms of other derivations too)

The derivation of a package is found in the store as well. It is a file, with its own hash (separate
from the actual package hash), and then has the package name, version, and then a .drv extension

Install/remove a package to/from your profile:
```
guix package -i/-r [package name]
```

Upgrade your packages:
```
guix upgrade (alias for guix package -u)
```

Roll back to the previous generation of your profile
```
guix package --roll-back
```

When you install something, it creates a new "generation" of your profile which you can move back
and forwards through. So if you upgrade your system and it breaks, you can roll back to a previous
generation and things will work again

You can use `guix environment --ad-hoc [package name]` to spawn a shell that has the package
installed with all of the environment that it needs. So if you want to "try before you buy", you
can use that.


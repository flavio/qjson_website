---
layout: page
comments: false
sharing: false
footer: true
---
[JSON (JavaScript Object Notation)](http://www.json.org/) is a lightweight
data-interchange format. It can represents integer, real number, string, an
ordered sequence of value, and a collection of name/value pairs.

QJson is a [Qt-based](http://qt-project.org/) library that maps JSON data to
`QVariant` objects and vice versa.

JSON arrays will be mapped to `QVariantList` instances, while JSON objects will
be mapped to `QVariantMap`.

Checkout the [usage](/usage) page for more details.

## Install

Gnu/Linux should install QJson using a package manager; binary packages are
available for all the major distributions.

OSX users can install QJson using [mac ports](http://www.macports.org) or
[homebrew](http://mxcl.github.com/homebrew/).

Checkout the [build](/build) page if you want to build QJson from sources.

## License

This library is licensed under the Lesser GNU General Public License version 2.1.

Some files are licensed under the GPLv2 with Bison exception which allows
their distribution using a custom license (like LGPLv2).

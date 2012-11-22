---
layout: page
title: "contribute"
comments: true
sharing: true
footer: true
---


## Source code

Currently QJson code is hosted on a [github repository](https://github.com/flavio/qjson).

You can download latest development version using a git client:

```
git clone git://github.com/flavio/qjson.git
```

Now create a new branch for your new feature/bug fix:

```
git checkout -b my_new_branch
```

## Submitting changes

The recommended way to submit your changes is via a pull request, but
you can send your patches to the QJson developers mailing list or to me.

Before submitting a patch please ensure:

  * Patched code compiles.
  * The patch is fixing a specific issue or implementing a new feature (it's not
    doing multiple things at the same time).
  * If you have patched the bison parser please don't include changes made to
    `json_parser.{cc,hh}`.
  * QJson unit tests have been updated.
  * QJson unit tests are passing.

> **Note well:** I won't merge changes that are not covered by a unit test.

## Unit testing

QJson unit tests are located under the tests directory. You can enable them
passing the `-DQJSON_BUILD_TESTS=yes` option to cmake.

> **Note well:** make sure you followed the [build](/build) instructions.

Move into the `build` directory and type:

```
make tests
```

This is going to run all the unit tests.


If you want to run the `QJson::Parser` unit tests just type:

```
./test/parser/testparser
```

If you want to run the `QJson::Serializer` unit tests just type:

```
./test/serializer/testserializer
```


If you want to run the `QJson::QObjectHelper` tests just type:
```
./tests/qobjecthelper/testqobjecthelper
```


If you want to test the QJson parser against a specific JSON object you can use
the `cmdline_tester` program.

This binary is located under the tests directory and has a straightforward syntax:

```
./tests/cmdline_tester/cmdline_tester text_file_containing_json_object
```

The command will convert the JSON object to a QVariant and dump it to stdout.
More options are available via cli options, just checkout the `--help` output.

> **Note well:** `cmdline_tester` relies on `qDebug()` for dumping the object.
> `qDebug` has some limitations, like being unable to print utf8 chars.


---
layout: page
title: "usage"
date: 2012-11-15 17:29
comments: true
sharing: true
footer: true
---

This page provides a quick overview of QJson's features:

* parsing: from JSON to QVariant.
* serializing: from QVariant to JSON.
* QObject helper: dump and restore QObject's attributes.

For more details checkout QJson's [documentation](http://qjson.sourceforge.net/docs/index.html).


## Parsing: from JSON to QVariant

Converting JSON's data to QVariant instance is really simple:


```cpp
// create a Parser instance
QJson::Parser parser;

bool ok;

// json is a QString containing the data to convert
QVariant result = parser.parse (json, &ok);
```

Suppose you're going to convert this JSON data:

```json
{
  "encoding" : "UTF-8",
  "plug-ins" : [
      "python",
      "c++",
      "ruby"
      ],
  "indent" : { "length" : 3, "use_space" : true }
}
```

The following code would convert the JSON data and parse it:

```cpp
QJson::Parser parser;
bool ok;

QVariantMap result = parser.parse (json, &ok).toMap();
if (!ok) {
  qFatal("An error occurred during parsing");
  exit (1);
}

qDebug() << "encoding:" << result["encoding"].toString();
qDebug() << "plugins:";

foreach (QVariant plugin, result["plug-ins"].toList()) {
  qDebug() << "\t-" << plugin.toString();
}

QVariantMap nestedMap = result["indent"].toMap();
qDebug() << "length:" << nestedMap["length"].toInt();
qDebug() << "use_space:" << nestedMap["use_space"].toBool();
```

The output would be:

```
encoding: "UTF-8"
plugins:
  - "python"
  - "c++"
  - "ruby"
length: 3
use_space: true
```

## Serialization: from QVariant to JSON

QVariant objects are converted to a string containing the JSON data.


Let's declare a QVariant object with some data to convert:

```cpp
QVariantList people;

QVariantMap bob;
bob.insert("Name", "Bob");
bob.insert("Phonenumber", 123);

QVariantMap alice;
alice.insert("Name", "Alice");
alice.insert("Phonenumber", 321);

people << bob << alice;
```

Now it's time to create the `Serializer`:

```cpp
QJson::Serializer serializer;
bool ok;
QByteArray json = serializer.serialize(people, &ok);

if (ok) {
  qDebug() << json;
} else {
  qCritical() << "Something went wrong:" << serializer.errorMessage();
}
```

The output will be:

```
 "[ { "Name" : "Bob", "Phonenumber" : 123 },
    { "Name" : "Alice", "Phonenumber" : 321 } ]"
```
It's possible to tune the indentation level of the resulting string using the
`Serializer::setIndentMode()` method.


## QObject helper

QJson provides an helper class for dumping `QObject`'s attributes to a `QVariant` and
for restoring `QObject`'s attributes from a `QVariantMap`.

Let's define a simple class inhereted from `QObject`:
```cpp
class Person : public QObject
{
  Q_OBJECT

  Q_PROPERTY(QString name READ name WRITE setName)
  Q_PROPERTY(int phoneNumber READ phoneNumber WRITE setPhoneNumber)
  Q_PROPERTY(Gender gender READ gender WRITE setGender)
  Q_PROPERTY(QDate dob READ dob WRITE setDob)
  Q_ENUMS(Gender)

  public:
    Person(QObject* parent = 0);
    ~Person();

    QString name() const;
    void setName(const QString& name);

    int phoneNumber() const;
    void setPhoneNumber(const int phoneNumber);

    enum Gender {Male, Female};
    void setGender(Gender gender);
    Gender gender() const;

    QDate dob() const;
    void setDob(const QDate& dob);

  private:
    QString m_name;
    int m_phoneNumber;
    Gender m_gender;
    QDate m_dob;
};
```
### Dump QObject's attributes to JSON

The following code will serialize an instance of `Person` to JSON:
```cpp
Person person;
person.setName("Flavio");
person.setPhoneNumber(123456);
person.setGender(Person::Male);
person.setDob(QDate(1982, 7, 12));

QVariantMap variant = QObjectHelper::qobject2qvariant(&person);
Serializer serializer;
qDebug() << serializer.serialize( variant);
```

The generated output will be:
```json
{ "dob" : "1982-07-12", "gender" : 0, "name" : "Flavio", "phoneNumber" : 123456 }
```
### Restore QObject's attributes from JSON

It's also possible to initialize a QObject using the values stored inside of a
`QVariantMap`.

Suppose you have the following JSON data stored into a `QString`:
```json
{ "dob" : "1982-07-12", "gender" : 0, "name" : "Flavio", "phoneNumber" : 123456 }
```

The following code will initialize an already allocated instance of `Person` using the JSON values:
```cpp
Parser parser;
QVariant variant = parser.parse(json);
Person person;
QObjectHelper::qvariant2qobject(variant.toMap(), &person);
```


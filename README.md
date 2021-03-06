# Reflect
Complete C++ reflect library in less than 200 lines code.

中文介绍：
  * [200 行的 C++ 反射](https://www.clarkok.com/blog/2015/03/09/200-%E8%A1%8C%E7%9A%84-C-%E5%8F%8D%E5%B0%84/)
  * [100 行的 Enum 反射库](https://www.clarkok.com/blog/2015/05/12/100-%E8%A1%8C%E7%9A%84-Enum-%E5%8F%8D%E5%B0%84%E5%BA%93/)

# Basic Usage

## Class reflection

```C++
#include <iostream>
#include "reflect.h"

class Base {
public:
  Base() {
    std::cout << "Base" << std::endl;
  }
};

RE_class(Class1, Base)
{
public:
  Class1() {
    std::cout << "Class1" << std::endl;
  }
};

RE_class(Class2, Base)
{
public:
  Class2() {
    std::cout << "Class2" << std::endl;
  }
};

int main(int argc, const char * argv[]) {
  Reflect(Base)["Class1"]->create();
  return 0;
}
```

And the result:

```
Base
Class1
```

## Enum reflection

```C++
#include <iostream>
#include "reflect-enum.h"

ENUM_CLASS(Test,
  TEST1,
  TEST2,
  TEST3
);

int main()
{
  std::cout << enum_reflection<Test>()->toString(Test::TEST1) << std::endl;

  std::cout << enum_reflection<Test>()->toString(
      enum_reflection<Test>()->fromString("Test::TEST1")
    ) << std::endl;

  std::cout << enum_reflection<Test>()->toString(
      enum_reflection<Test>()->fromString("TEST1")
    ) << std::endl;
}
```

And the result:

```
Test::TEST1
Test::TEST1
Test::TEST1
```

# Document

This library is a header-only library, include the header wherever you need.

This library supports multi-object-file projects, but has no effect on the
efficiency of that program. Neither does this library need a non-standard
preprocessor.

## Class reflection

### RE_class(ClassName, BaseClass [, tag])
Defined a class to reflect with in TAG. This macro is to replace 
`class ClassName : public BaseClass`.

### Reflect(BaseClass [, tag])
Return a reference to a `map<string, factory>`, aka. the reflect map.

### tag
A tag is a empty struct that defines a group of reflects, and each group of
reflects will not affect others.

## Enum reflection

### ENUM_CLASS(EnumName, \<EnumMenbers\>)
Defined a enum class to reflect, for example:

```C++
ENUM_CLASS( MyEnum    // enum class MyEnum {
  Member1,            //   Member1,
  Member2             //   Member2
);                    // };
```

### std::string ENUM_REFLECT(EnumName)->toString(EnumValue)
map EnumValue to string

### EnumName ENUM_REFLECT(EnumName)->fromString(string)
Reflect from string to EnumValue, Accept EnumMember with or without EnumName

### Reflect in namespace
Define `REFLECT_NS` to your namespace before including `reflect-enum.h`


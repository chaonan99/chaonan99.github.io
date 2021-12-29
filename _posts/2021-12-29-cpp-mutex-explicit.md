---
layout: post
title: "C++ mutex cause call to implicitly deleted constructor"
comments: true
date: "2021-12-29"
description: ""
keywords: "c++, cxx, cc, mutex, constructor, copy constructor, move constructor, explicit, implicit"
---

See the simple test case below:

```c++
#include <iostream>
#include <mutex>


namespace {

class Test {
public:
    explicit Test(int i) {
        std::cout << "custom constructor" << std::endl;
    }

    // Test(Test const& t) {
    //     std::cout << "copy constructor" << std::endl;
    // }

    // Test(Test&& t) {
    //     std::cout << "move constructor" << std::endl;
    // }
private:
    mutable std::mutex mutex_;
};
}

int main() {
    auto t = Test(0);
    // Test t(0);
}
```

On Linux with gcc 6.3.0 it failed to compile with error:

{% include image.html url="../../assets/images/20211229/error.png" description="error message" %}

* This won't fail on MAC with clang 13.0.0.
* Removing line 20-21 clears the error.
* Uncommenting either line 13-15 or 17-19 clears the error (but when running the program these constructors are not called).
* Changing line 26 to line 27 clears the issue.

On compiler explorer ( https://godbolt.org/ ), this failed to compile for gcc <= 6.4.
With gcc >= 7 and -std=c++17 this can pass.
With clang>=5.0.0 and -std=c++17 this can pass.

Conclusion: this is an issue for low version gcc.
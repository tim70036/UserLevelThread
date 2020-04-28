# Cooperative User Level Thread
**User-level** threads are similar to the type of threads you might be familiar with, OS threads, but are implemented entirely in user-level code. They are typically speedier than OS threads since there is no context switching into/out of the kernel. **Cooperative** usel-level threads are user-level threads that release control to the scheduler deliberately, usually through a `Yield()` call. That is, they execute for as long as they want.


## Prerequisites

First, ensure that you are using a machine meeting the following requirements:

* Runs a modern Unix: Linux, BSD, or OS X.
* Runs a 64-bit variant of the OS (check with arch).
* Has GCC or Clang and GNU Make installed.

## Make Targets
Use `make` to build, test.
* `all`: compiles and packages your library.
* `test`: compiles all test cases.
* `clean`: deletes temporary files.

## Usage
A full program using this library might look like this:
```cpp
#include "chloros.h"

extern bool NotFinished(void*);
extern void DoWork(void*);
extern void* GetWork();

void Worker(void* work) {
  while (NotFinished(work)) {
    DoWork(work);
    chloros::Yield();
  }
}

int main() {
  chloros::Initialize();
  for (int i = 0; i < 10; ++i) {
    chloros::Spawn(Worker, GetWork());
  }

  chloros::Wait();
  return 0;
}
```

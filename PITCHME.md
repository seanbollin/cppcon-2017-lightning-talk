---

### Reactor vs. Proactor
### Sean Bollin 

<span style="color: gray">sean@sean-bollin.com</span>

---

### Intro

  - Boost ASIO (C++), Twisted (Python), etc.
  - Codified in Pattern-Oriented Software Architecture (Schmidt, Stal, Rohnert, Buschmann)

---

### Reactor

  - Handles requests delivered concurrently
  - Demultiplexes and dispatches synchronously to handlers
  - Single-threaded

---

### Reactor Handlers
<pre>
```cpp
Reactor reactor;

reactor.addHandler("one", [](){
  std::cout << "one handler called!" << '\n';
});

reactor.addHandler("two", [](){
  std::cout << "two handler called!" << '\n';
});
</pre>
reactor.run();
```

---

### Reactor Event Loop
```cpp
while (true) {
  int numberOfEvents = wait();

  for (int i = 0; i < numberOfEvents; ++i) {
    std::string input;
    std::getline(std::cin, input);

    try {
      handlers.at(input)();
    } catch (const std::out_of_range& e) {
      std::cout << "no handler for " << input << '\n';
    }
  }
} 
```
---

### Epoll

```cpp
class Epoll {
  public:
    static const int NO_FLAGS = 0;
    static const int BLOCK_INDEFINITELY = -1;
    static const int MAX_EVENTS = 1;
 
    int wait() {
      return epoll_wait(
        fileDescriptor,
        events.begin(),
        MAX_EVENTS,
        BLOCK_INDEFINITELY);
    }
};
```

---

### Reactor Limitation

  - What if the handler blocks?

```cpp
    reactor.addHandler("blocking", [](){
      // ifstream setup ..
      while (getline(myFile, line)) {
        std::cout << line << '\n';
      }
    });
```

---

### Proactor

  - Fully asynchronous
  - Can rely heavily on operating system functionality
  - Linux aio, Windows IOCP

---

### Proactor Async I/O

```cpp
#include <aio.h>
// -lrt 

aiocb.aio_fildes = fd;
aiocb.aio_nbytes = BUFSIZE;
aiocb.aio_offset = 0;
 
int ret = aio_read(&aiocb);
```

---

### Proactor

  - Initiator (can start async operations proactively)

---

### Resources
  
  - https://www.ibm.com/developerworks/library/l-async/index.html
  - https://github.com/seanbollin/reactor-proactor-example

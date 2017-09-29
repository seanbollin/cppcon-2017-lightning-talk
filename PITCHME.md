---

### Reactor vs. Proactor
### Sean Bollin 

<span style="color: gray">sean@sean-bollin.com</span>

---

### Intro

<span style="color: gray">Boost ASIO (C++), Twisted (Python), etc.</span><br />
<span style="color: gray">Event-driven programming</span><br />
<span style="color: gray">Pattern-Oriented Software Architecture (Schmidt, Stal, Rohnert, Buschmann)</span>

---

### Reactor

<span style="color: gray">Handles requests delivered concurrently</span><br />
<span style="color: gray">Dispatches synchronously to handlers</span><br />
<span style="color: gray">Single-threaded</span><br />

---

### Reactor Handlers
```cpp
Reactor reactor;

reactor.addHandler("one", `[]`(){
  std::cout << "one handler called!" << '\n';
});

reactor.addHandler("two", `[]`(){
  std::cout << "two handler called!" << '\n';
});

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

<span style="color: gray">What if the handler blocks?</span><br />

```cpp
reactor.addHandler("blocking", `[]`(){
  // ...
  while (getline(myFile, line)) {
    std::cout << line << '\n';
  }
});
```

---

### Proactor

<span style="color: gray">Fully asynchronous</span><br />
<span style="color: gray">Can rely heavily on operating system functionality</span><br />
<span style="color: gray">Linux AIO, Windows IOCP</span>
<span style="color: gray">Initiator (can start async operations proactively)</span><br />

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

### Resources
  
<span style="color: gray">https://www.ibm.com/developerworks/library/l-async/index.html</span>
<span style="color: gray">https://github.com/seanbollin/reactor-proactor-example</span><br />

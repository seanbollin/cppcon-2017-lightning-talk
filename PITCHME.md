---

### Reactor vs. Proactor
### Sean Bollin 

<span style="color: gray">sean@sean-bollin.com</span>

---

### Intro

  - Asynchronous patterns
  - Boost ASIO (C++), Twisted (Python), etc.
  - Codified in Pattern-Oriented Software Architecture (Schmidt, Stal, Rohnert, Buschmann)

---

### Full-code

  - https://github.com/seanbollin/reactor-proactor-example

---

### Reactor

```cpp
Reactor reactor;

reactor.addHandler("one", [](){
  std::cout << "one handler called!" << '\n';
});

reactor.addHandler("two", [](){
  std::cout << "two handler called!" << '\n';
});

reactor.run();
```

---

### Reactor
```cpp
void run() {
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
 
    ~Epoll() {
      close(fileDescriptor);
    }
};
```

---

### Proactor

---

### Resources

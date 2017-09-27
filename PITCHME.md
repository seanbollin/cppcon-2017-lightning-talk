---

### Reactor vs. Proactor
### Sean Bollin 

<span style="color: gray">sean@sean-bollin.com</span>

---

### Intro

  - Asynchronous patterns
  - Utilized in Boost ASIO (C++), Twisted (Python), and others 
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

### Proactor

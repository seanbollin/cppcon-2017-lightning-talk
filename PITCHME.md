---

### Reactor vs. Proactor
### Sean Bollin (sean@sean-bollin.com)

---

### Intro

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

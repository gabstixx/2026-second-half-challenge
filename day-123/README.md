# Day 123/184 - October 31, 2026

## Training: Creational Design Patterns

First session back after missing a lot of classes. Today was design patterns, starting with the creational ones: how we implemented them and why each is used.

### Singleton

- Only one instance exists, shared everywhere
- Downsides: hidden dependencies, global state, hard to test

### Factory

- One place decides which concrete object to create
- Calling code depends on the abstraction, not the class

### Abstract Factory

- A factory that creates families of related objects
- Keeps objects that belong together consistent

### Builder

- Builds a complex object step by step
- Avoids constructors with many parameters

### Prototype

- Creates new objects by cloning an existing one
- Useful when building from scratch is expensive

### Dependency Injection (the D in SOLID)

- Dependencies are passed in, usually through the constructor
- They are explicit, easy to swap, and easy to mock in tests
- Often a better choice than a singleton

```typescript
// Singleton style: hidden, global dependency
class OrderService {
  private db = Database.getInstance();
}

// Dependency injection: explicit and swappable
class OrderService {
  constructor(private db: Database) {}
}
```

### Still to Check

One more creational pattern from today's session that I need to look into.

### Key Learning

Patterns give names to problems I was already running into.
Dependency injection ties directly back to SOLID.

### Next

Finish the remaining creational pattern, then structural patterns.

---

**Streak: 123/184** 🔥

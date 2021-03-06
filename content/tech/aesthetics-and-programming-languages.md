---
Title: Aesthetics and Programming Languages
Subtitle: Why does C♯ drive me insane but I love Rust?
Date: 2018-05-13 11:00
Category: Tech
Tags: [csharp, rust, programming languages]
Summary: >
    Rust isn’t exactly prettier than C♯, but its aesthetics don’t drive me up the wall the same way. Why not?

---

My distaste for the aesthetics of C^♯^ are fairly well known to people I talk to about programming languages—perhaps equally as well known as my love of Rust. So much so that both are running jokes among some of my colleagues and friends. My hypersensitivity to aesthetics both in general and also specifically in programming languages and work environment is *also* so well-known as to be a gag.

But I was writing a bunch of Rust this weekend, and looking at it and thinking about it and wondering why it is that C^♯^ drives me so up the wall aesthetically and experientially, while Rust doesn’t. On the surface, they don’t actually look all that different.

Here’s *roughly* equivalent code in each:

```cs
public class Person {
    public string Name { get; set; } = "Chris";

    public void greet() {
        Console.WriteLine($"Hello, {Name}");
    }
}
```

```rust
struct Person {
    name: String,
}

impl Person {
    pub fn new() -> Person {
        Person { name: String::from("Chris") }
    }

    pub fn greet(&self) {
        println!("Hello, {}", self.name);
    }
}
```

When you start tossing in generics and lifetimes, Rust can actually end up looking a *lot* messier than C^♯^.

```rust
impl<'a, 'b, T, U> SomeTrait<'a, U> for SomeType<'b, U>
where
    T: SomeOtherTrait + YetAnotherTrait,
    U: OhWowSoManyTraits
{
    fn some_trait_method(&self) {
        // ...
    }
}
```

Nothing about that is what I would call aesthetically beautiful in a general sense! There’s a *lot* of syntax.

What I’ve concluded so far, though, is that my difference in feelings comes down to the way that syntax maps back to the underlying semantics, and my feelings about those underlying semantics. The basic language design approach C^♯^ takes—i.e. everything is a class; mutation is both encouraged and implicit; don’t bother with value types—drives me batty. I don’t love the syntax, not least because it ends up being *so* verbose and noisy (you can express the same things in F^♯^ much more briefly)—but also because I actively dislike the programming models it encourages (I don’t like the C^♯^ programming model when I see in in F^♯^ either!).

Rust, by contrast, matches the way I *do* and *want to* think about the world. Mutability is allowed but neither actively encouraged nor actively discouraged; more to the point it’s *explicit*. Insofar as “shared mutable state is the root of all evil,” Rust has two legs up on C^♯^: it (a) doesn’t *allow* shared mutable state and (b) makes explicit where mutation *is* happening. It also separates data from behavior. It also has real value types. It also has sum types and pattern matching. In both cases, a lot of the syntactical noise is inessential, a holdover from the legacy of C; but in Rust’s case the way it maps onto a *programming model* that is more like OCaml than like C decreases the pain I feel from that noise.

This *could* be taken to validate the idea that syntax doesn’t matter, that the underlying semantics are everything, but that’s not the case. It’s not that I *love* Rust’s syntax. It’s that, although I dislike it at times, it doesn’t rise to the level of frustration I feel in C^♯^ because it’s not coupled to a programming model that I loathe. The syntax matters; it’s just not the *only* thing that matters.

An interesting thing to consider: what Rust would look like in a world where it embraced its OCaml roots. (I don’t think Rust should have done this; spending its complexity budget on ideas instead of syntax was the right choice. But it’s still interesting.) The simplest level of translation might look something (very) roughly like this:

```haskell
impl 'a 'b T U SomeTrait 'a T for SomeType 'b U
  where T : SomeOtherTrait + YetAnotherTrait

  some_trait_method :: &self -> void
  some_trait_method self =
    -- ...
```

This is obviously still a lot of syntax, but it’s all basically necessary given the things Rust is trying to express with lifetimes, ownership, etc.—and I did this off the top of my head with literally *no* consideration other than “what’s the most direct translation into roughly Haskell-ish syntax I can write?” It makes me genuinely curious where a language that aimed for Rust’s same kinds of guarantees but actively embracing the ML/Haskell family’s syntax might end up. I have a guess that I’d like it even better than I do Rust.


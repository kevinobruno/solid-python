# SOLID Principles applied to Python

This repository contains notes on SOLID Principles to maintain the knowledge updated and easy
day-to-day reference. The examples are applied to Python language.

## [S - Single Responsability Principle][01-single-responsability-principle]

- One class should be doing one thing or be responsible for only one thing.

Example: If we are building a social media website with users, events, messages, etc... The user
part of the system has nothing to do with the part responsible for creating events pages or
displaying messages so it is better to separate them into different parts with their own
responsibilities.

## [O - Open-Closed Principle][02-open-closed-principle]

- Open for extension but closed for modification.
- Desiging your software so that we can extend a class' behaviour without modifying it.
- Open for extension: A class' behavior can be extended.
- Closed for modification: No changes are allowed in the code.

## [L - Liskov Substitution Principle][03-liskov-substitution-principle]

- Every part of the code should get the expected result no matter what instance of a class it gets,
given it implements the same interface.
- A subclass should override the parent class methods in a way that does not break functionality
from a client’s point of view.

## [I - Interface Segregation Principle][04-interface-segregation-principle]

- A client should never be forced to depend on methods it doesn't use.
- Breaking down interfaces by roles rather than by type favors *composition over inheritance* and
*Decoupling over Coupling*.

Example: Consider an interface called `Animal` which would have `eat`, `sleep`, `walk` methods.
Some animals `fly` so inheriting from the monolith `Animal` would mean there are methods we might
not use. Instead we could break into smaller interfaces by **roles** so our animal that flies could
just use the `canEat` interface for example.

## [D - Dependecy Inversion Principle][05-dependecy-inversion-principle]

- High level modules should not depend on low level modules, they should depend on abstractions.
- Abstractions should not depend on details. Details should depend on abstractions.
- Changes to low level modules should not break high-level ones.

## Reminders

- SOLID Principles are principles, not rules. Make sure not to overfragment, use SOLID to assure
maintainability, use it as a tool, not a goal.
- SOLID Principles are all intertwined and interdependents.
- SOLID Principles are most effective when they are combined together.
- SOLID Principles complement each other and work together in unison to achieve the common purpose
of well-designed software.

## References

- [Robert C. Martin (Uncle Bob) paper "Design Principles and Design Patterns"][uncle-bob-paper].
- [Udemy course SOLID Principles: Introducing Software Architecture & Design][udemy-course].
- [Charlie Gerard's notes][charlie-notes].

[01-single-responsability-principle]: 01-single-responsability-principle.md
[02-open-closed-principle]: 02-open-closed-principle.md
[03-liskov-substitution-principle]: 03-liskov-substitution-principle.md
[04-interface-segregation-principle]: 04-interface-segregation-principle.md
[05-dependecy-inversion-principle]: 05-dependecy-inversion-principle.md
[uncle-bob-paper]: https://web.archive.org/web/20191116231621/https://fi.ort.edu.uy/innovaportal/file/2032/1/design_principles.pdf
[udemy-course]: https://www.udemy.com/course/solid-design
[charlie-notes]: https://github.com/charliegerard/dev-notes/blob/master/softwareDev/solid.md

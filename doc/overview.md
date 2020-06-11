# Overview

Coy is a lightweight, experimental game engine. It attempts to combine the [simplicity](https://www.youtube.com/watch?v=kGlVcSMgtV4) of the [Elm Architecture](https://guide.elm-lang.org/architecture/) with the [easiness](https://www.youtube.com/watch?v=oytL881p-nQ) of [DragonRuby](https://dragonruby.itch.io/dragonruby-gtk). I will lay out the goals of the game engine, the roadmap, the architecture, and a quick guide to start using it.

## Goals
The purpose of Carp is to combine functional programming techniques, type inference, and compile-time borrow checking to make popular ideas of modern software development to performance-intensive projects like videogames. Likewise, the purpose of this engine is to bring a functional, strongly type sensibility to the architecture of a game project. The engine that results should
- **Be reasonably efficient** - I am not systems programmer or C++ game developer, and I am not an expert on the kinds of performance optimizations these programmers know. I am sure to miss some low-hanging fruit, but I intend to write algorithms with appropriate space and time complexity. 
- **Be Data Driven** - The various subsystems of the engine should be driven primarily by simple datastructures the user can edit by hand and, eventually, change at runtime.
- **Be Idiomatically Functional** - The functions exposed by the subsystems should behave in ways that make sense to LISP and Haskell programmers - they should take some values and return another value. They shouldn't mutate or side-effect except when they must, and when they do they should be marked as such. A Clojure, Elm, or Haskell programmer should feel more comfortable with the functions exposed than a C or C++ developer would be.
- **Be Genre Agnostic** - The engine should provide the tools most commonly required by game developers, but will make as few assumptions as possible about what kind of game you're trying to make. Although it currently depends on SDL and focuses on 2D games, the systems that do not deal with graphics or geometry should be completely independent from these dependencies.
- **Be Modular** - Each subsystem should be reasonably separated from the others - you should be able to use the animation system without having to use collision detection, for instance - and should provide an API that only requires conformance to a small core and the requirements of the system itself.

In contrast, the engine does _not_ strive to
- **Be Performance Obsessed** - There are, probably, inevitable performance sacrifices to be made to focus on the kind of architecture I want. When they are tolerably small, or when the architectural sacrifice is too great, these sacrifices will be made in the interest of sane, maintainable code.
- **Be Extensively Cross-Platform** - Although I do intend to target all three major desktop platforms - Windows, MacOS, and Linux - I do not intend to put any effort into developing for or accomodating mobile devices, consoles, or web targets like asm.js or WebAssembly. If you can make it work, I'm happy for you. If you can't, this is not the library for you - at least, not yet.

## Roadmap
- Resource Manager
- Input Handlings
- Timing
- 2D Animation (i.e. spire sheets)
- 2D Collision Detection
- 2D Rigid Body Dynamics
- Serialization/Deserialization of game entities/chunks
- Game World Editor (HTML app via http connection with running game?)

## Architecture
The architecture is similar to the Elm Architecture: your game has a state and an update function: the update function takes the state and a game event, and returns an updated state. Coy itself will provide you with Game events of your own definiton (according to a map of input bindings) and will draw it as well (according to the `get-rasters` function). This loops forever until the game is closed.

### Game Events
Game events are just a sum type of every possible consequence. We don't define them for you - they're for you to define yourself. You will probably have to wrap events for some subsystems.

### Game Entities
Game entities are structs that contain whatever information you need, plus data for each subsystem you're using. Thus, if you are using the 2D animation system, your struct should have a `Sprite` and implement the `get-sprite` functon. If you are also using the 2D collision-detection system, you should have a `Collider` and a implement the `get-collider` function. Entities are aggregated in a map of IDs to Entities. You can pass this map directly to each subsystem's `update` function, as long as it implements the appropriate getter. Thus, if you pass your Entities map to `Animate2D.update`, what you will get back is a map of entities with every sprite advanced appropriately.

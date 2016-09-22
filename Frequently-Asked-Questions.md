* [Why Decoupled instead of ReadyValid?](#why-decoupled-instead-of-readyvalid)
* [Why do I have to wrap `Data` in `Wire(...)`?](#why-do-i-have-to-wrap-data-in-wire)
* [Why do I have to wrap module instantiations in `Module(...)`?](#why-do-i-have-to-wrap-module-instantiations-in-module)
* [Why Chisel?](#why-chisel)

### Why Decoupled instead of ReadyValid?

There are multiple kinds of Ready/Valid interfaces that impose varying restrictions on the producers and consumers. Gives static type safety between these interfaces.

### Why do I have to wrap `Data` in `Wire(...)`?
### Why do I have to wrap module instantiations in `Module(...)`?
### Why Chisel?
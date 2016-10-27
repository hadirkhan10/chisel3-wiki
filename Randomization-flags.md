In many Verilog simulators, undefined behaviors such as reading from uninitialized registers or memories, performing logic on undriven wires, or reading from a bad index in a memory will result in X values propagating in the simulation.

If this is undesirable, the Chisel-generated verilog contains randomization code guarded by pre-processor flags. The flags available are

 * RANDOMIZE_REG_INIT - randomly initialize the values of registers
 * RANDOMIZE_MEM_INIT - randomly initialize the contents of memories
 * RANDOMIZE_GARBAGE_ASSIGN - if a read from memory uses an address not existing in the memory (e.g. read address 3 from a 3-entry memory), produce a random value
 * RANDOMIZE_INVALID_ASSIGN - if a net is not connected, drive it with random values
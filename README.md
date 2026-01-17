# pfforth-machine

Hardware Forth stack machine implementation for ULX3S FPGA with asynchronous execution and ESP32 integration.

## Project Overview

PFForth Machine is a pure hardware Forth implementation targeting the ULX3S FPGA (ECP5-based development board). Unlike traditional CPU-based Forth interpreters, this project implements Forth primitives directly in hardware with asynchronous execution—meaning the machine processes instructions without requiring a global clock.

### Key Features

- **Pure Hardware Forth Machine**: Forth primitives implemented directly in hardware
- **Asynchronous Execution**:  Instruction-after-instruction processing without clock synchronization
- **ULX3S FPGA Target**: Based on Lattice ECP5 FPGA
- **Dual Stack Architecture**: 
  - Data stack (32 cells)
  - Return stack (32 cells)
- **Memory**:
  - RAM: 250 KB
  - Flash: 512 KB
- **Peripherals**:
  - WiFi (via on-board ESP32)
  - UART (serial communication)
  - SPI (external devices)
  - GPIO (sensors/IO)
  - ESP32 Integration

### Architecture

```
┌─────────────────────────────────────────────────┐
│         ASYNCHRONOUS FORTH MACHINE              │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌──────────────┐      ┌──────────────┐        │
│  │ Data Stack   │      │ Return Stack │        │
│  │ (32 cells)   │      │ (32 cells)   │        │
│  └──────────────┘      └──────────────┘        │
│         ▲                     ▲                 │
│         └─────────┬───────────┘                │
│                   │                            │
│           ┌───────┴────────┐                   │
│           │ Hardware       │                   │
│           │ Primitives     │                   │
│           │ (30+ ops)      │                   │
│           └────────────────┘                   │
│                   │                            │
│    ┌──────────────┼──────────────┐             │
│    │              │              │             │
│  ┌─▼─┐ ┌────────┐ ┌──────────┐ ┌─▼──┐         │
│  │RAM│ │ Instr  │ │ Flash    │ │ PC │         │
│  │250K││ RAM    │ │ 512K     │ │    │         │
│  └───┘ │~250K  │ │          │ └────┘         │
│        └────────┘ └──────────┘                │
│                                                 │
│    (Asynchronous - No Global Clock)            │
└─────────────────────────────────────────────────┘
```

## Project Structure

```
pfforth-machine/
├── README.md                 # This file
├── LICENSE                   # MIT License
├── . gitignore               # Git ignore rules
│
├── docs/                    # Documentation
│   ├── ARCHITECTURE.md      # Hardware architecture design
│   ├── ISA.md              # Instruction Set Architecture
│   ├── MEMORY_MAP.md       # Memory layout specification
│   └── PRIMITIVES.md       # Forth primitives specification
│
├── verilog/                 # Verilog HDL source code
│   ├── top_module.v        # Top-level module
│   ├── stack. v             # Stack implementation
│   ├── primitives/         # Primitive implementations
│   │   ├── arithmetic.v    # +, -, *, /
│   │   ├── stack_ops.v     # DUP, DROP, SWAP, etc.
│   │   ├── memory.v        # @, !  operations
│   │   └── control.v       # IF, THEN, DO, LOOP
│   ├── memory/             # Memory controllers
│   │   ├── ram_ctrl.v      # RAM interface
│   │   └── flash_ctrl.v    # Flash interface
│   └── peripherals/        # I/O interfaces
│       ├── uart.v          # UART controller
│       ├── spi. v           # SPI controller
│       └── esp32_if.v      # ESP32 interface
│
├── tools/                   # Development tools
│   ├── compiler/           # Forth to bytecode compiler
│   │   └── forthc.py       # Python compiler
│   ├── assembler/          # Assembler
│   │   └── asm.py          # Assembler tool
│   └── simulator/          # Hardware simulator
│       └── sim. py          # Simulation tool
│
└── tests/                   # Test files and examples
    ├── simple. fs           # Simple Forth examples
    └── test_primitives.fs  # Primitive tests
```

## Getting Started

### Prerequisites

- **Hardware**: ULX3S FPGA development board
- **Software**:
  - Verilog HDL development tools (Yosys, nextpnr, or similar)
  - Python 3.7+ (for tools)
  - Git

### Installation

1. Clone this repository: 
```bash
git clone https://github.com/Pfontvilanova/pfforth-machine.git
cd pfforth-machine
```

2. Install development tools (as per your FPGA toolchain setup)

3. Review the documentation in `docs/` folder

### Usage

*Details to be added as project develops*

## Specifications

### Forth Primitives (30+)

Stack operations, arithmetic, memory access, control flow, and I/O operations.  See `docs/PRIMITIVES.md` for complete list.

### Memory Map

- **RAM**: 250 KB (data and stacks)
- **Flash**: 512 KB (program code)
- **Register file**: Stack pointers, program counter

See `docs/MEMORY_MAP.md` for detailed layout.

## Project Roadmap

- [ ] Define Instruction Set Architecture (ISA)
- [ ] Implement stack modules (asynchronous push/pop)
- [ ] Implement basic primitives (arithmetic, stack ops)
- [ ] Implement memory interface (@, ! operations)
- [ ] Implement control flow (IF/THEN, DO/LOOP)
- [ ] Develop Forth compiler/assembler
- [ ] Implement UART interface
- [ ] Implement SPI interface
- [ ] Integrate ESP32 WiFi support
- [ ] Create test suite
- [ ] Documentation and tutorials

## Contributing

Contributions are welcome! Please feel free to fork this repository and submit pull requests. 

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

- **Pfontvilanova** - Project creator and maintainer

## Support

For questions, issues, or discussions about this project, please open an issue on GitHub. 

---

**Status**: Early Development 🚀
```

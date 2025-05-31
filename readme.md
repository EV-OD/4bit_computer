# 4-Bit Custom Microprocessor — Instruction Set

Welcome to the official documentation for a custom 4-bit microprocessor. This system is designed with minimalism and clarity in mind, ideal for education, simulations, or hobbyist CPU development.

---

## Architecture Overview

- **4-bit data bus**
- **8-bit instruction memory**
- **4-bit general-purpose registers**: A, B, C
- **Separated Program and Data RAM**
- **I/O-mapped architecture**
- **Max 16 core opcodes due to 4-bit limit**  
  > ⚠️ Extension mechanism under development to allow more instructions (e.g. `JNZ`, `CMP`, etc.)

---

## 📦 Register Summary

| Register | Description         |
|----------|---------------------|
| `A`      | Accumulator         |
| `B`      | Operand (used in ALU) |
| `C`      | Operand (used in ALU) |

---

## 📘 Instruction Set

### 🔹 Core ALU Operations (4-bit)

All operations are performed as: `A = B OP C` (except `NOT`)

| Mnemonic | Opcode | Operation        |
|----------|--------|------------------|
| `ADD`    | `1000` | `A = B + C`      |
| `SUB`    | `1001` | `A = B - C`      |
| `AND`    | `1010` | `A = B & C`      |
| `OR`     | `1011` | `A = B | C`      |
| `XOR`    | `1100` | `A = B ^ C`      |
| `NOT`    | `1101` | `A = ~B`         |

> ℹ️ ALU operates only on registers B and C (or B for NOT).

---

### 🔹 Data Movement

| Mnemonic    | Format         | Description                  |
|-------------|----------------|------------------------------|
| `MOV A, imm`| `0101 iiii`    | Load 4-bit immediate into A  |
| `MOV B, A`  | `0110`         | Copy A → B                   |
| `MOV C, A`  | `0111`         | Copy A → C                   |

---

### 🔹 Memory Instructions

| Mnemonic | Format         | Description                          |
|----------|----------------|--------------------------------------|
| `STA addr`| `0100 aaaa`   | Store A to memory address (4-bit)    |
| `LDA addr`| `0011 aaaa`   | Load from memory → A                 |
| `NOP`     | `0000`        | No operation *(also required after STA)* |

> 🧠 Memory and Program space are **separate**.

---

### 🔹 I/O Instructions

| Mnemonic | Format         | Description                     |
|----------|----------------|---------------------------------|
| `IN addr`| `0010 aaaa`    | Read from I/O port → A          |
| `OUT addr`| `0001 aaaa`   | Write A → I/O port              |

> I/O uses 4-bit address space (`0000`–`1111`)

---

### 🔹 Flow Control

| Mnemonic | Format             | Description                  |
|----------|--------------------|------------------------------|
| `JMP`    | `1111 aaaaaaaa`    | Unconditional jump to 8-bit address |

> Future instructions like `JNZ`, `JC`, `JZ` can be added once architecture is expanded.

---

## 💡 Example Program

### 📋 Task

- Read input from I/O port `4`
- Add `2`
- Output result to port `1`
- Store result to memory address `0x0`
- Repeat forever

### 🔤 Assembly

```asm
IN 4          ; Read from I/O[4] → A
MOV B, A      ; A → B

MOV A, 2      ; Load 2 → A
MOV C, A      ; A → C

ADD           ; A = B + C

OUT 1         ; Output A → I/O[1]

STA 0         ; Store A → memory[0x0]
NOP           ; Required after STA

JMP 0         ; Loop back
```

## 🔧 Reserved for Future Instructions

```
| Mnemonic | Opcode | Description        |
|----------|--------|--------------------|
| JNZ      | TBD    | Jump if not zero   |
| CMP      | TBD    | Compare A & value  |
```

> Additional instructions will be implemented by extending the architecture beyond the current 4-bit opcode space.
---

## 🧪 Development Notes

```
- STA requires a following NOP due to memory write latency.
- Opcodes are 4-bit limited (max 16 instructions), which restricts expansion.
- A new instruction encoding architecture is under development to enable:
  - Conditional jumps (JNZ, JZ, JC)
  - Comparisons and calls
  - More advanced I/O features
- Program and data memory are independently addressed.
- All I/O and memory addresses are 4-bit wide (0–15).
```

## 🧠 Credits

```
Designed and implemented by: Rabin Lamichhane
GitHub:    https://github.com/EV-OD
LinkedIn:  https://www.linkedin.com/in/rabinlc01/
```

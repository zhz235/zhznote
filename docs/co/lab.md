# lab record
## 内联汇编
```
static uint64_t add_sp(uint64_t addend, uint64_t *dst) {
  uint64_t retval;
  asm volatile("mv a0, sp\n"
               "mv a1, %[arg]\n"
               "add %[ret], a0, a1\n"
               "sd %[ret], 0(%[dst])\n"
               "mv a0, %[ret]\n"
               "call puti\n"
               : [ret] "=r"(retval)
               : [arg] "r"(addend), [dst] "r"(dst)
               : "ra", "a0", "a1", "memory");
  return retval;
}
```


## RISCV汇编
- CSR寄存器：用于控制和管理 RISC-V 处理器的状态、异常、中断等重要信息的特殊寄存器。
- 访问 CSR：通过专门的指令（如 csrr、csrw、csrs、csrc）来读取、写入或修改这些寄存器。
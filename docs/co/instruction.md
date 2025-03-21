# Instruction: Language of Computer

##  
Main memory used for composite data
- arrays,structures,dynamic,data
- 只有load,store可以访问memory
memory is byte addressed
risc-v is little endian(小端)
0x12345678 ->3,2,1,0  ; 78->0

word aligned（对齐）: 地址按字节排列，第一个地址0，第二个地址4······

RISC-v不需要word aligned

## register vs memory
registers are faster than memory

operating on memory data requires loads and stores

compiler must use registers for variables as much as possible

32个寄存器，x0~x31

and used to mask

signed:blt bge
unsigned: bltu,bgeu

jalr x1,100(x6):把pc+4存在x1里面,然后跳到x6+100  callee
jal x1,ProcedureAddress:  caller
x1常做ra
t和s

basic block：顺序执行，没有跳转指令

lui:把立即数放到寄存器高位，低位填0（20位）

branch指令是相对位置跳转，立即数最后一位为0

基址寻址，pc相对寻址，
 
auipc rd,immed : rd = (imm<<12)+pc
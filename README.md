# Processador RISC-V Educacional

## 📋 Visão Geral
Este projeto implementa um processador RISC-V 32-bit educacional desenvolvido no Logisim. O processador suporta um subconjunto significativo da ISA RISC-V e é ideal para fins de aprendizado em arquitetura de computadores.

- Confira o vídeo explicativo **[aqui](https://youtu.be/Gh65pxwISRk)**!
---
## 🏗️ Especificações Técnicas
- Arquitetura: RISC-V 32-bit
- Barramento de dados: 32 bits
- Barramento de endereços: 24 bits (PC), 32 bits (dados)
- Tamanho da instrução: 32 bits
- Quantidade de registradores: 32 (x0-x31)
- Memória de instruções: ROM com palavras de 32 bits
- Memória de dados: RAM com palavras de 32 bits (acessível por byte)
---
## 👥 Integrantes do Projeto
| Nome                             |     RA    |
|----------------------------------|-----------|
| Aline Cristina Ribeiro de Barros | 081230021 |
| Luis Gustavo de Oliveira Carneiro| 081230029 |
| João Victor Pereira Andrade      | 081230010 |
| André Mendes Garcia              | 081230012 |
| Roger Rocha da Silva             | 081230045 |
---
## 🔧 Arquitetura do Datapath
Componentes:
### 1. Instruction Fetch
- PC: Program Counter de 24 bits
- Instruction Memory: ROM de instruções
### 2. Instruction Decode (ID)
- Instruction Decoder: Separa campos da instrução (opcode, funct3, funct7, registradores)
- Register File: 32 registradores de 32 bits
- Immediate Generator: Gera valores imediatos de 32 bits para múltiplos formatos
- Control Unit: Gera sinais de controle baseados na instrução
### 3. Execution (EX)
- ALU: Unidade Lógica Aritmética com 10 operações
- Data Memory: Memória RAM para dados
- Load Control Unit: Tratamento especializado para diferentes tipos de load

## 🎛️ Unidades de Controle

### Control Unit
Gera sinais baseados no opcode (bits [6:0]) e campos funct:

| Sinal  | Função                          |
|--------|----------------------------------|
| bSel   | Seleciona fonte do operando B (registrador vs imediato) |
| regWe  | Write Enable do Register File   |
| aluSel | Operação da ALU (4 bits)        |
| wbSel  | Seleção Write Back              |
| lSel   | Tipo de load (lb/lbu/lh/lhu/lw) |

### Immediate Generator
Suporta múltiplos formatos de imediatos:

| Tipo | Bits utilizados                    | Uso                |
|------|-------------------------------------|--------------------|
| I    | [31:20] → imm[11:0]               | Instruções imediatas |
| S    | [31:25,11:7] → imm[11:5,4:0]     | Store              |
| B    | [31,30:25,11:8,7] → imm[12,10:5,4:1,11] | Branch           |
| J    | [31,19:12,20,30:21] → imm[20,19:12,11,10:1] | Jump        |
| U    | [31:12] → imm[31:12]              | Upper immediate    |

### ALU Control
Operações suportadas pela ALU:

| ALUCtrl | Operação | Descrição               |
|---------|----------|-------------------------|
| 0000    | add      | Soma                    |
| 0001    | sll      | Shift Left Logical      |
| 0010    | slt      | Set Less Than           |
| 0011    | sltu     | Set Less Than Unsigned  |
| 0100    | xor      | XOR                     |
| 0101    | srl      | Shift Right Logical     |
| 0110    | or       | OR                      |
| 0111    | and      | AND                     |
| 1000    | sub      | Subtração               |
| 1101    | sra      | Shift Right Arithmetic  |

### Load Control Unit
Trata diferentes tipos de load com controle de sinal:

| lSel | Tipo  | Descrição               |
|------|-------|-------------------------|
| 000  | lb    | Load Byte (signed)      |
| 001  | lh    | Load Halfword (signed)  |
| 010  | lw    | Load Word               |
| 100  | lbu   | Load Byte (unsigned)    |
| 101  | lhu   | Load Halfword (unsigned)|

## 💻 Assembly

O Assembly do projeto pode ser convertido em hexadecimal [nesse site](https://riscvasm.lucasteske.dev/#)

## Programa Teste
```asm
addi x5, x0, 10    # x5 = 10
addi x6, x0, 20    # x6 = 20
add x7, x5, x6     # x7 = x5 + x6 = 30

addi x3, x0, 4     # x3 = 4
lb x2, 2(x3)       # x2 = MEM[x3 + 2] = MEM[6] (carrega byte do endereço 6)
```
Em Hexadecimal:
```
00a00293
01400313
006283b3
00400193
00218103
```
Basta jogar o código Hexadecimal na ROM de instrução.

### Valores Esperados
- x5: 10 (0x0000000A)
- x6: 20 (0x00000014)
- x7: 30 (0x0000001E)
- x3: 4 (0x00000004)
- x2: Valor em MEM[6] (Memória RAM)

## ❗ Limitação
O projeto não implementa funções de set na RAM, apenas Load Byte e Load Half World

---
## 📚 Documentação de Referência
- **[The RISC-V Instruction Set Manual, Volume I: User-Level ISA](https://digitalassets.lib.berkeley.edu/techreports/ucb/text/EECS-2016-1.pdf)** - Documento oficial com a especificação completa do conjunto de instruções RISC-V
- **[RISC-V Green Card](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/notebooks/RISCV/RISCV_GREEN_CARD.pdf)** - Cartão de referência rápida com todas as instruções RISC-V e codificação
---
## 👨‍🏫 Créditos

### Orientação e Base Técnica
- **[Professor Vinícius Borges](https://www.linkedin.com/in/vinicius-borges-07a170153/)** - Pelas aulas de arquitetura de computadores que forneceram a base técnica para este projeto
- **[Chuck Benedict](https://www.youtube.com/@chuckbenedict7235)** - Pelos vídeos instrucionais que serviram como base para a implementação prática



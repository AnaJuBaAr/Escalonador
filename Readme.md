# Simulação de Escalonador de Processos

**Trabalho Prático II — BCC202 · Estrutura de Dados I**

Universidade Federal de Ouro Preto (UFOP) · Instituto de Ciências Exatas e Biológicas (ICEB)
Departamento de Computação (DECOM) · Ciência da Computação

- **Autora:** Ana Júlia Barros Araújo
- **Professores:** Pedro Henrique Lopes Silva e Karla Alexsandra de Souza Joriatti
- **Data:** 5 de fevereiro de 2026

---

## Sobre o projeto

Simulador de escalonador de processos escrito em C, baseado em um modelo de **fila de prioridades** com **tempo máximo de execução por ciclo de CPU (100 ciclos)**.

O programa recebe uma lista de *n* processos, ordena-os por prioridade (e, em caso de empate, por ordem de chegada) e simula a execução na CPU. Ao final, exibe o **log de atendimento**: a ordem em que os processos usaram a CPU, com o ID, a prioridade naquele momento e a quantidade de ciclos que permaneceram executando.

O código é dividido em módulos (`.h` e `.c`), buscando modularidade, clareza e ausência de *memory leaks*.

## Dados de um processo

| Campo | Descrição                                        | Tipo    |
|-------|--------------------------------------------------|---------|
| `ID`  | Número de identificação do processo              | inteiro |
| `T`   | Tempo de chegada (único campo `float` do TAD)    | float   |
| `P`   | Nível de prioridade                              | inteiro |
| `C`   | Ciclos de CPU necessários                        | inteiro |

## Como funciona

1. **Entrada:** o usuário informa a quantidade de processos e, em seguida, os dados de cada um.
2. **Ordenação inicial:** os processos são ordenados com **MergeSort**, por prioridade e, em caso de empate, por tempo de chegada.
3. **Escalonamento:** o processo à frente da fila executa na CPU por no máximo **100 ciclos** por vez, e cada passagem é registrada no log da CPU.
4. **Reinserção:** se o processo não terminou, sua **prioridade é incrementada**, seus ciclos restantes são atualizados e ele volta à fila, na posição correta, por meio de **InsertionSort**. Se terminou, é removido da fila.
5. **Saída:** o log é impresso com a ordem de execução, a prioridade do processo naquele momento e os ciclos usados.

## Estrutura do código

### TADs

- **`Processo`** — guarda `ID`, `T`, `P` e `C`.
- **`FilaProcesso`** — vetor de `Processo` e um inteiro com o tamanho do vetor.

### Principais funções

| Função                | Responsabilidade                                                                                   |
|-----------------------|----------------------------------------------------------------------------------------------------|
| `primeiraEntrada()`   | Lê a quantidade de processos informada pelo usuário                                                |
| `criarFila()`         | Aloca a `FilaProcesso` com o tamanho necessário                                                    |
| `criarProcesso()`     | Chamada por `criarFila()`; aloca o vetor de processos                                              |
| `segundaEntrada()`    | Preenche a fila com os dados informados e chama o MergeSort                                        |
| `MergeSort`           | Ordena o vetor de processos                                                                        |
| `comparar()`          | Critério de ordenação: prioridade primeiro, tempo de chegada como desempate                        |
| `escalonador()`       | Gerencia a execução, limitando cada passagem a 100 ciclos e registrando o log                      |
| `adicionaFila()`      | Move o processo executado para o fim da fila e, se restarem ciclos, reposiciona-o por InsertionSort |
| impressão do log      | Exibe a lista de atendimento da CPU                                                                |

## Compilação e execução

### Compilar

```bash
gcc -c filaprocessos.c -Wall
gcc -c tp.c -Wall
gcc filaprocessos.o tp.o -o exe
```

Opções usadas:

- `-c`: gera o arquivo objeto (`.o`) sem criar o executável final
- `-o`: define o nome do executável
- `-Wall`: ativa todos os avisos do compilador

### Executar

```bash
./exe
```

### Verificar vazamentos de memória

```bash
valgrind --leak-check=full ./exe
```

## Resultados

O programa foi testado com diferentes entradas, incluindo o exemplo do enunciado, e o comportamento observado foi:

- processos de maior prioridade são executados primeiro;
- processos com a mesma prioridade são ordenados pelo tempo de chegada;
- cada passagem pela CPU é limitada a 100 ciclos, com reinserção na fila e incremento de prioridade;
- ausência de *memory leaks* confirmada com o Valgrind.

## Ferramentas e ambiente

- **Editor:** Visual Studio Code
- **Linguagem:** C
- **Análise estática:** GCC
- **Análise dinâmica:** Valgrind
- **Documentação:** Overleaf (LaTeX)
- **Máquina de desenvolvimento e testes:** Intel Core i5, 8 GB de RAM, Ubuntu 24.04 LTS

## Dificuldades encontradas

- Integrar a ordenação inicial (MergeSort) com a ordenação de reinserção (InsertionSort) sem perda de desempenho.
- Atualizar corretamente ciclos restantes e prioridades após cada execução parcial.
- Garantir a liberação completa da memória, inclusive nas realocações.
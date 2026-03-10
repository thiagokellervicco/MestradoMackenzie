# Algoritmos de Ordenação

Comparação de desempenho entre algoritmos clássicos e otimizados de ordenação em C#.

**Aluno:** Thiago Keller | **RA:** 10779365 | **Curso:** Mestrado de Computação Aplicada  
**Instituição:** Universidade Presbiteriana Mackenzie | **Professora:** Dra Valeria Farinazzo Martins  
**Ano:** 2026 (1º semestre)

---

## Sobre o Projeto

Este projeto implementa uma comparação de desempenho entre diversos algoritmos de ordenação em C#, gerando relatórios HTML com resultados, gráficos comparativos e análise de complexidade assintótica. Cada benchmark executa **5 iterações** por combinação (algoritmo × tamanho × tipo de entrada), reportando a mediana dos tempos e da memória alocada.

---

## Algoritmos e Complexidade

| Algoritmo | Melhor Caso | Caso Médio | Pior Caso |
|-----------|-------------|------------|-----------|
| Bubble Sort | O(n²) | O(n²) | O(n²) |
| Bubble Sort (Optimized) | O(n) | O(n²) | O(n²) |
| Selection Sort | O(n²) | O(n²) | O(n²) |
| Selection Sort (Double) | O(n²) | O(n²) | O(n²) |
| Insertion Sort | O(n) | O(n²) | O(n²) |
| Insertion Sort (Sentinel) | O(n) | O(n²) | O(n²) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) |
| Merge Sort (Single Aux) | O(n log n) | O(n log n) | O(n log n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) |
| Quick Sort (Pivô Aleatório) | O(n log n) | O(n log n) | O(n²) |
| Quick Sort (Hoare + Median + Insertion) | O(n log n) | O(n log n) | O(n²) |
| Array.Sort (C# Nativo) | O(n log n) | O(n log n) | O(n log n) |

### Tipos de Entrada

- **Aleatório** – Vetores com elementos pseudoaleatórios
- **Crescente** – Dados já ordenados
- **Decrescente** – Dados em ordem inversa

---

## Pré-requisitos

- [.NET 9 SDK](https://dotnet.microsoft.com/download) ou superior

---

## Como Executar

> **Importante:** Para resultados com otimizações reais do JIT, use sempre o modo **Release** (`-c Release`).

### Benchmark completo em Release (recomendado)

```bash
dotnet run -c Release --project src/AlgoritmosOrdenacao -- --optimized
```

### Outros comandos

```bash
# Demonstração no console (10 elementos, entrada e saída)
dotnet run --project src/AlgoritmosOrdenacao -- --demo

# Relatório Demo (10 elementos) → docs/Relatorio-Demo.html
dotnet run --project src/AlgoritmosOrdenacao -- --demo-report

# Teste completo (1.000, 10.000, 100.000 elementos)
dotnet run --project src/AlgoritmosOrdenacao

# Com algoritmos otimizados
dotnet run --project src/AlgoritmosOrdenacao -- --optimized

# Modo rápido (1.000 e 10.000 elementos)
dotnet run --project src/AlgoritmosOrdenacao -- --rapido

# Modo ultra (inclui 10M elementos, apenas O(n log n))
dotnet run --project src/AlgoritmosOrdenacao -- --ultra

# Atualizar apenas o índice de relatórios em docs/
dotnet run --project src/AlgoritmosOrdenacao -- --index-only
```

### Por que o modo Release é fundamental?

- **Otimização de Código:** No modo Debug, o compilador mantém o código "literal" para facilitar o rastreio. No Release, reescreve partes do algoritmo para rodar mais rápido.
- **Performance do GC:** O Garbage Collector se comporta de forma mais agressiva e eficiente em Release.
- **Remoção de NOP:** O modo Debug insere instruções vazias para breakpoints, adicionando latência visível em vetores grandes.

### Por que o modo ultra usa apenas O(n log n) em 10M elementos?

Algoritmos O(n²) tornam-se impraticáveis com vetores muito grandes:

| Tamanho | O(n log n) | O(n²) |
|---------|------------|-------|
| 100.000 | segundos | minutos |
| 10.000.000 | poucos min | horas/dias |

Para 10 milhões de elementos, O(n²) significa ~100 trilhões de operações. O modo ultra executa apenas Merge Sort, Quick Sort e Array.Sort nesse cenário.

---

## Estrutura do Projeto

```
├── docs/                       # Relatórios HTML (GitHub Pages)
│   ├── index.html              # Página inicial
│   └── Relatorio-*.html        # Relatórios gerados
├── src/AlgoritmosOrdenacao/
│   ├── Algoritmos/             # Algoritmos clássicos
│   ├── AlgoritmosOtimizados/   # Versões otimizadas
│   ├── Core/                   # BenchmarkRunner, DataGenerator
│   ├── Relatorio/              # ReportGenerator, MachineInfo
│   └── Program.cs
└── README.md
```

---

## Relatórios

A página inicial (`docs/index.html`) contém informações da máquina, tabela de complexidade, descrição dos algoritmos otimizados e comandos de benchmark. Cada relatório HTML inclui:

- Estado da execução (fonte de energia, memória GC)
- Tabelas com tempos (mediana, mínimo, máximo) e memória
- Gráficos comparativos (Chart.js, escala linear em segundos)

---

## Referências

- Cormen, T. H., et al. *Introduction to Algorithms* (3ª ed.), MIT Press.
- Sedgewick, R. *Algorithms* (4ª ed.). Mediana de três no Quick Sort.
- Documentação .NET – [Array.Sort](https://learn.microsoft.com/dotnet/api/system.array.sort) (IntroSort).

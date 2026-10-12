# Mini_Linguagem_de_Programa-o_usando_Jflap
DataLang: mini linguagem para análise de dados, com análise léxica por expressões regulares e AFDs implementados no JFLAP. Trabalho de Linguagens Formais e Compiladores (UEPA).

# DataLang

Mini linguagem de programação para **análise de dados**, com análise léxica modelada por **expressões regulares** e **autômatos finitos determinísticos (AFDs)** implementados e testados no **JFLAP**.

> Trabalho avaliativo (AV1) de **Linguagens Formais e Compiladores**
> Bacharelado em Engenharia de Software, Universidade do Estado do Pará (UEPA)
> Professor: Leno Martins · Apresentação: 13/10/2026

## Equipe

- Davi dos Santos CHaves
- João Pedro Carvalho de Mesquita
- Kauan Kawasaki
- Eduardo Lucas Guimarães Beckman

## Finalidade

A DataLang permite escrever scripts curtos para carregar tabelas, filtrar linhas e resumir valores, sem exigir uma linguagem de propósito geral. Exemplo de programa:

```
carregar vendas;
filtrar vendas onde preco > 99.9;
somar preco;
```

## Alfabeto (Σ)

| Grupo | Símbolos |
|---|---|
| Letras minúsculas | `a–z` |
| Dígitos | `0–9` |
| Símbolos | `_ . + - * / = < > ! ( ) ; ,` |
| Espaçamento | espaço e quebra de linha |

Maiúsculas e símbolos fora de Σ (como `@`, `#`, `$`) geram **erro léxico**.

## Tokens e regras léxicas

| Categoria | Regra | Exemplos |
|---|---|---|
| Palavra reservada | 10 palavras fixas | `carregar`, `filtrar`, `agrupar`, `ordenar`, `somar`, `media`, `contar`, `exibir`, `salvar`, `onde` |
| Identificador | letra minúscula ou `_`, seguida de letras, dígitos ou `_` | `idade`, `total_vendas`, `_tmp` |
| Constante numérica | inteiro sem zeros à esquerda, com parte decimal opcional | `0`, `42`, `3.14` |
| Operador | aritméticos, relacionais e atribuição | `+ - * / = < > <= >= == !=` |
| Delimitador | agrupam e separam | `( ) ; ,` |

**Prioridade:** vale o maior casamento; em caso de empate, palavra reservada vence identificador (por exemplo, `media` é reservada, não identificador).

## Expressões regulares

| Estrutura | Expressão regular | Válidos | Inválidos |
|---|---|---|---|
| Identificador | `[a-z_][a-z0-9_]*` | `idade`, `total_vendas`, `_tmp` | `1coluna`, `Total`, `preco-unitario` |
| Constante numérica | `(0\|[1-9][0-9]*)(\.[0-9]+)?` | `0`, `42`, `3.14` | `007`, `3.`, `.5`, `1.2.3` |
| Palavra reservada | `carregar\|filtrar\|agrupar\|ordenar\|somar\|media\|contar\|exibir\|salvar\|onde` | `media`, `onde`, `salvar` | `carregue`, `Media`, `mediana` |

## Autômatos (JFLAP)

| Arquivo | Reconhece | Estados |
|---|---|---|
| `jflap/afd_identificadores.jff` | identificadores | 2 |
| `jflap/afd_constantes_numericas.jff` | inteiros e decimais | 5 |
| `jflap/afd_palavras_reservadas.jff` | as 10 palavras reservadas (AFD em árvore) | 59 |

Cada autômato tem um arquivo de entradas de teste correspondente, com palavras aceitas e rejeitadas:
`jflap/entradas_identificadores.txt`, `jflap/entradas_constantes_numericas.txt` e `jflap/entradas_palavras_reservadas.txt`.

### Como executar

1. Instale o [JFLAP](https://www.jflap.org/) (requer Java).
2. Abra o JFLAP e escolha **File › Open** para carregar um arquivo `.jff` da pasta `jflap/`.
3. Para testar uma palavra: **Input › Step...** e digite a entrada.
4. Para testar várias de uma vez: **Input › Multiple Run**, depois **Load Inputs** e selecione o `.txt` correspondente.

### Como identificar erros léxicos

Uma entrada é **rejeitada** quando:

- chega a um símbolo sem transição definida a partir do estado atual (por exemplo, `Total` ou `1coluna` no AFD de identificadores); ou
- a entrada termina em um estado **não final** (por exemplo, `medi` no AFD de palavras reservadas).

Observe que `mediana` é rejeitada pelo AFD de palavras reservadas, mas aceita pelo AFD de identificadores.

## Estrutura do repositório

```
.
├── README.md
├── src/
│   ├── afd_identificadores.jff
│   ├── afd_constantes_numericas.jff
│   └── afd_palavras_reservadas.jff
└── testes/
    ├── entradas_identificadores.txt
    ├── entradas_constantes_numericas.txt
    └── entradas_palavras_reservadas.txt
```

## Licença

[Defina a licença, por exemplo MIT]

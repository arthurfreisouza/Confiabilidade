# Opções de tema — Trabalho Prático de Confiabilidade de Sistemas

Duas opções da lista de exemplos do enunciado, ambas sem protótipo físico e sem coleta
de dados de falha. O estudo é analítico nas duas.

## Resumo

| | Opção 1: Fonte linear ajustável | Opção 2: Sistema de pagamentos online |
|---|---|---|
| Tipo de sistema | Circuito eletrônico, não reparável | Serviço de software, reparável |
| Método principal | Contagem das partes (MIL-HDBK-217) | Diagrama de blocos (RBD) |
| Aula de base | 9a | 9b |
| Índices | λ, R(t), MTTF | Disponibilidade A, indisponibilidade em horas/ano |
| Origem dos números | Tabelas da norma | SLAs publicados pelos provedores |
| Ferramenta | Planilha ou script curto | Planilha ou script curto |
| Ponto fraco | Norma de 1995, taxa de falha constante | SLA é compromisso contratual, não taxa medida |
| Recomendação | **Primeira escolha** | Se o grupo preferir software |

---

## Opção 1 — Fonte linear ajustável

### O sistema

Uma fonte de bancada simples, a partir de um esquemático já existente (por exemplo, o
circuito de aplicação do regulador LM317 no datasheet). Nada é montado nem medido.

```
Rede AC -> [Transformador] -> [Ponte retificadora] -> [Capacitor de filtro]
        -> [Regulador ajustável + resistores + potenciômetro] -> Saída DC
```

Todos os componentes estão em série do ponto de vista da confiabilidade: se um falha,
a fonte falha.

### O que será feito

1. Escolher o esquemático e listar os componentes (tipo e quantidade).
2. Para cada tipo, buscar no MIL-HDBK-217 a taxa de falha base λ_b e o fator de
   qualidade π_Q.
3. Calcular a taxa de falha do sistema pela contagem das partes (aula 9a):

   λ = Σ Nᵢ · π_Qᵢ · λ_bᵢ

4. Calcular os índices (aula 3):
   - R(t) = exp(−λt)
   - MTTF = 1/λ
5. Montar o ranking de contribuição: quanto cada componente responde pela λ total.
6. Propor uma melhoria no componente dominante (qualidade superior ou operação com
   folga) e recalcular, comparando antes e depois.

### Resultados a apresentar

- Tabela de componentes com N, π_Q, λ_b e a contribuição percentual de cada um.
- Curva R(t) da fonte original e da fonte melhorada.
- MTTF das duas versões.

### Vantagens

- As taxas de falha vêm da norma, que é pública e citável.
- A conta é uma soma ponderada; qualquer integrante explica.
- O sistema é pequeno e o escopo é fechado.

### Limitações a declarar no texto

- A norma assume componentes em série e taxa de falha constante.
- Os valores são de 1995 e nem sempre representam componentes atuais.
- Não considera transitórios, variação de temperatura nem manutenção (a própria aula 9a
  lista esses pontos de atenção).

### Perguntas prováveis na arguição

- Por que a taxa de falha é constante? O que isso significa na curva da banheira?
- O que é o fator de qualidade π_Q?
- Qual a diferença entre contagem das partes e estresse das partes?
- O MTTF obtido é realista? Por quê?

---

## Opção 2 — Sistema de pagamentos online

### O sistema

A cadeia de serviços que precisa estar funcionando para um pagamento ser concluído,
hospedada em nuvem.

```
Sistema base (série):

[Balanceador] -> [Servidor de aplicação] -> [Banco de dados] -> [Gateway de pagamento]

Sistema melhorado (redundância no bloco mais fraco, por exemplo a aplicação):

                  +-> [Aplicação A] -+
[Balanceador] ->  |                  | -> [Banco de dados] -> [Gateway de pagamento]
                  +-> [Aplicação B] -+
```

### O que será feito

1. Definir a arquitetura e desenhar o diagrama de blocos.
2. Levantar a disponibilidade de cada bloco nos SLAs publicados pelos provedores,
   guardando o link de cada um.
3. Calcular a disponibilidade do sistema em série (aula 9b):

   A_sys = ∏ Aᵢ

4. Converter em indisponibilidade anual: horas paradas = (1 − A_sys) × 8760.
5. Identificar o bloco que mais contribui para a indisponibilidade.
6. Adicionar redundância ativa nesse bloco e recalcular:

   A_paralelo = 1 − ∏ (1 − Aᵢ)

7. Opcional: simulação de Monte Carlo (aula 7) para conferir o resultado analítico.

### Resultados a apresentar

- Tabela com a disponibilidade de cada bloco e a fonte.
- Disponibilidade e horas paradas por ano, com e sem redundância.
- Ranking dos blocos por contribuição para a indisponibilidade.

### Vantagens

- Nenhuma eletrônica; a arquitetura é familiar para quem já trabalhou com software.
- O resultado em "horas paradas por ano" é fácil de comunicar na apresentação.
- Aceita uma extensão simples (Monte Carlo) que conta em "complexidade do problema".

### Limitações a declarar no texto

- SLA é o mínimo que o provedor se compromete a entregar, não a disponibilidade medida.
- As premissas da aula 9b são otimistas: falhas independentes, sem modo de falha comum,
  chaveamento da redundância perfeito, reparo imediato.
- Falhas de software (bugs, erros de implantação) não aparecem nos SLAs de
  infraestrutura.

### Perguntas prováveis na arguição

- Por que usar SLA como disponibilidade? Qual o erro dessa aproximação?
- O que acontece se os dois servidores redundantes falharem pela mesma causa?
- Qual a diferença entre confiabilidade e disponibilidade?
- Como ficaria com redundância standby em vez de ativa?

---

## Como decidir

- Se alguém do grupo tem familiaridade com circuitos: **Opção 1**. É a mais defensável,
  porque os números têm origem em norma.
- Se o grupo é mais de software: **Opção 2**. Exige assumir abertamente a limitação dos
  SLAs.

Nas duas, o enunciado exige a declaração de uso de IA e que todos os integrantes saibam
explicar o trabalho inteiro.

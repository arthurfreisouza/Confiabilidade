# Trabalho Prático — Confiabilidade de Sistemas (EEE 017)

Documento de planejamento do grupo. Explica o que vamos fazer, por quê e como.
Não é o texto da proposta nem do relatório: esses precisam ser escritos pelo grupo.

## 1. Ideia em uma frase

Modelar um sistema real pequeno por **diagrama de blocos de confiabilidade (RBD)**,
calcular sua **confiabilidade e disponibilidade**, identificar o bloco mais crítico e
mostrar quanto uma **redundância** nesse bloco melhora os índices.

## 2. Por que este projeto

- O enunciado aceita estudo analítico, numérico ou com dados históricos. O analítico
  dispensa a coleta e limpeza de dados de falha.
- As fórmulas necessárias estão todas na aula 9b (diagrama de blocos).
- A matemática é produto e soma de probabilidades, então qualquer integrante consegue
  explicar na arguição.
- Entrega mais de um índice de confiabilidade (o enunciado exige pelo menos um).

## 3. Sistema estudado

> **A definir pelo grupo.** O ideal é um sistema que alguém conheça de estágio, IC ou
> outra disciplina. O exemplo abaixo serve de referência e pode ser trocado sem mudar
> o método.

Exemplo de referência: **acesso à internet de um pequeno escritório**.

```
Sistema base (tudo em série):

[Energia] -> [Link do provedor] -> [Modem/ONT] -> [Roteador] -> [Switch]

Sistema melhorado (redundância no acesso):

              +-> [Link A] -> [Modem A] -+
[Energia] ->  |                          | -> [Roteador] -> [Switch]
              +-> [Link B] -> [Modem B] -+
```

Critérios para escolher o sistema:

- 4 a 6 blocos, com dependência funcional clara (se o bloco falha, o sistema falha).
- Componentes com MTBF e MTTR encontráveis em datasheet, norma ou artigo.
- Um ponto fraco evidente, onde faça sentido propor redundância.

## 4. Índices que vamos calcular

| Índice | Significado | Onde está nos slides |
|---|---|---|
| R(t) | Probabilidade de funcionar sem falha até o tempo t | Aula 3 |
| MTTF / MTBF | Tempo médio até a falha / entre falhas | Aula 3 |
| A | Disponibilidade em regime permanente | Aulas 3 e 9b |
| U = 1 − A | Indisponibilidade (convertida em horas paradas por ano) | Aula 9b |

## 5. Método

### 5.1 Dados de entrada

Para cada bloco, levantar:

- taxa de falha λ = 1/MTBF;
- taxa de reparo μ = 1/MTTR;
- a **fonte** de cada valor (datasheet, MIL-HDBK-217, Telcordia SR-332, artigo).

| Bloco | MTBF (h) | MTTR (h) | Fonte |
|---|---|---|---|
| ... | a levantar | a levantar | a levantar |

Nenhum valor pode ser inventado: cada número precisa de uma referência citável.

### 5.2 Fórmulas (aula 9b)

Um bloco, com taxa de falha constante:

- R(t) = exp(−λt)
- A = μ / (λ + μ) = MTBF / (MTBF + MTTR)

Blocos em série:

- R_sys(t) = ∏ R_i(t) = exp(−(Σλ_i) t)
- MTTF_sys = 1 / Σλ_i
- A_sys = ∏ A_i

Blocos em paralelo (redundância ativa):

- R_sys(t) = 1 − ∏ (1 − R_i(t))
- A_sys = 1 − ∏ (1 − A_i)

O sistema melhorado é resolvido por redução: primeiro o ramo em série de cada caminho,
depois o paralelo entre os caminhos, depois a série com o restante.

### 5.3 Premissas

As mesmas listadas na aula 9b, que precisam aparecer no texto como limitações:

- taxas de falha e de reparo constantes (distribuição exponencial);
- falhas independentes, sem modo de falha comum;
- chaveamento da redundância perfeito;
- falha detectada e reparo iniciado imediatamente; reparo deixa o item "como novo".

### 5.4 Validação por Monte Carlo (opcional, recomendada)

Simular o sistema (aula 7): sortear tempos até falha e de reparo de cada bloco, verificar
o estado do sistema ao longo do tempo e estimar R(t) e A. O resultado deve bater com o
analítico. Custa pouco código e aumenta a "complexidade do problema", que é critério de
avaliação da apresentação.

## 6. Resultados esperados

1. Curva R(t) do sistema base e do sistema melhorado, no mesmo gráfico.
2. Tabela com MTTF, A e horas de indisponibilidade por ano nas duas configurações.
3. Ranking dos blocos por contribuição para a indisponibilidade (qual é o gargalo).
4. Comparação analítico × Monte Carlo, se a validação for feita.

## 7. Entregas e prazos

| Entrega | Prazo | Formato |
|---|---|---|
| Proposta | 06/11/2026, 23:55 | PDF, até 3 páginas em coluna dupla (ou 6 em simples) |
| Documento final | 25/11/2026, 20:55 | PDF, até 6 páginas em coluna dupla (ou 12 em simples) |
| Apresentação | 25/11 ou 30/11 | 10 min + 5 min de perguntas, slides em PDF |

Template recomendado: conferência IEEE.

Seções da proposta: título, resumo, introdução, descrição do sistema, revisão da
literatura, metodologia (com declaração de uso de IA), resultados esperados, referências.

O documento final acrescenta resultados e conclusão.

## 8. Divisão de tarefas (sugestão)

| Tarefa | Responsável |
|---|---|
| Escolha e descrição do sistema, diagrama de blocos | |
| Levantamento de MTBF/MTTR com fontes | |
| Revisão da literatura | |
| Cálculo analítico e gráficos | |
| Simulação de Monte Carlo | |
| Texto e slides | |

Todos precisam entender o trabalho inteiro: a arguição pode ser feita a qualquer membro.

## 9. Cronograma

| Período | Atividade |
|---|---|
| até 16/10 | Definir sistema e diagrama de blocos |
| até 30/10 | Levantar taxas e referências; escrever a proposta |
| 06/11 | Entregar a proposta |
| até 16/11 | Cálculos, simulação e gráficos |
| até 23/11 | Documento final e slides; ensaio cronometrado |
| 25/11 | Entregar o documento final; apresentação (25/11 ou 30/11) |

## 10. Uso de IA

Regras do enunciado que afetam o grupo:

- A declaração de uso de IA é **obrigatória** na proposta e no documento final:
  ferramenta, versão, finalidade, etapa e forma de uso.
- Permitido: esclarecer conceitos, revisar texto, gerar e depurar código, buscar
  referências, gerar gráficos.
- Proibido: fabricar dados ou resultados, redigir o relatório integralmente, produzir
  as conclusões.
- Toda referência obtida com IA precisa ser conferida na fonte original.
- Omitir o uso ou não saber explicar um resultado pode zerar a nota.

## 11. Referências de base

- O'Connor, P. D. T.; Kleyner, A. *Practical Reliability Engineering*, 5ª ed., Wiley, 2012.
- Benbow, D. W.; Broome, H. W. *The Certified Reliability Engineer Handbook*.
- Zio, E. *The Monte Carlo Simulation Method for System Reliability and Risk Analysis*.
- Slides da disciplina: aulas 3, 7, 9a, 9b e 10.

A revisão da literatura ainda precisa de artigos sobre o sistema escolhido.

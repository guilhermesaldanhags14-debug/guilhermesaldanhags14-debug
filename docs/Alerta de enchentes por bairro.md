# Alerta de Enchentes por Bairro

Documentação do projeto: definições, planejamento e implementação da v0.1

- **Disciplina:** Fundamentos de Programação
- **Projeto:** Alerta de Enchentes por Bairro
- **Etapas de referência:** Entrega 1 — definições e planejamento; Entrega 2 — implementação e demonstração da v0.1.
- **Equipe:** [Inserir nomes dos integrantes]

> **Nota sobre o escopo:** Este documento apresenta as definições e o planejamento da primeira versão do projeto, além da estrutura para registrar sua implementação. A proposta utiliza pontos historicamente problemáticos cadastrados pela equipe e rotas alternativas simuladas. O sistema não realiza previsões meteorológicas, não monitora o nível da água em tempo real e não emite alertas oficiais de enchentes. As informações têm caráter orientativo e servem como apoio ao planejamento do deslocamento.

---

## 1. Visão Geral do Projeto

- **Nome:** Alerta de Enchentes por Bairro.

- **Contexto e problema:** Durante períodos de chuva, determinadas ruas e regiões dos bairros podem apresentar problemas recorrentes de alagamento. Isso dificulta o deslocamento de pessoas que precisam sair de casa para trabalhar, estudar, utilizar serviços ou realizar outras atividades. Muitas vezes, o usuário conhece seu destino, mas não sabe quais pontos do caminho apresentam histórico de alagamentos. O problema não está apenas na ocorrência das enchentes, mas também na dificuldade de encontrar informações organizadas que ajudem a escolher um trajeto alternativo antes de sair.

- **Usuário principal:** Pessoas que precisam se deslocar entre bairros durante períodos de chuva, utilizando transporte público, carro, bicicleta ou caminhando.

- **Objetivo:** Permitir que o usuário informe seu bairro ou ponto de origem e seu destino, recebendo informações sobre pontos historicamente problemáticos no percurso, alternativas de deslocamento simuladas e orientações para ajudar na escolha de uma rota.

- **Recorte da implementação:** A v0.1 utilizará um mapa simplificado, com pontos de referência e trechos previamente cadastrados. O usuário informará a origem e o destino, e o sistema identificará os caminhos disponíveis, destacando os pontos com histórico de alagamento e apresentando uma alternativa simulada. Não pretende funcionar como um aplicativo de navegação em tempo real.

## 2. História de Usuário

> "Como pessoa que precisa se deslocar durante períodos de chuva, quero informar meu bairro de origem e meu destino para identificar pontos historicamente problemáticos no caminho, conhecer rotas alternativas e receber orientações que me ajudem a planejar meu deslocamento com mais segurança."

## 3. Requisitos Funcionais

Requisitos funcionais descrevem o que a solução deve fazer.

| Código | Requisito | Prioridade para a v0.1 |
|---|---|---|
| RF01 | Exibir os bairros e pontos de referência disponíveis no mapa cadastrado. | Essencial |
| RF02 | Permitir que o usuário informe o ponto de origem e o ponto de destino. | Essencial |
| RF03 | Validar se a origem e o destino existem no mapa e se são diferentes. | Essencial |
| RF04 | Identificar os trechos do percurso que possuem histórico de alagamento. | Essencial |
| RF05 | Exibir o nível de atenção cadastrado para cada ponto problemático (baixo, moderado ou alto). | Essencial |
| RF06 | Identificar e apresentar uma rota alternativa simulada, quando existir. | Essencial |
| RF07 | Exibir orientações para o deslocamento, destacando os pontos de atenção e as limitações das informações. | Essencial |
| RF08 | Informar quando não existir uma rota alternativa cadastrada para o percurso solicitado. | Essencial |
| RF09 | Permitir a simulação de um trecho temporariamente bloqueado para demonstrar como a rota alternativa pode mudar. | Desejável |

## 4. Requisitos Não Funcionais

Requisitos não funcionais descrevem as condições de qualidade e utilização da solução.

| Código | Requisito | Como verificar |
|---|---|---|
| RNF01 | Apresentar todas as informações, mensagens e orientações em português. | Conferir os textos exibidos durante o fluxo principal. |
| RNF02 | Possuir uma interface simples, organizada e de fácil compreensão. | Verificar se o usuário consegue informar origem e destino sem dificuldades. |
| RNF03 | Diferenciar visualmente os níveis de atenção dos pontos cadastrados. | Conferir se os níveis baixo, moderado e alto são identificados corretamente. |
| RNF04 | Apresentar as informações de forma clara, sem afirmar que existe uma previsão de enchente. | Conferir as mensagens e os avisos apresentados ao usuário. |
| RNF05 | Executar o fluxo principal em um navegador, sem necessidade de instalação de programas adicionais. | Abrir a página pelo navegador e realizar uma consulta. |
| RNF06 | Manter os dados dos bairros e trechos separados da lógica de cálculo das rotas. | Alterar um ponto cadastrado e verificar se o cálculo utiliza a nova informação. |
| RNF07 | Informar que os dados representam registros históricos cadastrados e não necessariamente as condições atuais das vias. | Conferir se o aviso aparece junto aos resultados. |

**Escolha tecnológica:** HTML, CSS e JavaScript puro, em uma página que funciona no navegador. Os dados dos bairros, pontos problemáticos e trechos ficarão organizados em um arquivo separado da lógica responsável pela identificação das rotas.

## 5. Regras de Negócio

As regras de negócio descrevem as condições que determinam o comportamento da solução.

### 5.1 Mapa de Referência da v0.1

Para a primeira versão, será utilizado um mapa simplificado com quatro pontos de referência e cinco trechos de ligação. Os nomes e valores abaixo são ilustrativos e deverão ser substituídos pelos locais e dados escolhidos pela equipe.

#### Pontos de referência

| Ponto | Descrição | Histórico de alagamento |
|---|---|---|
| E1 | Ponto de origem no Bairro A | Não cadastrado |
| E2 | Ponto de referência no Bairro B | Alto |
| E3 | Ponto de referência no Bairro C | Baixo |
| E4 | Ponto de destino no Bairro D | Não cadastrado |

#### Trechos

| Trecho | Liga | Tempo estimado de referência | Condição cadastrada |
|---|---|---|---|
| T1 | E1 e E2 | 10 minutos | Histórico alto de alagamento |
| T2 | E2 e E4 | 12 minutos | Histórico moderado de alagamento |
| T3 | E1 e E3 | 15 minutos | Histórico baixo de alagamento |
| T4 | E3 e E4 | 10 minutos | Sem registro de ponto problemático |
| T5 | E2 e E3 | 8 minutos | Histórico moderado de alagamento |

> **Observação:** Os tempos e níveis apresentados são exclusivamente dados de referência para demonstrar o funcionamento do protótipo. Não representam medições reais, previsões de duração ou condições atuais das ruas. Os trechos poderão ser percorridos nos dois sentidos.

### 5.2 Classificação dos Pontos Problemáticos

Os pontos cadastrados serão classificados conforme o histórico de ocorrências registrado no banco de dados do projeto.

| Classificação | Significado | Tratamento pelo sistema |
|---|---|---|
| Baixo | Local com registros históricos menos recorrentes. | Apresentar informação de atenção. |
| Moderado | Local com registros históricos recorrentes. | Destacar o ponto e recomendar atenção. |
| Alto | Local com histórico significativo de alagamentos. | Dar destaque ao ponto e priorizar alternativas cadastradas. |
| Sem registro | Não existe ocorrência cadastrada para o trecho. | Informar que não há registro disponível, sem garantir segurança. |

A classificação representa o histórico disponível no projeto e não indica a existência de alagamento no momento da consulta.

### 5.3 Critério de Escolha das Rotas

Para a v0.1, o sistema utilizará uma pontuação simplificada para comparar os caminhos disponíveis.

| Condição do trecho | Penalidade ilustrativa |
|---|---:|
| Sem registro de ponto problemático | +0 |
| Histórico baixo | +0 |
| Histórico moderado | +10 |
| Histórico alto | +30 |
| Trecho simulado como bloqueado | Inacessível |

O custo de cada caminho será calculado somando o tempo de referência e as penalidades dos trechos.

**Exemplo de comparação entre E1 e E4:**

- Rota 1: E1 → E2 → E4. Tempo de referência de 22 minutos, com penalidades de 30 e 10 pontos. Custo total: 62.
- Rota 2: E1 → E3 → E4. Tempo de referência de 25 minutos, sem penalidades. Custo total: 25.

Nesse exemplo, o sistema seleciona a segunda alternativa, mesmo sendo um pouco mais demorada, porque apresenta menor pontuação de risco histórico cadastrado.

> A pontuação é apenas um critério didático de comparação do protótipo. Não representa uma probabilidade real de alagamento nem garante que a rota escolhida esteja segura.

### 5.4 Regras Gerais

| Código | Regra |
|---|---|
| RN01 | A origem e o destino devem corresponder a pontos existentes no mapa cadastrado. |
| RN02 | A origem e o destino devem ser diferentes. |
| RN03 | Cada trecho possui uma classificação histórica cadastrada, que será utilizada na comparação das rotas. |
| RN04 | O custo de cada trecho será calculado somando seu tempo de referência à penalidade correspondente. |
| RN05 | Trechos classificados como alto risco histórico recebem maior penalidade na comparação. |
| RN06 | Trechos simulados como bloqueados não poderão fazer parte da rota calculada. |
| RN07 | O sistema deverá selecionar o caminho de menor custo total entre os caminhos disponíveis. Em caso de empate, deverá escolher o de menor tempo de referência. |
| RN08 | A rota apresentada deverá identificar os pontos historicamente problemáticos presentes no percurso. |
| RN09 | Se não existir rota alternativa cadastrada, o sistema deverá informar essa condição sem inventar um caminho. |
| RN10 | As orientações deverão deixar claro que o sistema não realiza previsão meteorológica nem emite alertas oficiais. |
| RN11 | O histórico cadastrado não deverá ser apresentado como confirmação de que uma rua está alagada no momento da consulta. |
| RN12 | Se os dados de determinado trecho não estiverem disponíveis, o sistema deverá informar a ausência de registro, sem classificá-lo automaticamente como seguro. |

> Exemplo de distinção: "Identificar uma rota alternativa" é uma funcionalidade; "trechos com histórico alto recebem maior penalidade na comparação" é uma regra que orienta o funcionamento dessa funcionalidade.

## 6. Critérios de Aceitação

Os critérios descrevem o que precisa acontecer para considerar uma funcionalidade atendida. Os exemplos abaixo são expectativas planejadas para a v0.1, utilizando exclusivamente o mapa ilustrativo da seção 5.1.

| Código | Situação e ação | Resultado esperado | Referências |
|---|---|---|---|
| CA01 | Ao iniciar, o usuário consulta os pontos disponíveis. | São apresentados os quatro pontos E1, E2, E3 e E4. | RF01; RN01 |
| CA02 | O usuário solicita uma rota de E1 para E4. | O sistema identifica as rotas disponíveis e seleciona E1 → E3 → E4, com custo 25. | RF02, RF06; RN04, RN07 |
| CA03 | O usuário solicita uma rota de E1 para E4 e consulta os pontos problemáticos. | O sistema informa que a alternativa E1 → E2 → E4 possui registros históricos altos em T1 e moderados em T2. | RF04, RF05; RN03, RN08 |
| CA04 | O usuário consulta a rota alternativa E1 → E3 → E4. | O sistema apresenta o tempo de referência de 25 minutos e informa a classificação dos trechos. | RF06, RF07; RN07, RN08 |
| CA05 | O usuário simula o bloqueio do trecho T3. | O sistema seleciona E1 → E2 → E4, caso os demais trechos permaneçam disponíveis, e destaca seus pontos problemáticos. | RF09; RN06, RN07 |
| CA06 | O usuário informa uma origem inexistente. | O sistema informa que o ponto não está cadastrado e solicita uma nova escolha. | RF03; RN01 |
| CA07 | O usuário escolhe o mesmo ponto como origem e destino. | O sistema informa que os pontos precisam ser diferentes. | RF03; RN02 |
| CA08 | O usuário solicita uma rota sem alternativa disponível. | O sistema informa que não existe uma rota alternativa cadastrada para aquele percurso. | RF08; RN09 |
| CA09 | O usuário consulta o resultado de uma rota. | O sistema informa que os dados são históricos e não representam as condições atuais das vias. | RF07; RN10, RN11 |
| CA10 | O usuário consulta um trecho sem informações históricas. | O sistema informa que não há registro disponível, sem afirmar que o local está livre de alagamentos. | RF05, RF07; RN12 |

## 7. Escopo da v0.1

### 7.1 Deve estar incluído

- Mapa simplificado com quatro pontos e cinco trechos de referência.
- Cadastro de bairros, pontos e trechos com informações históricas.
- Seleção de origem e destino.
- Validação das informações inseridas pelo usuário.
- Identificação de pontos historicamente problemáticos.
- Classificação dos pontos em níveis de atenção.
- Cálculo simplificado do custo das rotas.
- Identificação de uma rota alternativa simulada.
- Simulação de bloqueio de trechos.
- Apresentação das rotas em texto, com seus respectivos pontos de atenção.
- Orientações gerais para o deslocamento.
- Mensagem quando não existir alternativa cadastrada.
- Aviso sobre as limitações do sistema.

### 7.2 Não faz parte desta versão

- Previsão meteorológica ou previsão de enchentes.
- Emissão de alertas oficiais ou comunicação com órgãos de Defesa Civil.
- Monitoramento do nível da água em tempo real.
- Integração com sensores, câmeras ou estações meteorológicas.
- Rastreamento da localização do usuário por GPS.
- Navegação em tempo real.
- Atualização automática de ruas alagadas.
- Integração com aplicativos de transporte público.
- Cadastro de usuários e armazenamento de informações pessoais.
- Cobertura de todos os bairros da cidade.
- Garantia de que a rota indicada esteja livre de alagamentos.

### 7.3 Fluxo principal planejado

1. O usuário acessa o sistema.
2. O sistema apresenta os bairros e pontos de referência disponíveis.
3. O usuário informa sua origem e seu destino.
4. O sistema valida as informações.
5. O programa identifica os caminhos cadastrados entre os pontos.
6. O sistema consulta a classificação histórica dos trechos.
7. O programa compara os custos e identifica a rota alternativa simulada.
8. O sistema apresenta a rota, os pontos historicamente problemáticos e o tempo de referência.
9. O usuário recebe orientações gerais e o aviso sobre as limitações das informações.

**Condição de sucesso do recorte:** O usuário deverá conseguir informar uma origem e um destino, visualizar os pontos historicamente problemáticos e receber uma alternativa simulada coerente com as regras estabelecidas. Não é necessário construir um aplicativo de navegação ou um sistema de previsão de enchentes.

## 8. Backlog e Divisão de Responsabilidades

O responsável conduz a tarefa, mas não precisa realizá-la sozinho. Os nomes e prazos devem ser ajustados conforme a divisão combinada pela equipe.

| Tarefa | Resultado esperado | Responsável | Apoio ou revisão | Previsão |
|---|---|---|---|---|
| T01 — Consolidar usuário, problema e objetivo | Definição do público, problema e proposta do projeto. | [Integrante 1] | Toda a equipe | [Data] |
| T02 — Revisar requisitos funcionais e não funcionais | Requisitos coerentes com as funcionalidades e limitações. | [Integrante 2] | [Integrante 3] | [Data] |
| T03 — Definir mapa e pontos históricos | Pontos, trechos e classificações organizados. | [Integrante 3] | [Integrante 4] | [Data] |
| T04 — Definir critérios de aceitação | Casos de teste e resultados esperados conferidos. | [Integrante 4] | [Integrante 1] | [Data] |
| T05 — Organizar repositório e documentação | Repositório estruturado e documento atualizado. | [Integrante 1] | Toda a equipe | [Data] |
| T06 — Planejar entrada e validação | Implementação da escolha de origem e destino. | [Integrante 2] | [Integrante 3] | [Data] |
| T07 — Planejar o cálculo das rotas | Implementação da comparação de caminhos e penalidades. | [Integrante 3] | [Integrante 4] | [Data] |
| T08 — Planejar resultado e orientações | Exibição das rotas, pontos problemáticos e mensagens. | [Integrante 4] | [Integrante 2] | [Data] |

## 9. Registro Individual de Contribuição

Cada integrante deverá registrar o que realmente realizou durante o planejamento e como conferiu o resultado.

| Integrante | O que fez nesta etapa | Como conferiu |
|---|---|---|
| [Integrante 1] | T01: Consolidou o problema, o usuário principal e o objetivo do projeto. T05: Organizou a documentação e o repositório. | Conferiu se a proposta atende à necessidade de planejamento do deslocamento e se a documentação está organizada. |
| [Integrante 2] | T02: Revisou os requisitos funcionais e não funcionais. T06: Planejou a entrada e a validação dos pontos. | Conferiu se os requisitos estão ligados às regras e se as entradas inválidas possuem tratamento. |
| [Integrante 3] | T03: Definiu o mapa de referência, os trechos e as classificações históricas. T07: Planejou o cálculo das rotas. | Conferiu se as classificações e penalidades produzem resultados coerentes com as regras estabelecidas. |
| [Integrante 4] | T04: Elaborou os critérios de aceitação. T08: Planejou a apresentação das rotas e orientações. | Conferiu se os resultados esperados correspondem aos requisitos e às regras de negócio. |

> O registro deverá ser atualizado de acordo com as contribuições reais de cada integrante. Não devem ser registradas atividades que ainda não foram realizadas.

---

# 10. Entrega 2: Implementação e Demonstração da v0.1

Esta seção deverá registrar o que foi efetivamente implementado, as decisões tomadas durante o desenvolvimento, os testes realizados e as melhorias propostas.

- **Usuário:** Pessoa que precisa se deslocar entre bairros durante períodos de chuva.
- **Problema:** Dificuldade de identificar previamente pontos com histórico de alagamento e encontrar alternativas de deslocamento.
- **Objetivo:** Permitir a consulta de origem e destino, identificar pontos historicamente problemáticos e apresentar rotas alternativas simuladas.
- **Solução planejada:** Página web desenvolvida com HTML, CSS e JavaScript, contendo um mapa simplificado, dados cadastrados e uma lógica de comparação de caminhos.

### Links de referência

- Repositório: [Inserir link do GitHub]
- Solução publicada: [Inserir link, se houver]
- Testes automáticos: [Inserir link, se houver]
- Versão no repositório: `v0.1`.

### Como executar

1. Acessar o repositório do projeto no GitHub.
2. Clicar em Code → Download ZIP e descompactar a pasta.
3. Abrir o arquivo `index.html` no navegador.
4. Informar a origem e o destino e realizar a consulta.
5. Conferir os pontos problemáticos, a rota alternativa e as orientações apresentadas.

### 10.1 O que entrou no escopo da v0.1

A tabela abaixo deverá ser preenchida após a implementação.

| Item | Comportamento implementado | Referência da documentação | Onde está no código |
|---|---|---|---|
| Pontos de referência | Exibir os bairros e pontos cadastrados. | RF01; RN01 | [Arquivo/função] |
| Entrada de dados | Permitir selecionar origem e destino. | RF02, RF03; RN01, RN02 | [Arquivo/função] |
| Histórico de alagamentos | Identificar os trechos com registros históricos. | RF04, RF05; RN03 | [Arquivo/função] |
| Cálculo das rotas | Comparar os caminhos disponíveis e suas penalidades. | RF06; RN04, RN05, RN07 | [Arquivo/função] |
| Bloqueios simulados | Desconsiderar trechos marcados como bloqueados. | RF09; RN06 | [Arquivo/função] |
| Resultado | Exibir rota, tempo de referência e pontos problemáticos. | RF06, RF07; RN08 | [Arquivo/função] |
| Sem alternativa | Informar quando não existir caminho alternativo cadastrado. | RF08; RN09 | [Arquivo/função] |
| Avisos e orientações | Apresentar as limitações e recomendações gerais. | RF07; RN10, RN11, RN12 | [Arquivo/função] |
| Mapa separado da lógica | Manter os dados dos trechos separados do cálculo. | RNF06 | [Arquivo] |

**Fluxo principal demonstrado:** Informar origem → informar destino → validar os pontos → consultar os registros históricos → calcular os caminhos → apresentar a rota alternativa → exibir os pontos de atenção e as orientações.

### 10.2 Decisões tomadas na implementação

As decisões abaixo são propostas para orientar o desenvolvimento e deverão ser ajustadas conforme o que a equipe realmente implementar.

| Código | Decisão | Motivo |
|---|---|---|
| D1 | Utilizar HTML, CSS e JavaScript puro para desenvolver uma página web. | Facilitar a execução e a demonstração do projeto em diferentes navegadores. |
| D2 | Utilizar um mapa simplificado com pontos e trechos previamente cadastrados. | Manter o projeto dentro de um escopo possível para a primeira versão. |
| D3 | Classificar os pontos problemáticos em níveis baixo, moderado e alto. | Facilitar a identificação visual dos locais com registros históricos de alagamento. |
| D4 | Utilizar penalidades para comparar os caminhos disponíveis. | Demonstrar que a rota de menor tempo nem sempre é a alternativa com menor pontuação de risco histórico. |
| D5 | Permitir a simulação de bloqueio de trechos. | Demonstrar como a indisponibilidade de um caminho pode modificar a alternativa apresentada. |
| D6 | Separar os dados do mapa da lógica de cálculo. | Facilitar a atualização de bairros, pontos e trechos sem alterar todo o algoritmo. |
| D7 | Exibir avisos sobre as limitações das informações. | Evitar que o usuário interprete o histórico cadastrado como previsão ou confirmação de alagamento atual. |

### 10.3 O que ficou fora da v0.1

| Funcionalidade | Situação |
|---|---|
| Previsão meteorológica | Fora do escopo; o sistema não prevê chuva ou enchentes. |
| Alertas oficiais | Fora do escopo; o projeto não substitui a Defesa Civil. |
| Monitoramento em tempo real | Fora do escopo; não existem sensores ou integração com câmeras. |
| GPS e localização automática | Fora do escopo; o usuário informa manualmente a origem e o destino. |
| Mapa interativo com navegação | Fora do escopo inicial; será utilizada uma representação simplificada. |
| Atualização automática dos pontos | Fora do escopo; os registros serão cadastrados previamente. |
| Cobertura de toda a cidade | Fora do escopo; a primeira versão utiliza uma área reduzida. |
| Histórico de consultas | Fora do escopo; a versão inicial não precisa armazenar os deslocamentos dos usuários. |

### 10.4 Proposta para a v0.2

A evolução da segunda versão deverá considerar as necessidades identificadas durante a demonstração e o retorno dos usuários.

**Necessidade a validar:** A primeira versão apresenta rotas alternativas com base nos pontos cadastrados, mas pode não representar suficientemente a realidade de todos os bairros. Também pode ser necessário ampliar a quantidade de pontos de referência e facilitar a visualização das informações.

**Proposta:** Ampliar a base de dados, melhorar a apresentação visual do mapa e permitir que os registros históricos sejam atualizados de maneira organizada.

| Código | Definição |
|---|---|
| RF10 | Permitir ampliar o cadastro de bairros e pontos historicamente problemáticos. |
| RF11 | Apresentar os pontos problemáticos em uma representação visual do mapa. |
| RF12 | Permitir atualizar os registros históricos por meio de um cadastro administrativo. |
| RF13 | Apresentar a data de atualização dos registros disponíveis. |
| RF14 | Permitir filtrar os pontos por nível de atenção. |

**Regras adicionais propostas:**

| Código | Regra |
|---|---|
| RN13 | Todo registro histórico deverá possuir uma identificação do local e sua classificação. |
| RN14 | Quando disponível, cada registro deverá apresentar sua fonte e data de atualização. |
| RN15 | A atualização de um ponto não deverá alterar automaticamente informações de outros trechos. |
| RN16 | A ausência de registros recentes deverá ser apresentada como limitação dos dados, e não como garantia de que o local está livre de alagamentos. |

### 10.5 Registro dos Testes

O roteiro completo deverá apresentar os passos para repetir os testes e conferir se os resultados estão de acordo com os critérios de aceitação.

| Teste | Entrada ou ação | Resultado esperado | Resultado observado | Situação | Responsável |
|---|---|---|---|---|---|
| M01 (CA01) | Abrir a página. | Exibir os quatro pontos cadastrados. | [Preencher] | [Preencher] | [Nome] |
| M02 (CA02) | Solicitar rota E1 → E4. | Apresentar E1 → E3 → E4, com custo 25. | [Preencher] | [Preencher] | [Nome] |
| M03 (CA03) | Consultar os pontos da rota E1 → E2 → E4. | Identificar os níveis alto e moderado. | [Preencher] | [Preencher] | [Nome] |
| M04 (CA05) | Simular bloqueio de T3. | Apresentar E1 → E2 → E4, se disponível. | [Preencher] | [Preencher] | [Nome] |
| M05 (CA06) | Informar uma origem inexistente. | Exibir mensagem de entrada inválida. | [Preencher] | [Preencher] | [Nome] |
| M06 (CA07) | Informar a mesma origem e destino. | Solicitar pontos diferentes. | [Preencher] | [Preencher] | [Nome] |
| M07 (CA08) | Solicitar uma rota sem alternativa disponível. | Informar que não existe alternativa cadastrada. | [Preencher] | [Preencher] | [Nome] |
| M08 (CA09) | Consultar o resultado de uma rota. | Exibir o aviso sobre dados históricos e limitações. | [Preencher] | [Preencher] | [Nome] |
| M09 (CA10) | Consultar trecho sem registro. | Informar que não há dados disponíveis. | [Preencher] | [Preencher] | [Nome] |

**Importante:** Os resultados observados e a situação de cada teste deverão ser preenchidos somente depois da execução real. Não devem ser apresentados testes como aprovados sem que tenham sido realizados.

### 10.6 Relação entre documentação, prompt e código

A documentação deverá servir como referência para o desenvolvimento da solução.

O prompt utilizado com apoio de inteligência artificial, caso seja utilizado, deverá considerar:

- O usuário e o problema descritos na seção 1.
- A história de usuário da seção 2.
- Os requisitos funcionais e não funcionais das seções 3 e 4.
- O mapa de referência e as classificações da seção 5.
- As regras de negócio RN01 a RN12.
- Os critérios de aceitação CA01 a CA10.
- O escopo estabelecido para a v0.1.

O código desenvolvido deverá ser conferido com base nos critérios de aceitação, garantindo que as funcionalidades estejam de acordo com as regras estabelecidas.

### 10.7 Tarefas e Quadro de Contribuições da Entrega 2

| Tarefa | Resultado esperado | Responsável | Apoio ou revisão |
|---|---|---|---|
| T09 — Revisar entrada e validação | Conferir a seleção de origem e destino e os tratamentos de entrada inválida. | [Integrante 1] | [Integrante 2] |
| T10 — Revisar o cálculo das rotas | Conferir a comparação dos caminhos, penalidades e bloqueios. | [Integrante 2] | [Integrante 3] |
| T11 — Revisar a apresentação dos resultados | Conferir as informações históricas, rotas e orientações. | [Integrante 3] | [Integrante 4] |
| T12 — Conferir o mapa e os testes | Verificar os pontos cadastrados e executar os testes automáticos. | [Integrante 4] | [Integrante 1] |
| T13 — Integrar, publicar e documentar | Organizar a implementação, realizar os testes e atualizar a documentação. | [Integrante 1] | Toda a equipe |

O registro individual da Entrega 2 deverá apresentar as contribuições efetivamente realizadas, os arquivos modificados e a forma utilizada para verificar cada atividade.

| Integrante | O que fez na Entrega 2 | Link da contribuição | Como conferiu |
|---|---|---|---|
| [Integrante 1] | [Descrever atividade realizada.] | [Link do GitHub] | [Descrever verificação.] |
| [Integrante 2] | [Descrever atividade realizada.] | [Link do GitHub] | [Descrever verificação.] |
| [Integrante 3] | [Descrever atividade realizada.] | [Link do GitHub] | [Descrever verificação.] |
| [Integrante 4] | [Descrever atividade realizada.] | [Link do GitHub] | [Descrever verificação.] |

---

**Consideração final sobre o projeto**

O Alerta de Enchentes por Bairro busca oferecer uma ferramenta simples de apoio ao planejamento dos deslocamentos durante períodos de chuva. Sua principal contribuição está em organizar informações históricas sobre pontos problemáticos e apresentar alternativas de trajetos que ajudem o usuário a tomar decisões mais conscientes antes de sair.

É importante reforçar que o sistema não prevê enchentes, não confirma alagamentos em andamento e não substitui informações dos órgãos oficiais. Em situações de risco, o usuário deverá seguir as orientações da Defesa Civil e dos órgãos responsáveis.
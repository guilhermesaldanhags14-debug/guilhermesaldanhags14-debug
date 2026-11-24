# Alerta de Enchentes por Bairro

24/11/2026

**Documentação final da AV2: definições, planejamento e implementação da v0.1.** Disciplina: [disciplina]. Equipe: [nomes dos integrantes].

> **Nota sobre o escopo:** o sistema não faz previsão do tempo e não emite alertas oficiais. O mapa e os registros de alagamento usados nas regras são simplificados e servem apenas para o protótipo: distâncias e níveis de histórico foram definidos pela equipe como referência e ainda precisam ser conferidos com fontes oficiais.

## 1. Visão Geral do Projeto

- **Nome:** Alerta de Enchentes por Bairro.
- **Contexto e problema:** Em períodos de chuva, certos pontos da cidade costumam alagar, e quem precisa se deslocar só descobre o problema quando chega nele: o carro para na água, a moto não passa e o pedestre acaba atravessando água parada ou descendo para a rua. O problema não é só o ponto alagado: é que a pessoa não sabe, antes de sair, quais trechos do caminho têm histórico de alagamento e se existe um caminho alternativo.
- **Usuário principal:** Pessoa que precisa se deslocar em período de chuva, a pé, de moto ou de carro, e quer conhecer os pontos com histórico de alagamento antes de sair.
- **Objetivo:** Permitir que o usuário informe seu perfil de deslocamento, o bairro de saída e o de chegada, e receba a rota com menos pontos críticos para o seu perfil, com os trechos em ordem, a distância total, os pontos críticos do caminho, os pontos de apoio e orientações para o deslocamento.
- **O que o projeto não é:** O sistema não faz previsão do tempo e não emite alertas oficiais. Ele trabalha apenas com pontos historicamente problemáticos cadastrados: não sabe se está chovendo nem se um trecho está alagado agora. Para previsão e alertas, o usuário deve consultar os órgãos oficiais, como a Defesa Civil.
- **Recorte da implementação:** A v0.1 vai calcular uma rota por vez, em um mapa fixo e simplificado de 4 pontos (bairros de referência) e 5 trechos, com 3 perfis de deslocamento e resposta em texto. Não pretende ser um aplicativo de navegação.

## 2. História de Usuário

> "Como pessoa que precisa se deslocar em período de chuva, quero informar meu perfil, de onde saio e aonde vou, para saber antes de sair quais trechos do caminho têm histórico de alagamento, qual rota evita os piores pontos e onde posso parar se precisar."

## 3. Requisitos Funcionais

Requisitos funcionais descrevem o que a solução deve fazer.

| Código | Requisito | Prioridade para a v0.1 |
| --- | --- | --- |
| RF01 | Exibir os perfis disponíveis (a pé, moto, carro) e pedir que o usuário escolha um. | Essencial |
| RF02 | Exibir os bairros de referência do mapa e pedir o ponto de saída e o ponto de chegada. | Essencial |
| RF03 | Calcular o custo de cada trecho de acordo com o perfil escolhido e o histórico de alagamento do trecho. | Essencial |
| RF04 | Desconsiderar os trechos bloqueados para o perfil escolhido e os trechos marcados como alagados no momento. | Essencial |
| RF05 | Encontrar a rota de menor custo entre a saída e a chegada. | Essencial |
| RF06 | Exibir a rota em texto: trechos em ordem, histórico de cada trecho, pontos críticos do caminho, distância total, pontos de apoio e orientações para o deslocamento. | Essencial |
| RF07 | Informar quando não existe rota possível para o perfil escolhido. | Essencial |
| RF08 | Exibir, em todo resultado, o aviso de que o sistema não é previsão do tempo nem alerta oficial. | Essencial |
| RF09 | Exibir também a rota mais curta permitida quando ela for diferente da recomendada, com os pontos críticos que ela atravessa. | Essencial |

## 4. Requisitos Não Funcionais

Requisitos não funcionais descrevem condições de qualidade e uso.

| Código | Requisito | Como verificar |
| --- | --- | --- |
| RNF01 | Apresentar perfis, pontos, orientações e mensagens em português. | Conferir os textos do fluxo principal. |
| RNF02 | Exibir distâncias em metros, como números inteiros. | Conferir a distância de cada trecho e a total no resultado. |
| RNF03 | Mostrar no resultado o histórico de alagamento de cada trecho, e não só a rota final. | Verificar se o usuário consegue entender por que a rota foi escolhida. |
| RNF04 | Executar o fluxo principal no navegador, abrindo o `index.html` localmente ou pelo site publicado, sem instalar nada e sem serviço pago. | Abrir o `index.html` e percorrer o fluxo. |
| RNF05 | Manter o mapa (pontos, trechos, históricos e marcações de alagamento) separado da lógica de cálculo, para que um trecho possa ser alterado sem mexer no algoritmo. | Alterar o histórico de um trecho no mapa e conferir se a rota muda sem editar outra parte do código. |
| RNF06 | Registrar no mapa, para cada trecho, a fonte e a data do registro de alagamento. Na v0.1, a fonte é "valor de referência da equipe". | Conferir se todo trecho do `mapa.js` tem fonte e data preenchidas. |

**Escolha tecnológica:** HTML, CSS e JavaScript puro, em uma página que roda no navegador. O mapa fica em listas e objetos no arquivo `mapa.js`, separado da lógica em `regras.js`. [Ajustar esta linha se a equipe usar outra linguagem.]

## 5. Regras de Negócio

Regras de negócio descrevem as condições que determinam o comportamento da solução.

### 5.1 Mapa de Referência da v0.1

#### Pontos de referência

| Ponto | Descrição | Ponto de apoio |
| --- | --- | --- |
| B1 | Bairro A (saída dos exemplos) | Não |
| B2 | Bairro B | Sim: local coberto e elevado, como estabelecimento ou abrigo parceiro |
| B3 | Bairro C | Não |
| B4 | Bairro D (destino de referência) | Não |

#### Trechos

| Trecho | Liga | Distância | Histórico de alagamento | Fonte e data do registro |
| --- | --- | --- | --- | --- |
| T1 | B1 e B2 | 100 m | Sem registro | Referência da equipe, [data] |
| T2 | B2 e B4 | 100 m | Moderado | Referência da equipe, [data] |
| T3 | B1 e B3 | 150 m | Moderado | Referência da equipe, [data] |
| T4 | B3 e B4 | 100 m | Sem registro | Referência da equipe, [data] |
| T5 | B2 e B3 | 80 m | Leve | Referência da equipe, [data] |

> **Observação:** Os valores são de referência, definidos para o protótipo. O nível de histórico é uma classificação da equipe, não uma medição oficial, e os nomes "Bairro A a D" devem ser trocados pelos bairros reais quando houver fonte para os dados. Os trechos podem ser percorridos nos dois sentidos.

### 5.2 Penalidades por Perfil

| Histórico do trecho | A pé | Moto | Carro |
| --- | --- | --- | --- |
| Leve | +20 | +100 | +50 |
| Moderado | +100 | Bloqueado | +200 |
| Grave | Bloqueado | Bloqueado | Bloqueado |

Nenhum trecho do mapa de referência é grave. Essa linha da tabela é verificada no mapa de teste dos testes automáticos.

### 5.3 Regras Gerais

| Código | Regra |
| --- | --- |
| RN01 | O perfil deve ser um dos três disponíveis (a pé, moto, carro). Qualquer outra entrada é inválida. |
| RN02 | Saída e chegada devem ser pontos existentes no mapa e diferentes entre si. |
| RN03 | O custo de um trecho é a sua distância em metros somada à penalidade do perfil escolhido para o histórico do trecho, conforme a tabela 5.2. |
| RN04 | Trecho com histórico grave é bloqueado para todos os perfis. Para o perfil moto, trecho com histórico moderado também é bloqueado. Trecho bloqueado não pode fazer parte da rota. |
| RN05 | Trecho marcado como alagado no momento é bloqueado para todos os perfis. Na v0.1, o sistema não recebe dados em tempo real: a marcação é feita antes do cálculo, no arquivo `mapa.js` ou pela simulação de trecho alagado da tela (decisão D2). |
| RN06 | A rota escolhida é a de menor custo total. Em caso de empate no custo, vale a de menor distância. |
| RN07 | A distância exibida é a soma das distâncias reais dos trechos, sem as penalidades. |
| RN08 | Todo trecho com histórico leve ou moderado que faça parte da rota aparece com um alerta no resultado, indicando o nível de histórico. |
| RN09 | Se não houver rota sem trechos bloqueados, o programa informa que não existe rota conhecida para aquele perfil e não exibe rota. |
| RN10 | Todo resultado, com rota ou sem rota, exibe o aviso de que o sistema não faz previsão do tempo nem emite alertas oficiais, de que os pontos críticos são registros históricos de referência e de que, em emergência, se deve ligar para a Defesa Civil (199) ou para os Bombeiros (193). |
| RN11 | Quando a rota mais curta entre as permitidas (não bloqueadas) for diferente da recomendada, o resultado também mostra essa rota, com seu custo e os pontos críticos que ela atravessa, para explicar a escolha. |
| RN12 | Todo resultado com rota exibe a orientação do perfil escolhido, conforme a tabela 5.4. |

> **Exemplo de distinção:** "calcular a rota" é uma funcionalidade; "a moto nunca passa por trecho com alagamento moderado, e quem vai a pé passa pagando +100" é a regra que orienta esse cálculo.

### 5.4 Orientações por Perfil

| Perfil | Orientação exibida |
| --- | --- |
| A pé | Se encontrar água parada ou com correnteza no caminho, não atravesse: procure o ponto de apoio mais próximo ou outro caminho. |
| Moto | Se encontrar água no caminho, não tente atravessar: pare em local seguro e elevado ou escolha outro caminho. |
| Carro | Se encontrar água no caminho, não entre nela: pare em local seguro e elevado ou escolha outro caminho. |

Na v0.1, esses textos são orientações gerais de segurança escritas pela equipe. Antes de qualquer uso real, devem ser conferidos com o material da Defesa Civil.

## 6. Critérios de Aceitação

Os critérios descrevem o que precisa acontecer para considerar uma funcionalidade atendida. Os valores abaixo vêm da soma da distância e da penalidade de cada trecho; a equipe deve refazer a conta à mão antes de entregar (tarefa T04). Até serem executados nos testes da seção 10.5, são expectativas planejadas, não afirmações de que a solução já foi testada.

| Código | Situação e ação | Resultado esperado | Referências |
| --- | --- | --- | --- |
| CA01 | Ao iniciar, o usuário consulta os perfis e os pontos. | São exibidos os três perfis (a pé, moto e carro) e os pontos B1 a B4. | RF01, RF02; RN01 |
| CA02 | Usuário a pé pede rota de B1 para B4. | Rota B1 → B2 → B4 (T1, T2), 200 m, custo 300. Ela empata no custo com B1 → B2 → B3 → B4 (custo 300, 280 m) e vence por ter menor distância. Alerta de alagamento moderado em T2 e ponto de apoio em B2. | RF03–RF06; RN03, RN06, RN07, RN08 |
| CA03 | Usuário de carro pede rota de B1 para B4. | Rota B1 → B2 → B3 → B4 (T1, T5, T4), 280 m, custo 330, com alerta de histórico leve em T5 e ponto de apoio em B2. O resultado mostra também a rota mais curta permitida (B1 → B2 → B4, 200 m, custo 400), que atravessa o ponto crítico moderado de T2. | RF03–RF06, RF09; RN03, RN06, RN08, RN11 |
| CA04 | Usuário de moto pede rota de B1 para B4. | Rota B1 → B2 → B3 → B4, 280 m, custo 380, com alerta de histórico leve em T5 e ponto de apoio em B2. As rotas por T2 e por T3 não são consideradas, pois o histórico moderado bloqueia a moto. | RF04–RF06; RN03, RN04, RN08 |
| CA05 | Com T1 marcado como alagado, usuário a pé pede rota de B1 para B4. | Rota B1 → B3 → B4 (T3, T4), 250 m, custo 350, com alerta de alagamento moderado em T3 e sem ponto de apoio na rota. | RF04–RF06; RN05, RN08 |
| CA06 | Com T1 marcado como alagado, usuário de moto pede rota de B1 para B4. | O programa informa que não existe rota conhecida para moto e não exibe rota. | RF04, RF07; RN04, RN05, RN09 |
| CA07 | O usuário digita um perfil que não existe (ex.: "4" ou "abc"). | O programa informa que o perfil é inválido e pede a escolha de novo. | RF01; RN01 |
| CA08 | O usuário escolhe o mesmo ponto como saída e chegada. | O programa informa que saída e chegada precisam ser diferentes e pede de novo. | RF02; RN02 |
| CA09 | Qualquer resultado, com rota (CA02 a CA05) ou sem rota (CA06). | O resultado mostra o aviso de que o sistema não é previsão do tempo nem alerta oficial, com os telefones 199 e 193. Quando há rota, mostra também a orientação do perfil escolhido. | RF06, RF08; RN10, RN12 |

## 7. Escopo da v0.1

### 7.1 Deve estar incluído

- Mapa fixo com os 4 pontos e 5 trechos de referência, cada trecho com seu histórico de alagamento.
- Escolha de perfil, saída e chegada, com validação.
- Cálculo do custo por trecho com as penalidades de cada perfil.
- Bloqueio de trechos conforme o histórico e de trechos marcados como alagados na simulação da tela.
- Escolha da rota de menor custo, com desempate pela menor distância.
- Resultado em texto com trechos, histórico de cada trecho, alertas, distância total, pontos de apoio, orientação do perfil e, quando for diferente, a rota mais curta permitida.
- Mensagem quando não existe rota.
- Aviso de que o sistema não é previsão do tempo nem alerta oficial em todo resultado.

### 7.2 Não faz parte desta versão

- Previsão do tempo, alerta oficial, dados de chuva em tempo real ou nível de rios e maré.
- Mapa visual, GPS ou navegação em tempo real.
- Aplicativo de celular. (A v0.1 é uma página web simples, sem mapa visual.)
- Atualização automática de pontos alagados, obras ou interdições. A marcação é feita à mão antes do cálculo.
- Bairros e ruas além dos 4 pontos de referência.
- Dados oficiais ou levantados em campo (a v0.1 usa valores de referência da equipe).
- Cadastro de usuários, envio de relatos e notificações.

### 7.3 Fluxo principal planejado

1. O usuário escolhe o perfil.
2. Informa o ponto de saída e o ponto de chegada.
3. Se quiser, marca na simulação um trecho como alagado no momento.
4. O programa calcula o custo de cada trecho para aquele perfil e descarta os bloqueados.
5. O programa encontra a rota de menor custo.
6. O programa exibe os trechos em ordem, o histórico de cada um, os alertas, a distância total, os pontos de apoio, a orientação do perfil e o aviso de que não é alerta oficial.

**Condição de sucesso do recorte:** percorrer esse fluxo para os três perfis e obter, para cada um, uma rota coerente com as regras e com os critérios de aceitação. Não é necessário construir um aplicativo de navegação.

## 8. Backlog e Divisão de Responsabilidades

O responsável conduz a tarefa, mas não precisa fazer sozinho. Ajustar os nomes e as datas conforme a divisão combinada no grupo.

| Tarefa | Resultado esperado | Responsável | Apoio ou revisão | Previsão |
| --- | --- | --- | --- | --- |
| T01 — Consolidar usuário, problema e recorte | Definições coerentes com a proposta, deixando claro que o sistema não é previsão nem alerta oficial | [Nome] | Toda a equipe | Até [data] |
| T02 — Revisar requisitos funcionais e não funcionais | Requisitos sem contradição com as regras e o escopo | [Nome] | [Nome] | Até [data] |
| T03 — Definir mapa de referência e penalidades | Pontos, trechos, históricos e penalidades fechados e justificados | [Nome] | [Nome] | Até [data] |
| T04 — Conferir critérios de aceitação | Rotas, custos e distâncias dos exemplos recalculados e ligados às regras | [Nome] | [Nome] | Até [data] |
| T05 — Organizar repositório, acessos e entrega | Repositório com o professor, integrantes identificados e links conferidos | [Nome] | [Nome] | Até [data] |
| T06 — Planejar entrada e validação | Divisão da implementação do menu de perfis e pontos (RF01, RF02) | [Nome] | [Nome] | Planejar até [data] |
| T07 — Planejar o cálculo da rota | Divisão da implementação do custo por trecho e da busca da rota (RF03–RF05) | [Nome] | [Nome] | Planejar até [data] |
| T08 — Planejar a saída, as mensagens e o aviso | Divisão da implementação do resultado em texto, da mensagem sem rota e do aviso de que não é alerta oficial (RF06–RF09) | [Nome] | [Nome] | Planejar até [data] |

## 9. Registro Individual de Contribuição

Cada integrante registra o que fez de verdade e como conferiu. Preencher só com o que foi feito.

| Integrante | O que fez nesta etapa | Como conferiu |
| --- | --- | --- |
| [Nome] @[usuário no GitHub] | **[Tarefa]:** [o que a pessoa fez] | **[Tarefa]:** [como conferiu o próprio trabalho] |
| [Nome] @[usuário no GitHub] | **[Tarefa]:** [o que a pessoa fez] | **[Tarefa]:** [como conferiu o próprio trabalho] |
| [Nome] @[usuário no GitHub] | **[Tarefa]:** [o que a pessoa fez] | **[Tarefa]:** [como conferiu o próprio trabalho] |
| [Nome] @[usuário no GitHub] | **[Tarefa]:** [o que a pessoa fez] | **[Tarefa]:** [como conferiu o próprio trabalho] |
| [Nome] @[usuário no GitHub] | **[Tarefa]:** [o que a pessoa fez] | **[Tarefa]:** [como conferiu o próprio trabalho] |

## 10. Entrega 2: implementação e demonstração da v0.1

- **Usuário:** pessoa que precisa se deslocar em período de chuva, a pé, de moto ou de carro.
- **Problema:** a pessoa só descobre o ponto alagado quando chega nele e não sabe, antes de sair, quais trechos têm histórico de alagamento nem se existe um caminho alternativo.
- **Objetivo:** escolher perfil, saída e chegada e receber a rota com menos pontos críticos, com trechos em ordem, alertas, distância total, pontos de apoio e orientações.
- **Solução:** site estático em HTML, CSS e JavaScript puro, publicado no GitHub Pages. Também roda abrindo o `index.html` no computador. Nada é salvo; recarregar a página apaga as escolhas. O sistema não faz previsão do tempo nem emite alertas oficiais. [Ajustar se a tecnologia ou a publicação forem outras.]

### Links de referência

- **Repositório:** [link do repositório]
- **Solução publicada:** [link da página publicada]
- **Testes automáticos publicados:** [link do `tests.html` publicado]
- **Versão no repositório:** tag `v0.1` [confirmar que a tag existe]
- **Prompt usado com a IA:** [link do `PROMPT.md`]

**Como executar sem internet**

1. Entre no repositório e clique em **Code → Download ZIP**, depois descompacte a pasta. Quem usa Git pode clonar o repositório.
2. Dentro da pasta, dê dois cliques em `index.html`. Ele abre no navegador (Chrome, Firefox ou Edge) e já funciona, sem instalar nada e sem internet.
3. Para ver os testes automáticos, abra `tests.html` na mesma pasta. O placar no topo mostra quantos foram aprovados.

### 10.1 O que entrou no escopo da v0.1

| Item | Comportamento implementado | Referência da documentação | Onde está no código |
| --- | --- | --- | --- |
| Perfis | Exibir a pé, moto e carro e recusar perfil fora da lista. | RF01; RN01 | `mapa.js` (`PERFIS`); `regras.js` (`validarPerfil`) |
| Pontos | Exibir B1 a B4 e exigir saída e chegada existentes e diferentes. | RF02; RN02 | `regras.js` (`validarPontos`) |
| Custo por trecho | Distância somada às penalidades do perfil para o histórico do trecho. | RF03; RN03 | `regras.js` (`calcularCustoTrecho`) |
| Bloqueios | Descartar trechos bloqueados pelo histórico (grave para todos, moderado para moto) e trechos marcados como alagados. | RF04; RN04, RN05 | `regras.js` (`calcularCustoTrecho`) |
| Rota de menor custo | Testar os caminhos possíveis e escolher o de menor custo; em empate, o de menor distância. | RF05; RN06 | `regras.js` (`listarCaminhos`, `calcularRota`) |
| Resultado em texto | Trechos em ordem com histórico, alertas, distância total em metros inteiros, pontos de apoio, orientação do perfil e rota mais curta quando diferente. | RF06, RF09; RN07, RN08, RN11, RN12; RNF02, RNF03 | `regras.js` (`calcularRota`, `alertasDoTrecho`, `formatarDistancia`); `script.js` (`mostrarResultado`) |
| Sem rota | Informar que não existe rota conhecida para o perfil e não mostrar rota. | RF07; RN09 | `regras.js` (`calcularRota`) |
| Aviso | Exibir em todo resultado que o sistema não é previsão do tempo nem alerta oficial. | RF08; RN10 | `script.js` (`mostrarResultado`); `mapa.js` (`AVISO`) |
| Mapa separado da lógica | Pontos, trechos, históricos, fontes e orientações só em `mapa.js`. | RNF05, RNF06 | `mapa.js` |

Os nomes de arquivos e de funções acima são os planejados. Ajustar aos nomes reais do código gerado.

**Fluxo principal demonstrado:** escolher o perfil → escolher saída e chegada → (opcional) simular trecho alagado → calcular rota → ler trechos, alertas, distância total, pontos de apoio, orientação e aviso.

### 10.2 Decisões tomadas na implementação

| Código | Decisão | Motivo |
| --- | --- | --- |
| D1 | Usar valores de referência da equipe em vez de dados oficiais de alagamento. | Não há base oficial nem levantamento em campo para esta entrega. Por isso o sistema registra fonte e data de cada trecho (RNF06) e deixa claro que não é alerta oficial. |
| D2 | Incluir na tela uma "simulação de trecho alagado", com caixas de marcar T1 a T5. | A RN05 diz que a marcação é feita antes do cálculo. Durante a demonstração não dá para editar o mapa a cada caso, e sem isso não seria possível demonstrar CA05 e CA06. A simulação cria uma cópia do mapa e não altera `mapa.js`. |
| D3 | Escolher perfil por botões de opção e pontos por listas. | Evita digitação errada. Os valores inválidos do CA07 ("4", "abc") são testados direto na função `validarPerfil`; na tela, o caso possível é calcular sem escolher perfil. |
| D4 | Buscar a rota testando todos os caminhos sem repetir ponto, em vez de Dijkstra. | Com 4 pontos e 5 trechos, é mais fácil de entender e de conferir à mão. Para um mapa maior, Dijkstra seria o indicado (anotado no código). |
| D5 | Mostrar também o custo usado na escolha e, quando for diferente, a rota mais curta permitida. | Ajuda a explicar por que a rota mais curta nem sempre é a escolhida (RNF03, RN11). |
| D6 | Listar como ponto de apoio qualquer ponto da rota com apoio, incluindo saída e chegada. | A documentação não dizia se saída e chegada contam. Quem sai ou chega a um ponto de apoio também pode usá-lo. |
| D7 | Mostrar o aviso de que não é previsão nem alerta oficial em todo resultado, inclusive quando não há rota. | Evita que o histórico seja lido como a situação atual das ruas e que alguém tome uma decisão de risco com base nele. |

### 10.3 O que ficou fora da v0.1

| Funcionalidade | Situação |
| --- | --- |
| Previsão do tempo, alerta oficial e dados de chuva em tempo real | Fora do recorte; o sistema só usa o histórico cadastrado. |
| Mapa visual, GPS e navegação em tempo real | Fora do recorte; o resultado é em texto. |
| Aplicativo de celular | Fora do recorte; a v0.1 é uma página web simples. |
| Atualização automática de pontos alagados, obras e interdições | Fora do recorte; a marcação é feita à mão antes do cálculo. |
| Bairros reais, dados oficiais e dados levantados em campo | Fora do recorte; o mapa usa valores de referência. |
| Cadastro de usuários, relatos dos usuários e notificações | Fora do recorte. |
| Salvar rotas ou histórico | Fora do recorte; nada é guardado. |
| Opção de evitar trechos com histórico moderado ou pior | Não foi pedida na v0.1; é a melhoria proposta para a v0.2. |

### 10.4 Proposta para a v0.2

**Necessidade a validar na apresentação.** Quem quer correr o mínimo de risco, por exemplo ao sair com criança ou com idoso, não tem como pedir uma rota que fuja de qualquer trecho com histórico moderado ou grave. Na v0.1, um trecho moderado ainda pode ser escolhido quando o custo compensa: a pé, de B2 para B4, a rota passa por T2 (moderado) porque o desvio por T5 e T4 tem o mesmo custo e é mais longo.

**Proposta.** Uma opção "Evitar trechos com alagamento moderado ou grave". Marcada, a solução escolhe a rota de menor custo entre as que não têm nenhum trecho com histórico moderado ou grave (o leve é permitido), mantendo todas as regras da v0.1.

| Código | Definição |
| --- | --- |
| RF10 | Permitir que o usuário marque a opção de evitar trechos com histórico moderado ou grave. |
| RN13 | Com a opção marcada, trecho com histórico moderado ou grave é bloqueado para todos os perfis. Entre as rotas restantes, valem RN03 a RN12. |
| RN14 | Se nenhuma rota sem esses trechos for possível, informar isso e mostrar a rota da v0.1 (sem a opção), se ela existir, com aviso claro. Se nem ela existir, vale a RN09. |
| RN15 | Com a opção desmarcada, o resultado é exatamente o da v0.1. |

| ID | Dado | Quando | Então | RF e RN |
| --- | --- | --- | --- | --- |
| CA10 | Mapa de referência, perfil a pé. | Pede rota de B2 para B4 sem a opção. | Rota B2 → B4 (T2), 100 m, custo 200. Ela empata no custo com B2 → B3 → B4 (custo 200, 180 m) e vence pela menor distância. Alerta de alagamento moderado em T2 e ponto de apoio em B2. | RF05; RN06, RN15 |
| CA11 | Mapa de referência, perfil a pé. | Pede rota de B2 para B4 com a opção marcada. | Rota B2 → B3 → B4 (T5, T4), 180 m, custo 200, com alerta de histórico leve em T5 e ponto de apoio em B2. | RF10; RN08, RN13 |
| CA12 | T5 marcado como alagado, perfil carro. | Pede rota de B1 para B4 com a opção marcada. | Aviso de que não há rota sem trechos moderados ou graves, seguido da rota B1 → B2 → B4 (T1, T2), 200 m, custo 400, com alerta de alagamento moderado em T2. | RF10; RN05, RN14 |
| CA13 | Mapa de referência, perfil carro. | Pede rota de B1 para B4 com a opção marcada. | Mesma rota do CA03 (B1 → B2 → B3 → B4, 280 m, custo 330), pois ela só tem trecho leve. A rota B1 → B2 → B4 não aparece, pois T2 é bloqueado pela opção. | RF10; RN13 |

Na v0.2, os testes da v0.1 (CA01 a CA09) devem ser executados de novo, com a opção desmarcada, e os casos CA10 a CA13 acrescentados. A proposta pode ser ajustada depois do retorno da apresentação.

### 10.5 Registro dos testes

O roteiro completo, com os passos para repetir, fica em `TESTES.md` [link]. Os testes automáticos ficam em `tests.html`.

**Como os resultados abaixo foram obtidos.** [Preencher com a data, quem executou, como (à mão ou por script) e o placar real do `tests.html`.] Só registrar como aprovado o que foi de fato executado.

| Teste | Entrada ou ação | Resultado esperado | Resultado observado | Situação | Responsável e evidência |
| --- | --- | --- | --- | --- | --- |
| M01 (CA01) | Abrir a página. | Três perfis e pontos B1 a B4. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M02 (CA02) | A pé, B1 → B4. | B1 → B2 → B4, 200 m, custo 300, alerta moderado em T2, apoio em B2. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M03 (CA03) | Carro, B1 → B4. | B1 → B2 → B3 → B4, 280 m, custo 330, alerta leve em T5, apoio em B2, e rota mais curta (200 m, custo 400) mostrada. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M04 (CA04) | Moto, B1 → B4. | B1 → B2 → B3 → B4, 280 m, custo 380, alerta leve em T5, apoio em B2. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M05 (CA05) | Marcar T1 alagado; a pé, B1 → B4. | B1 → B3 → B4, 250 m, custo 350, alerta moderado em T3, sem ponto de apoio. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M06 (CA06) | Marcar T1 alagado; moto, B1 → B4. | Mensagem de que não existe rota conhecida; sem rota. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M07 (CA07) | Calcular sem escolher perfil. Na função: perfis "4" e "abc". | Mensagem de perfil inválido. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M08 (CA08) | Mesmo ponto (B2) como saída e chegada. | Mensagem de que precisam ser diferentes. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |
| M09 (CA09) | Conferir os resultados de M02 a M06. | Aviso de que não é previsão nem alerta oficial, com 199 e 193, e orientação do perfil quando há rota. | [preencher] | [Aprovado ou reprovado] | [Nome]. Evidência: print em `evidencias/`. |

Além dos critérios, `tests.html` deve verificar os custos de trecho (RN03), o bloqueio por histórico grave no mapa de teste (RN04), a marcação de trecho alagado (RN05), o desempate por menor distância (RN06), a distância sem penalidades (RN07), o sentido inverso dos trechos e que a simulação de trecho alagado não altera o mapa original.

### 10.6 Relação entre documentação, prompt e código

O prompt (`PROMPT.md`) deve ser escrito a partir desta documentação: usa o mesmo usuário, o mapa das seções 5.1 e 5.2, as regras RN01 a RN12 e os critérios CA01 a CA09, e acrescenta as orientações técnicas de página estática (arquivos separados, caminhos relativos, regras sem acesso à página). O código gerado com apoio de IA deve ser conferido contra os critérios da seção 6, e as decisões tomadas durante a implementação ficam na seção 10.2. [Confirmar o que foi de fato feito antes de entregar.]

### 10.7 Tarefas e quadro de contribuições da Entrega 2

| Tarefa | Resultado esperado | Responsável | Apoio ou revisão |
| --- | --- | --- | --- |
| T09 — Revisar entrada e validação | Conferir `script.js` (formulário) e `validarPerfil`/`validarPontos`; executar M01, M07 e M08 | [Nome] | [Nome] |
| T10 — Revisar o cálculo da rota | Conferir `calcularCustoTrecho`, `listarCaminhos` e `calcularRota`; executar M02 a M06 | [Nome] | [Nome] |
| T11 — Revisar a saída e as mensagens | Conferir `mostrarResultado`, alertas, aviso e mensagem sem rota; executar M09 | [Nome] | [Nome] |
| T12 — Conferir o mapa e os testes automáticos | Conferir `mapa.js` contra as seções 5.1, 5.2 e 5.4 e rodar `tests.html` | [Nome] | [Nome] |
| T13 — Integrar, publicar e documentar | Implementação da v0.1 com apoio de IA, testes automáticos, `TESTES.md`, publicação e esta seção | [Nome] | [Nome] |

O link de cada integrante mostra os commits feitos por ele no repositório. Cada um registra no `TESTES.md`, com um commit próprio, o resultado dos testes manuais pelos quais é responsável.

| Integrante | O que fez na Entrega 2 | Link da contribuição | Como conferiu |
| --- | --- | --- | --- |
| [Nome] @[usuário] | **[Tarefa]:** [o que a pessoa fez] | [link dos commits da pessoa] | [como conferiu] |
| [Nome] @[usuário] | **[Tarefa]:** [o que a pessoa fez] | [link dos commits da pessoa] | [como conferiu] |
| [Nome] @[usuário] | **[Tarefa]:** [o que a pessoa fez] | [link dos commits da pessoa] | [como conferiu] |
| [Nome] @[usuário] | **[Tarefa]:** [o que a pessoa fez] | [link dos commits da pessoa] | [como conferiu] |
| [Nome] @[usuário] | **[Tarefa]:** [o que a pessoa fez] | [link dos commits da pessoa] | [como conferiu] |

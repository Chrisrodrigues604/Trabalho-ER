# S.I.G.M.A. — Sistema Inteligente Geoespacial de Mapeamento e Análise

# 1. Introdução
## 1.1. Estrutura/Visão Geral do Documento
Este documento especifica o S.I.G.M.A., plataforma de consulta geoespacial e
geomarketing. As seções seguem a ordem do modelo: introdução, requisitos, histórias,
perspectivas e diagramas.
Os requisitos definem o comportamento; as histórias, a rastreabilidade e os diagramas
detalham o uso e a estrutura do sistema.
## 1.2. Audiência do Documento
A tabela abaixo relaciona cada seção a seus leitores principais.
| Seção | Público principal | Finalidade |
|------------|------------------------|------------------------------|
| Introdução | Usuários e avaliadores | Compreender o sistema e seus limites. |
| Requisitos e histórias | Desenvolvedores e testadores | Implementar e  validar funcionalidades. |
| Diagramas e casos de uso | Equipe técnica | Examinar fluxos, dados e componentes.
## 1.3. Convenções, Termos e Abreviações
RF - Requisito funcional; descreve uma capacidade observável do sistema.<br>
RNF - Requisito não funcional; estabelece uma condição de qualidade ou restrição.<br><br>
RN - Regra de negócio; RU - requisito de usuário; RS - requisito de sistema RSW - requisito de software; POI -
ponto de interesse; SSO - autenticação única; MFA - autenticação multifator.
## 1.4. Descrição Geral do Sistema
No S.I.G.M.A., o usuário delimita a área e escolhe tipos de POI e a fonte da consulta. O sistema busca
lugares pela Google Places API e/ou em uma base importada, aplica o recorte espacial e apresenta indicadores.
Também permite comparar áreas, salvar critérios, exportar resultados permitidos e compartilhar análises.
## 1.5. Descrição dos Usuários
Os usuários do S.I.G.M.A. são classificados conforme suas responsabilidades:
1. Analista: seleciona áreas, tipos de POI e fonte da consulta; interpreta os
indicadores.
2. Administrador de dados: valida e publica bases importadas, quando essa fonte
é utilizada.

<!-- Página 5 do PDF -->

# 2. Requisitos Gerais do Sistema
## 2.1. Requisitos Funcionais
A tabela apresenta as funcionalidades propostas. O usuário escolhe os tipos de POI; a busca pode usar Google Places API, base importada ou ambas.<br>
| ID | Descrição | Escopo | Prioridade |
|----|-----------|--------|------------|
| RF-01 | Autenticar usuários com Google SSO e MFA para acesso às funções protegidas. | Acesso | Essencial |
| RF-02 | Permitir localizar e navegar até a área de interesse no mapa. | Mapa | Importante |
| RF-03 | Criar, editar e excluir um polígono e avisar quando sua geometria for inválida. | Mapa | Essencial |
| RF-04 | Consultar POIs da Google Places API e/ou da base importada, conforme a fonte escolhida pelo usuário, e aplicar o recorte da área. | Consulta | Essencial |
| RF-05 | Permitir ao usuário selecionar tipos de POI, fonte da consulta e filtros sem alterar a área original. | Consulta | Essencial |
| RF-06 | Mostrar total, distribuição por tipo e densidade por km² dos POIs retornados na consulta. | Indicadores | Essencial |
| RF-07 | Identificar POIs do segmento escolhido entre os resultados retornados e mostrar sua distribuição. | Análise | Essencial |
| RF-08 | Comparar duas áreas sob os mesmos tipos, filtros e fonte; registrar horário da consulta e versão importada, quando houver. | Análise | Importante
| RF-09 | Salvar e reabrir área, tipos, filtros, fontes e horário da consulta; registrar a versão da base importada quando utilizada. | Histórico | Importante |
| RF-10 | Exportar indicadores e dados autorizados pela fonte em CSV e um resumo em PDF. | Resultados | Essencial |
| RF-11 | Compartilhar uma análise em modo de leitura com outro usuário autenticado. | Resultados | Desejável |
| RF-12 | Permitir ao administrador importar, validar e publicar uma base geoespacial opcional. | Dados | Essencial |
| RF-13 | Mostrar origem e horário da consulta; para dados importados, mostrar também a versão e a data da base. | Dados | Essencial |
| RF-14 | Mostrar estado e resultado das análises executadas em fila. | Processamento | Essencial |
## 2.2. Requisitos Não-Funcionais
A tabela reúne metas de qualidade e condições de verificação. A meta de cinco
segundos depende da definição do cenário de carga de aceitação.<br>
| ID | Descrição | Categoria | Escopo | Prioridade |
|----|-----------|-----------|--------|------------|
| RNF-01 | Exibir a consulta geoespacial em até 5 segundos no cenário de carga
definido para aceitação. | Desempenho | Geral | Importante |
| RNF-02 | Exigir MFA e proteger o tráfego da aplicação por conexão criptografada. | Segurança | Geral | Importante |
| RNF-03 | Restringir importação/publicação ao administrador e compartilhamentos aos destinatários definidos. | Autorização | Geral | Importante |
| RNF-04 | Rejeitar POIs sem identificador, categoria ou coordenadas válidas no processo de importação. | Qualidade de dados | Geral | Importante |
| RNF-05 | Separar interface, API, consulta espacial e processamento das
análises. | Manutenibilidade | Geral | Importante |
| RNF-06 | Oferecer mensagens compreensíveis para área inválida, ausência de
resultados e falha de tarefa. | Usabilidade | Geral | Importante |
| RNF-07 | Registrar quem publicou cada versão da base e quando a publicação
ocorreu. | Auditabilidade | Geral | Importante |

## 2.3. Regras de Negócio
As regras seguintes fixam o significado das consultas, dos indicadores e da publicação dos dados importados.<br>
| ID | Descrição | Escopo | Prioridade |
|---|---|---|---|
| RN-01 | Um polígono válido tem pelo menos três vértices distintos e não possui auto-interseção. | Geral | Essencial |
| RN-02 | POIs no limite do polígono entram no resultado; duplicatas por ID são removidas em cada fonte. Possíveis equivalências entre fontes são sinalizadas. | Geral | Essencial |
| RN-03 | Concorrente é um POI do tipo ou segmento escolhido entre os resultados da fonte selecionada; o sistema não estima potencial comercial. | Geral | Essencial |
| RN-04 | Densidade de POIs é a quantidade dividida pela área do polígono em km²; áreas sem medida válida não geram densidade. | Geral | Essencial |
| RN-05 | Uma análise salva conserva área, tipos, filtros, fonte e horário. A versão é registrada somente para bases importadas. | Geral | Essencial |
| RN-06 | Tarefas assíncronas seguem os estados Criada, Na fila, Em execução e um estado final: Concluída, Falhou ou Cancelada. | Geral | Essencial |
| RN-07 | Exportação e compartilhamento conservam o recorte e a origem da análise; conteúdo da API respeita as políticas do provedor. | Geral | Essencial |
| RN-08 | Uma versão importada torna-se ativa somente após validação e aprovação do administrador. | Geral | Essencial |

## 3. Histórias de Usuário (User Story)
Os requisitos funcionais são descritos a seguir em histórias com critérios de aceitação verificáveis.<br>
<table>
  <tr>
    <td><strong>Título:</strong> Selecionar e consultar área<br>(RF-03/RF-04)</td>
    <td><strong>Prioridade:</strong> Essencial</td>
    <td><strong>Estimativa:</strong> a definir</td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>História de Usuário</strong><br><br>
      <strong>Como:</strong> analista<br><br>
      <strong>Eu quero:</strong> delimitar a região e escolher tipos e fonte de POIs<br><br>
      <strong>Para:</strong> consultar somente os pontos nela contidos.
    </td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>Critério de Aceitação:</strong><br>
      Dado um polígono válido, tipos e fonte escolhidos, a consulta mostra os POIs retornados dentro ou no limite. Um polígono auto-intersectado gera correção antes da busca.
    </td>
  </tr>
</table>

*Tabela 1 — História de usuário para seleção da área.*

<table>
  <tr>
    <td><strong>Título:</strong> Analisar concorrência<br>(RF-07)</td>
    <td><strong>Prioridade:</strong> Essencial</td>
    <td><strong>Estimativa:</strong> a definir</td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>História de Usuário</strong><br><br>
      <strong>Como:</strong> analista<br><br>
      <strong>Eu quero:</strong> examinar concorrentes de um segmento
      <strong>Para:</strong> consultar somente os pontos nela contidos.
    </td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>Critério de Aceitação:</strong><br>
      A tarefa mostra estado; ao concluir, apresenta quantidade e distribuição dos POIs do segmento.
    </td>
  </tr>
</table>

*Tabela 2 - História de usuário para análise de concorrência.*

<table>
  <tr>
    <td><strong>Título:</strong> Comparar áreas<br>(RF-08)</td>
    <td><strong>Prioridade:</strong> Importante </td>
    <td><strong>Estimativa:</strong> a definir</td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>História de Usuário</strong><br><br>
      <strong>Como:</strong> analista<br><br>
      <strong>Eu quero:</strong> comparar duas áreas com a mesma fonte e filtros<br><br>
      <strong>Para:</strong> interpretar diferenças.
    </td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>Critério de Aceitação:</strong><br>
      As duas áreas exibem total e densidade sob os mesmos filtros e fonte, com horário da consulta e versão importada quando aplicável.
    </td>
  </tr>
</table>

*Tabela 3 - História de usuário para comparação de áreas.*

<table>
  <tr>
    <td><strong>Título:</strong> Salvar e exportar<br>(RF-09/RF-10)</td>
    <td><strong>Prioridade:</strong> Essencial</td>
    <td><strong>Estimativa:</strong> a definir</td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>História de Usuário</strong><br><br>
      <strong>Como:</strong> analista<br><br>
      <strong>Eu quero:</strong> salvar e exportar uma análise<br><br>
      <strong>Para:</strong> consultar o mesmo recorte depois.
    </td>
  </tr>
  <tr>
    <td colspan="3">
      <strong>Critério de Aceitação:</strong><br>
      Ao reabrir ou exportar, área, tipos, filtros e fonte correspondem ao registro salvo; conteúdo da API segue as permissões do provedor.
    </td>
  </tr>
</table>

*Tabela 4 - História de usuário para reutilização dos resultados.*

# 4. Perspectivas / Níveis de Detalhamento
Os objetivos dos usuários se desdobram em capacidades do sistema e em
comportamentos verificáveis de software. A matriz ao final da seção registra os
vínculos principais.
## 4.1 Requisitos de Usuário

Requisitos de usuário expressam os resultados que analistas e administradores
esperam obter.<br>
| ID | Requisito de usuário | Prioridade |
|---|---|---|
| RU-01 | Delimitar uma região, escolher tipos e fonte, e explorar os POIs retornados. | Essencial |
| RU-02 | Analisar segmentos, concorrência e diferenças entre duas áreas. | Essencial |
| RU-03 | Retomar, exportar e compartilhar critérios e resultados conforme as permissões da fonte. | Essencial |
| RU-04 | Conhecer a origem dos POIs e garantir qualidade e rastreabilidade das bases importadas. | Essencial |
## 4.2 Requisitos de Sistema
Requisitos de sistema traduzem esses objetivos em capacidades da plataforma.<br>
| ID | Requisito de sistema | Origem | Prioridade |
|---|---|---|---|
| RS-01 | Mapa com navegação e polígono validado. | RU-01 | Essencial |
| RS-02 | Consulta espacial à Google Places API e/ou à base importada com tipos e filtros. | RU-01 | Essencial |
| RS-03 | Indicadores territoriais e densidade. | RU-02 | Essencial |
| RS-04 | Análise de concorrentes e comparação de áreas. | RU-02 | Essencial |
| RS-05 | Persistência dos critérios, exportação permitida e compartilhamento. | RU-03 | Essencial |
| RS-06 | Execução e acompanhamento de tarefas assíncronas. | RU-02 | Essencial |
| RS-07 | Integração com Google Places API e importação opcional com validação e versionamento. | RU-04 | Essencial |
| RS-08 | Autenticação, autorização e auditoria. | RU-03, RU-04 | Essencial |
## 4.3 Requisitos de Software
Requisitos de software detalham operações implementáveis e rastreáveis.<br>
| ID | Requisito de software | Origem | Prioridade |
|---|---|---|---|
| RSW-01 | Localizar área no mapa e registrar os vértices do polígono. |  RS-01 | Essencial |
| RSW-02 | Validar número de vértices e auto-interseção. | RS-01 | Essencial |
| RSW-03 | Consultar Google Places API e/ou base importada; filtrar  coordenadas retornadas pelo polígono. | RS-02 | Essencial |
| RSW-04 | Aplicar tipos e filtros escolhidos; remover duplicatas por identificador em cada fonte. | RS-02 | Essencial |
| RSW-05 | Calcular total e agrupamento por categoria. | RS-03 | Essencial |
| RSW-06 | Calcular área e densidade em km². | RS-03 | Essencial |
| RSW-07 | Identificar POIs do segmento selecionado. | RS-04 | Essencial |
| RSW-08 | Aplicar tipos, filtros e fonte idênticos a duas áreas e registrar o horário. | RS-04 | Essencial |
| RSW-09 | Salvar área, tipos, filtros, fonte, horário e versão da base importada, quando houver. | RS-05 | Essencial |
| RSW-10 | Gerar CSV/PDF apenas com indicadores e campos cuja exportação seja permitida. | RS-05 | Essencial |
| RSW-11 | Criar tarefa e atualizar seus estados. | RS-06 | Essencial |
| RSW-12 | Entregar tarefas da fila ao processador e consultar as fontes selecionadas. | RS-06 | Essencial
| RSW-13 | Validar identificador, coordenadas e categoria na importação opcional. | RS-07 | Essencial |
| RSW-14 | Publicar versão importada aprovada com fonte, licença e data. | RS-07 | Essencial |
| RSW-15 | Integrar SSO e verificar perfil do usuário. | RS-08 | Essencial |
| RSW-16 | Registrar autor e data de publicação da base. | RS-08 | Essencial |

### Matriz de rastreabilidade
| Usuário | Sistema | Funcional | Software |
|---|---|---|---|
| RU-01 | RS-01, RS-02 | RF-02 a RF-05 | RSW-01 a RSW-04 |
| RU-02 | RS-03, RS-04, RS-06 | RF-06 a RF-08, RF-14 | RSW-05 a RSW-08, RSW-11, RSW-12 |
| RU-03 | RS-05, RS-08 | RF-01, RF-09 a RF-11 | RSW-09, RSW-10,RSW-15 |
| RU-04 | RS-07, RS-08 | RF-12, RF-13 | RSW-13, RSW-14, RSW-16 |

# 5. Diagramas UML
## 5.1. Diagramas de Casos de Uso


![Figura 1 — diagrama](Casos%20de%20uso%20do%20analista.jpg)
<br>*Figura 1 - Casos de uso do analista.*
## 5.2. Especificação de Casos de Uso
As tabelas preservam o padrão de fluxo principal, pré-condições, pós-condições e restrições do modelo.<br>
<table>
  <tr>
    <th>Nome do caso de uso</th>
    <td>UC-01 - Definir área</td>
  </tr>
  <tr>
    <th>Caso de uso geral</th>
    <td>Fluxo principal</td>
  </tr>
  <tr>
    <th>Ator principal</th>
    <td>Analista</td>
  </tr>
  <tr>
    <th>Atores secundários</th>
    <td>Nenhum</td>
  </tr>
  <tr>
    <th>Resumo</th>
    <td>O usuário delimita a região que servirá de base para a consulta.</td>
  </tr>
  <tr>
    <th>Pré-Condições</th>
    <td>Mapa disponível e usuário autenticado.</td>
  </tr>
  <tr>
    <th>Pós-Condições</th>
    <td>Área válida registrada com seus vértices.</td>
  </tr>
  <tr>
    <th>Ações do Ator</th>
    <th>Ações do Sistema</th>
  </tr>
  <tr>
    <td>1. Localizar a região no mapa.</td>
    <td></td>
  </tr>
  <tr>
    <td>2. Desenhar ou editar o polígono.</td>
    <td></td>
  </tr>
  <tr>
    <td>3. Confirmar a seleção.</td>
    <td>3. Validar vértices e geometria.</td>
  </tr>
  <tr>
    <td></td>
    <td>4. Registrar a área e suas coordenadas.</td>
  </tr>
  <tr>
    <td></td>
    <td>5. Exibir a área pronta para consulta.</td>
  </tr>
  <tr>
    <th>Restrições/Validações</th>
    <td>Polígono com menos de três vértices distintos ou auto-interseção não pode ser confirmado.</td>
  </tr>
</table>

<p align="center"><em>Tabela 5 - Especificação do caso de uso Definir Área.</em></p>

<table>
  <tr>
    <th>Nome do caso de uso</th>
    <td>UC-02 - Consultar pontos de interesse</td>
  </tr>
  <tr>
    <th>Caso de uso geral</th>
    <td>Fluxo principal</td>
  </tr>
  <tr>
    <th>Ator principal</th>
    <td>Analista</td>
  </tr>
  <tr>
    <th>Atores secundários</th>
    <td>Nenhum</td>
  </tr>
  <tr>
    <th>Resumo</th>
    <td>O sistema consulta as fontes escolhidas pelo usuário e apresenta os POIs retornados na área.</td>
  </tr>
  <tr>
    <th>Pré-Condições</th>
    <td>Área válida selecionada.</td>
  </tr>
  <tr>
    <th>Pós-Condições</th>
    <td>Resultado identificado por área, tipos, filtros, fonte e horário; versão se houver base importada.</td>
  </tr>
  <tr>
    <th>Ações do Ator</th>
    <th>Ações do Sistema</th>
  </tr>
  <tr>
    <td>1. Escolher tipos de POI, fontes e filtros.</td>
    <td></td>
  </tr>
  <tr>
    <td>2. Solicitar consulta.</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>3. Consultar API e/ou versão ativa da base importada.</td>
  </tr>
  <tr>
    <td></td>
    <td>4. Filtrar POIs retornados pelo polígono.</td>
  </tr>
  <tr>
    <td></td>
    <td>5. Mostrar marcadores, total e distribuição.</td>
  </tr>
  <tr>
    <th>Restrições/Validações</th>
    <td>Consulta vazia mostra total zero. Resultados da API podem ser limitados; dados do Google obedecem às políticas do provedor.</td>
  </tr>
</table>

<p align="center"><em>Tabela 6 - Especificação do caso de uso Consultar Pontos de Interesse.</em></p>

<table>
  <tr>
    <th>Nome do caso de uso</th>
    <td>UC-03 - Analisar concorrência</td>
  </tr>
  <tr>
    <th>Caso de uso geral</th>
    <td>Fluxo principal</td>
  </tr>
  <tr>
    <th>Ator principal</th>
    <td>Analista</td>
  </tr>
  <tr>
    <th>Atores secundários</th>
    <td>Nenhum</td>
  </tr>
  <tr>
    <th>Resumo</th>
    <td>O sistema calcula a quantidade e distribuição dos concorrentes no segmento escolhido.</td>
  </tr>
  <tr>
    <th>Pré-Condições</th>
    <td>Consulta concluída e segmento selecionado.</td>
  </tr>
  <tr>
    <th>Pós-Condições</th>
    <td>Análise concluída ou estado final de falha/cancelamento informado.</td>
  </tr>
  <tr>
    <th>Ações do Ator</th>
    <th>Ações do Sistema</th>
  </tr>
  <tr>
    <td>1. Solicitar análise.</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>2. Criar tarefa e informar seu estado.</td>
  </tr>
  <tr>
    <td></td>
    <td>3. Processar a tarefa e exibir o resultado.</td>
  </tr>
  <tr>
    <th>Restrições/Validações</th>
    <td>A tarefa segue a fila; considera apenas POIs do tipo escolhido nas fontes selecionadas e registra a origem.</td>
  </tr>
</table>

<p align="center"><em>Tabela 7 - Especificação do caso de uso Analisar Concorrência.</em></p>

## 5.3. Diagrama de Sequência
As três sequências mostram a consulta espacial, o processamento assíncrono e a
exportação do resultado salvo.

![Figura 2 — diagrama](Sequência%20da%20consulta%20espacial.jpg)
<br>*Figura 2 - Sequência da consulta espacial.*
### Como funciona o fluxo da consulta
1. O analista confirma um polígono válido e os filtros na interface.
2. A interface envia área e filtros para a API.
3. A API solicita a consulta ao serviço espacial.
4. A API consulta Google Places e/ou a base importada, conforme a escolha do
usuário.<br>
   4.1. As fontes retornam os POIs candidatos; o serviço aplica o polígono aos
resultados.<br>
   4.2. O serviço remove duplicatas por identificador em cada fonte e calcula os
totais dos POIs retornados na área.
5. A API devolve os resultados com origem e horário; inclui a versão quando a
fonte é importada.

6. A interface mostra marcadores e indicadores ao analista.

![Figura 3 — diagrama](Sequência%20da%20análise%20assíncrona.jpg)
<br>*Figura 3 - Sequência da análise de concorrência em fila.*


![Figura 4 — diagrama](Sequência%20da%20exportação.jpg)
<br>*Figura 4 - Sequência da exportação de resultados.*
## 5.4. Diagrama de Atividades

![Figura 5 — diagrama](Atividades%20da%20análise%20setorial.jpg)
<br>*Figura 5 - Atividades da análise territorial.*

A validação antecede a consulta. O fluxo representado continua com análise detalhada e
utilização do resultado.
## 5.5. Modelos Complementares
Os modelos seguintes detalham a importação opcional, as fontes de POIs, os componentes e
os estados da tarefa. Eles estendem o conjunto mínimo de diagramas previsto no modelo.
### 5.5.1. Casos de Uso da Gestão de Dados

![Figura 6 — diagrama](Caso%20de%20uso%20da%20gestão%20de%20dados.jpg)
<br>*Figura 6 - Casos de uso do administrador de dados.*
### 5.5.2. Modelo Conceitual de Dados

![Figura 7 — diagrama](Modelo%20conceitual%20de%20dados.jpg)
<br>*Figura 7 - Entidades centrais e cardinalidades.*

### 5.5.3. Componentes

![Figura 8 — diagrama](Arquitetura%20lógica%20proposta.jpg)
<br>*Figura 8 - Arquitetura lógica proposta.*
### 5.5.4. Estados da Análise

![Figura 9 — diagrama](Estados%20de%20uma%20terefa%20de%20análise.jpg)
<br>*Figura 9 - Estados de uma tarefa de análise.*


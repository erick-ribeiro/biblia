# Plano de Testes de API 

## Objetivo:

...

## Descrição da API:

...


## Escopo:

- Testes Funcionais:
  - Verificar a corretude e funcionalidade de todos os endpoints da API de acordo com a documentação da API.
  - Testar vários cenários para criação, modificação e deleção.
  - Validar mecanismos de autenticação e autorização de usuários para endpoints protegidos.

- Testes de Validação de Dados:
  - Garantir que a API valide corretamente os dados de entrada, rejeitando solicitações inválidas.
  - Testar valores de limite para campos de entrada para verificar qualquer comportamento inesperado.
  - Validar a precisão dos dados retornados nas respostas.

- Testes de Tratamento de Erros:
  - Verificar que códigos e mensagens de erro apropriados são retornados para solicitações inválidas.
  - Verificar respostas de erro quanto à divulgação de informações sensíveis.
  - Validar a capacidade da API de tratar erros inesperados de maneira apropriada.

- Testes de Performance:
  - Avaliar o tempo de resposta da API sob cargas normais e de pico para identificar gargalos potenciais.
  - Medir a taxa de transferência e escalabilidade da API para lidar com solicitações simultâneas.

- Testes de Segurança:
  - Realizar avaliações de segurança para identificar vulnerabilidades, como injeção de SQL, XSS, etc.
  - Validar a conformidade da API com práticas seguras de transmissão de dados (por exemplo, HTTPS).
  - Verificar controles de acesso adequados para prevenir o acesso não autorizado a recursos sensíveis.

- Testes de Integração:
  - Verificar interações entre diferentes endpoints e serviços da API.
  - Testar a consistência de dados em endpoints relacionados.

- Testes de Compatibilidade:
  - Testar a API em diferentes plataformas, navegadores e dispositivos para garantir compatibilidade.

- Testes de Regressão:
  - Realizar testes de regressão após correções de bugs ou atualizações para garantir que a funcionalidade existente permaneça intacta.

- Testes de Concorrência:
  - Avaliar o comportamento da API quando vários usuários tentam acessar e modificar reservas simultaneamente.

- Testes de Usabilidade:
  - Avaliar a facilidade de uso e amigabilidade da API do ponto de vista de um desenvolvedor.

- Testes de Performance:
  - Implementar monitoramento para rastrear o desempenho da API em tempo real.

- Testes de Carga:
  - Avaliar o comportamento da API sob alta carga de usuários simultâneos para garantir estabilidade.

- Testes de Limite de Taxa:
  - Verificar a aderência da API às regras de limitação de taxa para prevenir abusos.

- Testes de Integração de Terceiros:
  - Validar qualquer integração de terceiros para um funcionamento suave.

- Revisão da Documentação:
  - Avaliar a clareza, completude e precisão da documentação da API.
  - Verificar se a documentação da API está sincronizada com o comportamento real da API.


## Ambiente de Teste:

- Os sistemas operacionais e versões que serão usados para testes, como Windows
10, MacOS ou Linux.

- Os navegadores e versões que serão usados para testes, como Google Chrome,
Mozilla Firefox ou Microsoft Edge.

- Os tipos de dispositivos e tamanhos de tela para teste, como computadores de mesa, laptops,
tablets e smartphones.

- A conectividade de rede e largura de banda que estarão disponíveis para testes, como
Wi-Fi, celular ou conexão com fio.

- Os requisitos de hardware e software para executar os casos de teste, como específicos
processador, memória ou capacidade de armazenamento.

- Os protocolos de segurança e métodos de autenticação que serão usados para acessar o
ambiente de teste, como senhas, tokens ou certificação.

As permissões de acesso e o papel dos membros da equipe que estarão usando o ambiente de teste, como testadores, desenvolvedores ou stakeholders.
<!-- Imagem de Tabela -->

## Dados de Teste:

- Dados de teste (válidos, inválidos) podem ser criados manualmente pela equipe de teste para cobrir
cenários específicos e casos de borda.

- Scripts de automação podem ser usados para gerar grandes volumes de dados de teste de forma rápida e
eficiente.

- Dados de teste podem ser criados manualmente ou automaticamente com base nos esquemas da API.
- Ao testar integrações de API, as APIs mock podem fornecer dados sintéticos para testar
vários cenários.

- Para casos de uso específicos, provedores de dados de terceiros podem ser usados para obter dados do mundo real
para testes.

- Em alguns casos, dados higienizados do ambiente de produção podem ser usados para
testes para replicar cenários do mundo real.

## Estratégia de Teste:
### Passo-1:

- O primeiro passo é criar cenários de teste e casos de teste para as várias funcionalidades em
escopo. Ao desenvolver casos de teste, utilizaremos várias técnicas de design de testes.
  - Partição de Classe de Equivalência
  - Análise de Valor de Limite
  - Teste de Tabela de Decisão
  - Teste de Transição Estática
  - Teste de Casos de Uso

- Também usamos nossa experiência na criação de Casos de Teste aplicando o seguinte:
  - Suposição de Erro
  - Teste Exploratório

- Também priorizamos os casos de teste.

### Passo-2:

- Primeiro, realizaremos o Teste de Fumaça para verificar se as várias e importantes
funcionalidades da aplicação estão funcionando.

- Rejeitamos a build, se o Teste de Fumaça falhar e esperaremos pela build estável
antes de realizar testes aprofundados das funcionalidades.

- Uma vez que recebemos uma build estável, que passa no Teste de Fumaça, realizamos testes aprofundados
usando os Casos de Teste criados.

- Vários Recursos de Teste estarão testando a mesma aplicação em Múltiplos
Ambientes simultaneamente.

- Então reportamos os Bugs na ferramenta de Rastreamento de Bugs e enviamos para a gestão de desenvolvimento naquele
dia.

- Como parte dos testes, realizaremos os seguintes tipos de testes:
  - Teste de Fumaça e Teste de Sanidade
  - Teste de Regressão e Re-teste
  - Teste de Usabilidade, Teste de Funcionalidade

- Repetimos o Ciclo de Testes até garantir a qualidade.

### Passo-3:

Seguiremos as melhores práticas abaixo para melhorar nossos testes:

- Teste Dirigido por Contexto 
  - Realizaremos os testes de acordo com o contexto da
aplicação.
Shift Left Testing 
  - Começaremos os testes desde as primeiras fases do
desenvolvimento, em vez de esperar pela build estável.

- Teste Exploratório 
  - Usando nossa experiência, realizaremos o Teste Exploratório,
além da execução normal dos casos de teste.

- Teste de Fluxo de Ponta a Ponta 
  - Testaremos o cenário de ponta a ponta que envolve
múltiplas funcionalidades para simular os fluxos de usuários finais.

## Procedimento de Relatório de Defeitos:

- Os critérios para identificar um defeito, como desvio dos requisitos, problemas de
experiência do usuário ou erros técnicos.

- As etapas para relatar um defeito, como usar um modelo designado, fornecer
passos detalhados para reprodução e anexar capturas de tela ou logs.

- O processo para triagem e priorização de defeitos, como atribuir severidade e
níveis de prioridade, e atribuí-los aos membros da equipe apropriados para
investigação e resolução.

- As ferramentas (JIRA/Test Rail) serão usadas para rastrear e gerenciar defeitos.

- As funções e responsabilidades dos membros da equipe envolvidos no processo de relatório de defeitos,
como testadores, desenvolvedores e o líder de teste.

- Os canais de comunicação e frequências para atualizar os stakeholders sobre o
progresso e status dos defeitos.

- As métricas que serão usadas para medir a eficácia do processo de relatório de defeitos,
como o número de defeitos encontrados, o tempo levado para resolvê-los e
a porcentagem de defeitos que foram corrigidos com sucesso.

Defeito e Pessoa de Relatório:
<!-- Imagem de Tabela -->

## Cronograma de Teste:

Duas sprints (Uma Sprint = 5 dias úteis) serão necessárias para testar a aplicação.
A seguir, está o cronograma de teste planejado para o projeto –

Duração do Tempo da Tarefa
<!-- Imagem de Tabela -->


## Coberturas de Teste:

A cobertura de teste garante que todas as funcionalidades relevantes da API sejam adequadamente testadas. Ela mede a
eficácia e a abrangência do esforço de teste. Critérios de cobertura para teste de API:

- Cobertura Funcional:
  - Este critério foca no teste de todos os requisitos funcionais da API como
especificado em sua documentação e design.
  - Cada endpoint e método devem ser testados com várias entradas válidas e inválidas para
verificar seu comportamento e respostas.

- Cobertura de Validação de Dados:
  - Este critério se concentra em validar a correção dos dados retornados pela API.

- Coberturas de Tratamento de Erros:
  - Diferentes cenários de erro, como entradas inválidas, erros de servidor e respostas de
limitação de taxa, devem ser incluídos no teste.
  - A API deve ser totalmente testada para garantir que ela lida com erros e fornece
mensagens de erro adequadas com códigos de status corretos.

- Coberturas de Desempenho:
  - Casos de teste devem ser criados para simular várias cargas de usuário e medir os
tempos de resposta e o uso de recursos da API.

- Coberturas de Regressão:
  - Casos de teste selecionados de vários critérios de cobertura são reutilizados para validar as
partes inalteradas da API.

## Risco e Mitigação:
Abaixo estão alguns riscos comuns e estratégias de mitigação para lidar com eles.

| Nº | Risco | Mitigação |
|----|-------|-----------|
| 1  | Documentação de API pobre ou desatualizada | Priorizar a obtenção de documentação de API atualizada e precisa |
| 2  | Mensagens de erro não claras | Implementar teste negativo extenso para validar cenários de tratamento de erros |
| 3  | Não disponibilidade de um Recurso | Planejamento de Recurso de Backup |
| 4  | APIs que dependem de APIs externas ou serviços podem introduzir incertezas devido a fatores além da API sendo testada | Use APIs mock ou serviços virtualizados para simular o comportamento de dependências externas durante o teste |
| 5  | Mudanças ou atualizações na API podem introduzir regressões que afetam funcionalidades existentes ou quebram a compatibilidade com aplicações dependentes | Execute testes de regressão após cada atualização e lançamento de versão para garantir compatibilidade retroativa e identificar problemas de regressão prontamente |
| 6  | Menos tempo para testes | Aumente os recursos com base nas necessidades do Cliente dinamicamente |
| 7  | Comunicação e colaboração insuficientes entre testadores, desenvolvedores e stakeholders podem resultar em mal-entendidos, requisitos perdidos | Promova canais de comunicação abertos e mantenha reuniões regulares entre as equipes de teste e desenvolvimento. Envolva stakeholders no início do processo de teste para obter feedback e esclarecer requisitos |

## Ferramentas de Teste:

- Postman: Teste de API
- Newman: Automação de API & Relatório de Teste
- JMeter: Carga & Teste de Desempenho
- Burp Suite: Teste de Segurança
- JIRA: Ferramentas de Rastreamento de Bugs
- Light short: Ferramentas de Captura de Tela
- Loom: Gravação de Tela
- MS Word & Excel: Documentação

## Funções e Responsabilidades:

### Líder SQA
- Desenvolver plano de teste
- Definir o escopo e os objetivos
- Atribuir tarefas à equipe de teste e monitorar o progresso
- Revisar e aprovar casos de teste e dados de teste
- Relatar o progresso e os resultados dos testes para os stakeholders

### Engenheiro SQA
- Criar, gerenciar e manter casos de teste
- Criar, gerenciar e manter dados de teste e garantir que os
dados de teste são relevantes, precisos e cobrem vários cenários
- Colaborar com testadores de API para entender os requisitos de dados
- Fornecer feedback para o líder do QA sobre o progresso dos testes
e desafios.
- Desenvolver e manter scripts de teste automatizados para teste de API.

### Engenheiro SQA Júnior
- Executar casos de teste e registrar resultados de testes.
- Validar funcionalidade, desempenho e aspectos de segurança da API
- Identificar, relatar e rastrear defeitos em um sistema de rastreamento de defeitos
- Colaborar com desenvolvedores para reproduzir e verificar defeitos relatados.

## Entregáveis de Teste:
Os seguintes devem ser entregues ao cliente:

### Plano de Teste 
- Detalhes sobre o escopo do projeto, estratégia de teste,
cronograma de teste e entregáveis de teste.
### Teste Funcional / Casos
- Casos de teste criados para o escopo definido.
### Relatório de Defeitos 
- Uma descrição detalhada dos defeitos identificados juntamente
com capturas de tela e passo a passo para reproduzir em uma base diária.
### Relatório Resumo 
- Relatório resumo do projeto

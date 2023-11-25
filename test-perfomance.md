TODO: Ajustar este titulo
# Teste de perfomance com o K6
Esta documentação ajudará você a evoluir de um iniciante total para um expert em teste de perfomace!


TODO: Ajustar esta seção para não falar tanto de forma expecifica do K6
## O que é o k6?
Grafana k6 é uma ferramenta de teste de carga open-source que torna os testes de desempenho fáceis e produtivos para equipes de engenharia. O k6 é gratuito, centrado no desenvolvedor e extensível.

Usando o k6, você pode testar a confiabilidade e o desempenho de seus sistemas e identificar regressões de desempenho e problemas mais cedo. O k6 ajudará você a construir aplicações resilientes e de alto desempenho que escalam.

O k6 é desenvolvido pela Grafana Labs e pela comunidade.

## Principais recursos
O k6 é repleto de recursos, sobre os quais você pode aprender em toda a documentação. Os principais recursos incluem:

Ferramenta CLI com APIs amigáveis para desenvolvedores.
Scripting em JavaScript ES2015/ES6 - com suporte para módulos locais e remotos.
Checks e Thresholds - para testes de carga orientados a objetivos e amigáveis à automação.

## Casos de uso
Os usuários do k6 são geralmente Desenvolvedores, Engenheiros de QA, SDETs e SREs. Eles usam o k6 para testar o desempenho e a confiabilidade de APIs, microservices e websites. Casos de uso comuns do k6 são:


TODO: Ajustar esta seção para falar sobre cada tecnica e não sobre como o k6 se comporta com esta tecnica
### Teste de carga
O k6 é otimizado para consumo mínimo de recursos e projetado para executar testes de alta carga (testes de pico, stress e soak (testes que verificam a estabilidade de um sistema sob carga contínua por períodos prolongados)).

### Teste de navegador
Através do navegador k6, você pode executar testes de desempenho baseados em navegador e identificar problemas relacionados apenas aos navegadores, que podem ser completamente ignorados no nível do protocolo.

### Testes de caos e resiliência
Você pode usar o k6 para simular tráfego como parte de seus experimentos de caos, acioná-los a partir de seus testes k6 ou injetar diferentes tipos de falhas no Kubernetes com o xk6-disruptor.

### Monitoramento de desempenho e sintético
Com o k6, você pode automatizar e programar para disparar testes com muita frequência com uma pequena carga para validar continuamente o desempenho e a disponibilidade de seu ambiente de produção.

## Manifesto de Teste de Carga

TODO: Ajusar introdução sobre o manifest do teste
Nosso manifesto de teste de carga é o resultado de anos imersos até o pescoço nas trincheiras, fazendo testes de desempenho e carga. Criamos isso para ser usado como orientação, ajudando você a direcionar seus testes de desempenho corretamente!

### Testes simples são melhores do que não testar

Os testes de carga devem imitar os usuários e clientes do mundo real o mais próximo possível. Seja na distribuição de solicitações para sua API ou nas etapas de um usuário passando por um fluxo de compra. Mas, sejamos pragmáticos por um segundo. A regra 80/20 (Princípio de Pareto) afirma que você obtém 80% do valor a partir de 20% do trabalho e alguns testes simples são muito melhores do que não ter testes. Comece pequeno e simples, garanta que você tire algo do teste primeiro, depois expanda a suíte de testes e adicione mais complexidade até sentir que alcançou o ponto em que mais esforço em realismo não proporcionará um retorno suficiente sobre o tempo investido.

Existem dois tipos de testes de carga que você pode executar. O "teste de carga unitário" e o "teste de carga de cenário".

Um teste de carga unitário testa uma única unidade, como um endpoint de API, isoladamente. Você pode estar principalmente interessado em como o desempenho do endpoint varia ao longo do tempo e ser alertado para regressões de desempenho. Este tipo de teste tende a ser menos complexo do que o teste de carga de cenário (descrito abaixo) e, portanto, é um bom ponto de partida para seus esforços de teste. Construa uma pequena suíte de testes de carga unitária primeiro, depois, se sentir que precisa de mais realismo, pode entrar no teste de cenário.

Um teste de carga de cenário é para testar um fluxo real de interações, por exemplo, um usuário fazendo login, iniciando alguma atividade, aguardando progresso e, em seguida, fazendo logout. O objetivo aqui é testar o sistema alvo com tráfego que seja consistente com o que você veria no mundo real em termos de URLs/endpoints acessados. Geralmente, isso significa garantir que os fluxos mais críticos em seu aplicativo sejam performáticos. Um teste de carga de cenário é tão simples quanto compilar seus testes de carga unitários em alguma ordem lógica relacionada à forma como seus usuários interagem com seu serviço.

```javascript
import { check } from 'k6';
import http from 'k6/http';

export function testLogin(params) {
  let data = params || { username: 'admin', password: 'test' };
  http.post('http://demo.loadimpact.com/login', data);
}

export default function () {
  testLogin();
}
```

```javascript
import { group } from 'k6';
import { testLogin } from 'units/login';

let users = open('users.csv');

export default function () {
  group('Login', function () {
    let user = users.getRandom();
    testLogin({ username: user.username, password: user.password });
  });
}
```

### Testes de carga devem ser orientados a objetivos

Os testes de carga devem ser orientados a objetivos
Um teste de carga bem-sucedido precisa de um objetivo. Podemos ter SLAs formais para testar, ou simplesmente queremos que nossa API, aplicativo ou site responda instantaneamente (<=100ms de acordo com Jakob Nielsen). Todos nós sabemos o quão impacientes ficamos enquanto esperamos por um aplicativo ou site carregar.

É por isso que especificar metas de desempenho é uma parte tão importante do teste de carga, por exemplo, acima de qual nível um tempo de resposta não é aceitável e/ou qual é uma taxa aceitável de falha de solicitação. Também é uma boa prática garantir que seu teste de carga seja funcionalmente correto. Ambos os objetivos de desempenho e funcionais podem ser codificados usando thresholds e checks (como asserts).

Para automação de testes de desempenho, a ferramenta de teste de carga precisa ser capaz de sinalizar para o executor de tarefas (geralmente um servidor CI) se o teste foi aprovado ou reprovado.

Com seus objetivos definidos, é uma tarefa simples alcançar isso, pois seus thresholds irão, em caso de falha, fazer com que o k6 retorne um código de saída diferente de zero.

```javascript
import { check } from 'k6';
import http from 'k6/http';

export let options = {
  thresholds: {
    'http_req_duration{url:http://demo.loadimpact.com/login}': ['p95<100'],
  },
};

export function testLogin(params) {
  let data = params || { username: 'admin', password: 'test' };
  let res = http.post('http://demo.loadimpact.com/login', data);
  check(res, {
    'é status 200': (r) => r.status === 200,
  });
}

export default function () {
  testLogin();
}
```

### Teste de carga por desenvolvedores

Os testes de carga devem ser feitos pelas pessoas que conhecem melhor a aplicação, os desenvolvedores, e acreditamos que ferramentas para desenvolvedores devem ser de código aberto para permitir que uma comunidade se forme e impulsione o projeto através de discussões e contribuições. É por isso que construímos o k6, a ferramenta de teste de carga que sempre quisemos para nós mesmos!

### A experiência do desenvolvedor é super importante

#### Ambiente local
Como desenvolvedores, amamos nossa configuração local. Gastamos muito tempo e esforço para garantir que nosso editor de código favorito e o terminal estejam do jeito que queremos. Tudo o que foge disso é inferior, um obstáculo à nossa produtividade. O ambiente local é rei.

É onde devemos codificar nossos scripts de teste de carga e de onde devemos iniciar nossos testes de carga.

#### Tudo como código
DevOps nos ensinou que o processo de desenvolvimento de software pode ser generalizado e reutilizado para lidar com mudanças não apenas no código da aplicação, mas também na infraestrutura, documentação e testes. Tudo pode ser apenas código.

Realizamos o check-in de nosso código no ponto de entrada de um pipeline, controle de versão (Git e Github em nosso caso), e depois ele passa por uma série de etapas destinadas a garantir a qualidade e reduzir o risco de lançamentos. A automação nos ajuda a manter essas etapas fora do nosso caminho, mantendo o controle através de ciclos rápidos de feedback (a troca de contexto é nosso inimigo). Se qualquer etapa do pipeline quebrar (ou falhar), queremos ser alertados em nosso canal de comunicação preferido (no nosso caso, Slack), e isso precisa acontecer o mais rápido possível enquanto estamos no contexto certo.

Nossos scripts de teste de carga não são exceção. Acreditamos que os scripts de teste de carga devem ser simples códigos para obter todos os benefícios do controle de versão, ao contrário de, por exemplo, XML ilegível e gerado por ferramentas.

#### Automação

Os testes de carga podem ser facilmente adicionados como outra etapa no pipeline, pegando os scripts de teste de carga do controle de versão para serem executados. Para dizer a verdade, se qualquer etapa no pipeline demorar muito, corre o risco de ser "temporariamente" desativada ("só por agora, prometo"). Seja aquela construção de imagem Docker que leva uma eternidade, ou aquele teste de carga de 30 minutos ;-)

Sim, os testes de carga de cenário tradicionais naturalmente correm o risco de serem cortados em nome de "esta-etapa-está-demorando-demais", pois os testes de carga precisam de tempo para aumentar e executar as jornadas do usuário com o tráfego simulado para obter medidas suficientes que possam ser consideradas. É por isso que não recomendamos que testes de carga sejam executados a cada commit para testes de carga tipo cenário, mas sim na faixa de frequência de "diariamente" para testes de regressão de desempenho. Ao mesclar o código em um branch de lançamento ou como uma construção noturna, talvez, para que você possa ter seu elegante relatório de teste de carga com seu café da manhã antes de entrar no seu ritmo!

Para testes de carga unitários, onde apenas um ou poucos pontos de extremidade da API estão sendo testados, executar testes de regressão de desempenho a cada commit é apropriado.

### Teste de carga em um ambiente de pré-produção

Os testes de carga devem ocorrer em pré-produção. Testar em produção corre o risco de interromper as operações comerciais. Isso só deve ser feito se seus processos forem maduros o suficiente para suportá-lo, por exemplo, se você já estiver realizando engenharia de caos. ;)

Além disso, usar um produto de monitoramento de desempenho de aplicativos (APM) em produção não é motivo para evitar testes de carga.

Um APM não lhe dirá o quão escalável é o seu sistema. É "apenas" um monitoramento e observabilidade mais profundos.
A separação estrita entre os ambientes de produção e pré-produção torna inviável a realização de dumps e restaurações de banco de dados e, em certos ambientes regulatórios, impossível.
Limpar fontes de dados pode não ser trivial. A cobertura total é difícil de verificar. Não queremos enviar milhares de e-mails para clientes reais simplesmente porque limpamos os dados de forma inadequada!
Os sistemas de hoje rapidamente se tornam complexos, com muitas partes móveis. Ao realizar testes de carga, precisamos garantir que nosso sistema faça a coisa certa™ em termos de integrações de terceiros, como processadores de cartão de crédito e serviços de entrega de e-mail, que podem precisar de valores simulados ou de pré-produção na camada de dados.

## O que o k6 não faz

O k6 é uma ferramenta de teste de carga de alto desempenho, scriptável em JavaScript. O design arquitetônico para ter essas capacidades traz alguns trade-offs:

Não é executado nativamente em um navegador
Por padrão, o k6 não renderiza páginas da web da mesma forma que um navegador. Navegadores podem consumir recursos significativos do sistema. Ignorar o navegador permite executar mais carga dentro de uma única máquina.

No entanto, com o navegador k6, você pode interagir com navegadores reais e coletar métricas de frontend como parte de seus testes k6.

Não é executado no NodeJS
JavaScript geralmente não é bem adequado para alto desempenho. Para alcançar o máximo de desempenho, a ferramenta em si é escrita em Go, incorporando um runtime de JavaScript que permite fácil scripting de testes.

## Rodando o K6



Conversa com o SR. Perfomance do K6s

tendencias
    ferramentas novas são diferentes das ferramentas do passado
    antes tinhamos plataformas muito grande que fazia tudo muito bem
    porem agora temos ferramentas mais enxutas

    os testes de perfomance não é so do engenheiro e sim que todos possam contribuir as ferramentas tem que ser leve, "o rei artur nao deve ser o unico a levantar a espada"

    solucoes monoliticas e antes era muito importante fazer um teste de load e geralmente os testes eram enorme

    hoje temos uma onde mais continua mais agil e nesta onda ja temos uma aplicacao em producao nao estamos lancançado o big ban, estamos adicionando pequenas novas funcionalidades 

    cereja do bolo é observar, hoje temos tantas maneiras de observar o desempenho das aplicaões 


todo mundo quer um software que seja eficiente e resonda rapido
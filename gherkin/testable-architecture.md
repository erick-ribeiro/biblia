
Testes Automatizados e especificações executáveis só podem funcionar com uma arquitetura de software testável.
Uma arquitetura testável permite testes rápidos e confiáveis que são fáceis de escrever, executar e manter.

# Feedback rápido
Testes rápidos permitem que os desenvolvedores os executem frequentemente para obter feedback rápido sobre o que eles estão construindo, sem perder o foco ou o fluxo.

# "Testability"
Ao projetar a testabilidade, certifique-se de que seus produtos e serviços estejam compostos de componentes ou módulos loosely-coupled e bem encapsulados.

Desacloe o seu código de negócios da sua infraestrutura (os componentes geralmente lentos e frágeis),
o que permite executar testes em diferentes níveis com máxima confiança e mínimo custo.

* Certifique-se de poder executar seus testes sem passar pela interface do usuário (UI). Eles são lentos, frágeis, caros e difíceis de corrigir. Não dependa apenas de testes de interface do usuário.
* Executar testes em um banco de dados torna os testes lentos. Precisamos garantir que o banco de dados esteja no estado esperado antes de cada teste para que os testes se comportem de forma consistente.
Isso garantirá que os testes não interfiram entre si e possam ser executados em qualquer ordem.

Queremos que a maioria dos nossos testes use uma implementação de stub na memória em vez de um banco de dados real.
A partir da perspectiva do código de domínio, o comportamento parece exatamente o mesmo.
Para garantir que o stub funciona da mesma maneira que o real, precisamos ter confiança, que podemos obter usando [testes de contrato](https://martinfowler.com/bliki/IntegrationContractTest.html).

# "Ports and adapters"
Podemos pensar em nosso banco de dados e nosso stub como duas coisas que podem ser conectadas ao **port**. A aplicação que contém a lógica de negócios não precisa saber se está se comunicando com um stub ou com o banco de dados real. Isso porque ela está se comunicando com esse port e armazenando e recuperando dados. Podemos executar nossos testes usando tanto o stub quanto a implementação real. Isso nos dá confiança de que o stub se comporta como a coisa real.

A lógica de negócios está no núcleo da aplicação, completamente desacoplada de dispositivos e serviços externos. Ela não sabe nada sobre bancos de dados, filas de mensagens ou serviços web, pois isolamos esses através desses ports. Frequentemente, esses ports estão conectados a adapters que interagem com a implementação real. Haverá um adapter para o banco de dados, um adapter para a fila e um adapter para o serviço web. A interface do usuário define como podemos interagir com nosso sistema. Ela também terá um adapter, como um servidor web que se conecta a um port para exibir uma UI em um navegador. Todas as operações de IO tendem a acontecer fora desses ports. Ao testar a lógica de negócios central diretamente através dos ports, podemos eliminar muitas operações de IO lentas e frágeis.

Este padrão arquitetônico é chamado de **padrão ports and adapters** (ou [arquitetura hexagonal](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))). Ele permite que você conecte seus cenários e testes unitários em um nível mais baixo, enquanto os testes de contrato lhe dão a confiança para fazer isso.

# Full stack
Você vai querer executar *alguns* testes que percorram toda a profundidade da sua stack, para obter total confiança. Diagnosticar onde o problema está em um teste de ponta a ponta, full-stack, é realmente difícil, porque pode estar em qualquer lugar. Esses testes são frágeis porque uma mudança pode quebrar todos eles. Eles também são lentos, pois há IO envolvida ao passar por um navegador, serviço web ou banco de dados.

# Pirâmide de Testes
Foque em ter muitos tipos de testes: muitos testes unitários, poucos testes que não passam por todos os componentes de infraestrutura pesadas e poucos testes que passam pela UI.
Isso é chamado de [pirâmide de testes](https://martinfowler.com/bliki/TestPyramid.html), em inglês [test pyramid].

# Mais informações

Para obter um exemplo de como implementar testes rápidos (em node.js), veja [subsecondtdd no GitHub](https://github.com/subsecondtdd/todo-subsecond)

Ou veja a [palestra de Aslak no Devlin 2017](https://skillsmatter.com/skillscasts/9971-teste-de-arquitetura-de-software-confiável-com-aslak-hellesoy) ou os [slides](https://speakerdeck.com/aslakhellesoy/teste-de-arquitetura-de-software-confiavel-devlin-2017) sobre este assunto.


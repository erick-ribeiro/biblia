Sistemas de software são suscetíveis ao acúmulo de falhas na qualidade interna que complicam e dificultam futuras modificações e expansões no sistema. "Dívida Técnica" é uma metáfora, introduzida por Ward Cunningham, que orienta a forma como devemos pensar e lidar com estas "dividas tecnicas", equiparando-o a uma dívida financeira. O esforço adicional necessário para implementar novas funcionalidades representa os juros dessa dívida.

---

https://martinfowler.com/bliki/images/tech-debt/sketch.png

Parte Esquerda da Imagem:

"Any software system has a certain amount of essential complexity required to do its job...": Isso indica que todo software tem uma quantidade inerente de complexidade necessária para realizar suas funções principais. Esse é o aspecto fundamental de qualquer sistema.
"... but most systems contain cruft that makes it harder to understand.": Esta parte destaca que, além da complexidade essencial, a maioria dos sistemas tem "cruft" ou código desnecessário/redundante que torna o sistema mais difícil de entender.
Ilustração do Software:

O retângulo representando o software é dividido em duas partes:
A parte superior verde representa a "complexidade essencial" do software, ou seja, o código e as funcionalidades necessárias para que o software realize suas tarefas.
A parte inferior marrom representa o "cruft", o código ou a complexidade desnecessária que torna o software mais difícil de manter ou compreender.
Parte Direita da Imagem:

"Cruft causes changes to take more effort": Isso indica que, devido ao "cruft", qualquer mudança ou atualização no software requer mais esforço do que o ideal.
As setas apontando para cima junto com os triângulos representam esse esforço extra necessário para fazer mudanças no software devido ao "cruft".
Parte Inferior da Imagem:

"The technical debt metaphor treats the cruft as a debt, whose interest payments are the extra effort these changes require.": Esta parte relaciona o conceito de "cruft" com "dívida técnica". Assim como uma dívida financeira tem pagamentos de juros, o esforço extra necessário para fazer mudanças no software devido ao "cruft" é comparado aos juros pagos sobre a dívida técnica.

---

Imagine que eu tenha uma estrutura de módulos confusa na minha base de código. Preciso adicionar um novo recurso. Se a estrutura do módulo fosse clara, levaria quatro dias para adicionar o recurso, mas com esse cruft, leva seis dias. A diferença de dois dias é o juro da dívida.

O que mais me atrai na metáfora da dívida é como ela molda meu pensamento sobre como lidar com esse cruft. Eu poderia levar cinco dias para limpar a estrutura modular, removendo esse cruft, pagando metaforicamente o principal da dívida. Se eu fizer isso apenas para este recurso, não haverá ganho, pois levaria nove dias em vez de seis. Mas se eu tiver mais dois recursos semelhantes a seguir, então eu acabaria sendo mais rápido removendo o cruft primeiro.

Dito assim, isso soa como uma simples questão de trabalhar com os números, e qualquer gerente com uma planilha deveria descobrir as escolhas. Infelizmente, como não podemos medir produtividade objetivamente, nenhum desses custos é objetivamente mensurável. Podemos estimar quanto tempo leva para fazer um recurso, estimar como seria se o cruft fosse removido e estimar o custo de remoção do cruft. Mas nossa precisão dessas estimativas é bastante baixa.

Dado isso, geralmente o melhor caminho é fazer o que normalmente fazemos com dívidas financeiras: pagar o principal aos poucos. No primeiro recurso, gastarei alguns dias extras para remover parte do cruft. Isso pode ser suficiente para reduzir a taxa de juros em melhorias futuras para um único dia. Isso ainda vai levar tempo extra, mas ao remover o cruft, estou tornando mais barato para futuras mudanças neste código. O grande benefício de uma melhoria gradual como essa é que naturalmente passamos mais tempo removendo cruft nas áreas que modificamos com frequência, que são exatamente as áreas da base de código de onde mais precisamos remover o cruft.

Pensar nisso como pagar juros versus pagar o principal pode ajudar a decidir qual cruft abordar. Se eu tenho uma área terrível na base de código, uma que é um pesadelo para mudar, não é um problema se eu não preciso modificá-la. Só desencadeio um pagamento de juros quando tenho que trabalhar com aquela parte do software (este é um ponto onde a metáfora falha, já que os pagamentos de juros financeiros são desencadeados pela passagem do tempo). Portanto, áreas cruft, mas estáveis do código podem ser deixadas de lado. Em contraste, áreas de alta atividade precisam de uma atitude de tolerância zero para cruft, porque os pagamentos de juros são altamente prejudiciais. Isso é especialmente importante, pois o cruft se acumula onde os desenvolvedores fazem mudanças sem prestar atenção à qualidade interna - quanto mais mudanças, maior o risco de acúmulo de cruft.

A metáfora da dívida às vezes é usada para justificar a negligência com a qualidade interna. O argumento é que leva tempo e esforço para impedir o acúmulo de cruft. Se há novos recursos que são necessários com urgência, então talvez seja melhor assumir a dívida, aceitando que essa dívida terá que ser gerenciada no futuro.

O perigo aqui é que na maioria das vezes essa análise não é bem feita. O cruft tem um impacto rápido, desacelerando justamente os novos recursos que são necessários rapidamente. As equipes que fazem isso acabam estourando todos os seus cartões de crédito, mas ainda entregam mais tarde do que teriam feito se tivessem investido em maior qualidade interna. Aqui, a metáfora muitas vezes leva as pessoas a erros, já que a dinâmica não corresponde realmente àquela dos empréstimos financeiros. Assumir dívidas para acelerar a entrega só funciona se você permanecer abaixo da linha de compensação de design da Hipótese de Stamina de Design (https://martinfowler.com/bliki/DesignStaminaHypothesis.html), e as equipes atingem essa linha em semanas, e não meses.

Há debates regulares sobre se diferentes tipos de cruft devem ser considerados como dívida ou não. Achei útil pensar se a dívida é adquirida deliberadamente e se é prudente ou imprudente, levando-me ao Quadrante da Dívida Técnica.
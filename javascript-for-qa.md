## Função Callback

Vamos entender o que é uma função callback colocando a mão na massa:

1. Inicializar o NPM
  ```bash
  npm init -y
  ```
2. Instalar o Mocha
  Explicar porque vamos usar o mocha, o cypress ele utiliza dois frame de testes de unidade por baixo do pano que é o mocha e o chai. O Mocha para estrutura e o Chai para asserções.
  https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test#Write-your-first-test

4. Adicionar o Mocha no NPM Script
3. Criar a estrutura das Pastas `test/test.js`


Agora vamos executar o nosso primeiro teste, no cypress declaramos um teste chamando a função `it` isto é algo que temos em comum com o Mocha então vamos declarar o nosso primeiro teste aqui no Mocha.

```
it()
```

Executando este teste podemos observar que ele esta reclamando da ausencia de um argumento, de um parametro que se chama title e é do tipo string. Vamos corrigir preenchendo este argumento agora e rodar o teste novamente.

```
it('Nome do meu teste')
```

Pode ver que o nosso teste foi reconhecido agora porem ele foi automaticamente ignorado, este comportamento se da pq não estamos passando o nosso segundo argumento que seria uma função. Vamos passar o nome de uma função qualquer e testar.

```
it('Nome do meu teste', funcaoQualquer)
```

O erro que estamos tomando agora é que a nossa execução falhou pq a `funcaoQualquer` não foi definida, bora declarar esta função e testar novamente.

```
it('Nome do meu teste', funcaoQualquer)

function funcaoQualquer() {
  
}
```

Pronto agora o nosso primeiro teste que não testa nada esta passando, então vamos recapitular o que esta acontecendo, a gente chamou uma `funcao` com o nome de `it` passando dois parametro, o primeiro que é uma string aonde adicionamos o titulo do teste, e passamos no segundo parametro uma função que teoricamente era para ter o codigo do nosso teste em si.

O conceite de ter uma função (`it`) chamando outra função (`funcaoQualquer`) é o significado de callback function. Antes de aprofundarmos aqui vamos verificar se de fato esta função esta sendo chamada.

```
it('Nome do meu teste', funcaoQualquer)

function funcaoQualquer() {
  console.log('Esta função esta sendo chamada?')
}
```

como podemos observar de fato esta função foi chamada pois foi imprenso no nosso console a pergunta que adicionamos dentro da função.

Vamos agora refatorar este codigo deixando ele num modelo mais aplicavel para conseguirmos fixar que este teste chama de fato uma função callback, para isto vamos criar uma função anonima (como ela já é chamada diretamente por outra função ela não precisa de nome pois ela não vai ser referenciada posteriormente)

```
it('Nome do meu teste', function() {
  console.log('Esta é uma função CALLBACK!!')
})

function funcaoQualquer() {
  console.log('Esta função esta sendo chamada?')
}
```

Ao executar os testes vemos que funcionou então podemos considerar que para declarar uma função callback basta deixar o bloco da função no lugar do parametro. Mas existe uma melhor forma hoje de declarar uma função callback que é declarando uma arrow funtion. Que é basicamente so remover a palavra `function` e adicionar uma seta

```
it('Nome do meu teste', () => {
  console.log('Esta é uma arrow function CALLBACK!!')
})

function funcaoQualquer() {
  console.log('Esta função esta sendo chamada?')
}
```

Belezamos funcionou lindamente. Mas Erick porque a gente que meche com o cypress precisa saber disto?

- O Cypress é construída em torno do conceito de encadeamento de comandos, onde cada comando é executado em ordem e muitos comandos aceitam funções callback para realizar ações ou verificações. Por exemplo, ao usar .then() em um elemento, você fornece uma função callback que recebe o elemento como argumento e realiza alguma ação ou verificação nele.

Vamos para o Cypress brincar nele agora, se a gente for la no arquivo `steps/propostaStep.js` a gente vai ver que ja usamos este conceite de callback não somente nos it's e describe's, aplicamos eles em outras funções do cypress como por exemplo no `.then`, vamos fazer uns ajuste no `.then` para entender um pouco de como o cypress gerencia automaticamente funções callbacks.

cy.get(proposta.contentPropostaAteAgora).should('be.visible').then((textoProposta) => {
  console.log(textoProposta)
  
  expect(textoProposta.attr('data-cy')).to.contain('propValPropostaAndamento')
  expect(textoProposta.attr('class')).to.contain('valor-proposta')
  expect(textoProposta.text()).to.contain('R$ 6.390,89')

  cy.pause()
})


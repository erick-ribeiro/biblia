## Guia Definitivo para Automação de Testes de API com Playwright: Parte 3 - Depuração de Testes de API
Uma grande parte de escrever verificações automatizadas é testar e depurar o código que você escreve, garantindo, idealmente, que não haja bugs no código de teste. Na parte 3, vou abordar como executo meus testes, depuro meus testes e como obtenho feedback antes que o código seja mesclado no branch de código. Se você perdeu a Introdução, a Parte 1 ou a Parte 2, recomendo que confira para obter o contexto de onde estamos entrando.

O que vou abordar:
- Como eu uso o VS Code para depurar meus testes
- Usando o modo UI para depurar
- Usando arquivos de rastreamento do Playwright Test via Show-Report
- Usando console.log()/console.table()
- Como obtenho feedback via GitHub Action antes de mesclar o código

VS Code para Depurar Testes:
Quando estou escrevendo ou depurando testes em Playwright, geralmente uso a extensão Playwright Test para VS Code. Uma vez instalada, você pode selecionar a opção de Teste (ícone de béquer) e visualizar, depurar ou executar os testes que estão escritos. O interessante é que você pode executar testes individuais com um clique de botão.

Com a extensão instalada, você também verá botões verdes de play em cada um dos seus blocos de teste e describe, onde você pode executar as verificações diretamente do arquivo spec.ts.

Vou demonstrar como executar um teste no modo de depuração abaixo. Antes de executar o teste, adicionarei um breakpoint onde quero que o teste pause e abrirei o menu do debugger. A partir daí, posso ver informações sobre os testes que normalmente não veria. Isso é útil para tentar determinar a que informações as variáveis estão atribuídas. No meu exemplo, vou adicionar um ponto de depuração após atribuir a variável body para response.json().

No exemplo acima, você pode ver que consegui validar a estrutura dos dados retornados { bookings: [ { bookingDates: { checkin: "Date", checkout: "Date"} } ] }, junto com quaisquer dados específicos.

Usando o modo UI do Playwright
Para usar o modo UI, você precisará ter a versão 1.32 do Playwright em seu projeto. Para atualizar sua versão, é tão simples quanto visitar seu arquivo package.json, encontrar devDependencies > @playwright/test e atualizar o valor para 1.32 ou superior.

Se você não tiver certeza de qual é a versão mais recente, pode encontrá-la na página de Releases do GitHub. Você também pode seguir este guia para configurar uma notificação no Slack quando uma nova versão do Playwright for lançada.

Agora, sobre o modo UI do Playwright, você pode estar pensando: por que eu precisaria usar o modo UI quando estamos executando testes de API? Bem, você não precisa, mas é uma ótima maneira de executar e depurar testes e visualizar as requisições de rede conforme elas ocorrem, em vez de adicionar console.log ou usar o debugger do VSCode.

Para executar o Playwright no modo UI, adicione --ui ao final do seu comando CLI do Playwright. Para facilitar isso, eu adicionei uma entrada de script npm ao arquivo package.json. Se você estiver acompanhando, criei uma chave chamada ui e adicionei o comando npx playwright test --ui. Isso me permite executar npm run ui, e o comando npx playwright test --ui será executado a partir da linha de comando. Acho que criar certos scripts npm é realmente útil para transformar comandos CLI longos em algo que você pode digitar rapidamente.

```json
{
  "name": "playwright-api-test-demo",
  "version": "1.0.0",
  "description": "Este repositório servirá como um local onde adicionarei verificações de automação de testes de API para artigos escritos em <https://playwrightsolutions.com>",
  "main": "index.js",
  "scripts": {
    "ui": "npx playwright test --ui"
  },
  ...
}
```

Quando npm run ui for executado, você notará uma nova janela que aparece e, após alguns segundos, todos os testes em sua suíte também devem aparecer na coluna à esquerda. É aqui que iremos executar ou depurar nossos testes.

Para executar um teste no modo UI, selecione um arquivo, um grupo de testes test.describe() ou um teste individual test(). Você pode executar o teste ou grupo de testes usando o botão de play verde. Uma vez executado, você poderá ver o histórico de cada teste no painel de Actions, ver o código do teste em source, os logs do console em console, as requisições de rede http em network e os logs de teste em logs. Para depuração de APIs, normalmente você vai querer encontrar requisições de rede específicas que foram feitas e examinar as diferentes requisições e respostas, incluindo headers e body. Tudo isso é visualizável em cada requisição http feita a partir do Playwright Test que você executa. Um exemplo pode ser encontrado abaixo.

Embora não estejamos utilizando todos os recursos do modo UI (já que não estamos abrindo um navegador), há um grande valor em poder executar testes com todos os detalhes das requisições de rede http ao nosso alcance. Antes de a funcionalidade de UI existir, eu costumava executar os testes com traces sempre ativados, para poder visualizar o relatório de testes e as requisições http por meio do visualizador de arquivos de trace.

Usando arquivos de rastreamento do Playwright Test via Show-Report
Este era meu método preferido para depurar testes antes de o modo UI ser lançado. Este método requer que você atualize o seu arquivo playwright.config.ts para definir trace: "on". Isso fará com que os arquivos de rastreamento sejam gravados para cada teste e estejam disponíveis ao visualizar o Relatório de Testes do Playwright. Um exemplo pode ser encontrado abaixo, isso também pode ser configurado ao executar a partir da linha de comando, sobrescrevendo o que estiver no arquivo playwright.config.ts, executando npx playwright test --trace on, conforme a documentação para linha de comando.

//playwright.config.ts

```typescript
export default defineConfig({
  use: {
    baseURL: process.env.URL,
    ignoreHTTPSErrors: true,
    trace: "on",
  },
  retries: 0,
  reporter: [["list"], ["html"]],
});
```

Uma vez que os testes são executados e concluídos, é tão simples quanto rodar npx playwright show-report, o que abrirá o Relatório de Testes do Playwright, e como configuramos o trace para "on", teremos todas as requisições de rede no relatório! Abaixo está um exemplo do Relatório de Teste em ação.

Console.log / Console.table
No exemplo abaixo, estou executando o comando npx playwright test tests/booking/booking.get.spec.ts:48 e adicionando os comandos console.log().
Um método comum para depurar seu código é adicionar declarações console.log dentro do código. Dependendo da estrutura dos seus dados, você pode obter resultados mistos apenas usando console.log(body); se houver objetos adicionais, como bookingdates listados abaixo, você notará que eles são exibidos como [Object] na saída do console.log.

Quando você quer ver todos os dados, pode usar o método JSON.stringify() para transformar o JSON em uma string. Isso é útil se você quiser ver todos os dados, mas não é muito prático dentro dos seus testes, pois você não terá mais um objeto JavaScript que possa usar a notação de ponto para extrair informações.

Outra dica útil que aprendi com Joel Black é usar console.table(); você pode passar um array de objetos e obter um formato realmente agradável. Note que estou passando body.bookings. Visualizar dados dessa maneira pode tornar mais fácil ver as diferenças no que é retornado, facilitando as comparações.

Executando apenas testes alterados em uma pull request
Recentemente, abordei como faço isso em um post recente. Fez sentido o suficiente para ser um post independente. Implementar isso pode permitir que você obtenha feedback rápido dos seus testes executando no CI. O que é realmente interessante é que, se você habilitar a captura de arquivos de trace em falhas e usar o artigo abaixo, você terá acesso aos artefatos de teste após a execução do teste.
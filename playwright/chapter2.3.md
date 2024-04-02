# Escrevendo seu arquivo de teste

Vamos dar uma olhada em outra parte importante de um projeto de teste - o arquivo de teste.

Vamos avançar e abrir example.spec.ts.

Primeiro, vamos ver a importação.

```
import { test, expect } from '@playwright/test';
```

Isso permitirá que usemos os comandos do Playwright para implementar um teste.

Primeiro, vemos o test, que é o objeto do caso de teste, e o expect, que usaremos para asserções.

Ambos vêm de @playwright/test.

Aqui, vemos a primeira parte de um teste - o nome do teste e a página que vem do Playwright.

```
test('has title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  // Expect a title "to contain" a substring.
  await expect(page).toHaveTitle(/Playwright/);
});
```

Com esta página objeto, podemos acessar funções e executar comandos em cada página.

Conceitualmente, um teste é composto por uma ação e uma afirmação, e isso é exatamente o que vemos aqui.

Primeiro, temos o comando page.goto() e a URL.

Isso fará com que o teste abra o navegador e vá para esta URL, "https://playwright.dev".

Em seguida, vemos essa afirmação através do comando expect.

Ela vai verificar se a página que acabou de abrir tem o título "Playwright".

Isso é uma expressão regular e, claro, temos muitos outros comandos que você pode usar para fazer essa afirmação.

Vamos dar uma olhada na próxima teste.

```
test('get started link', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  // Click the get started link.
  await page.getByRole('link', { name: 'Get started' }).click();

  // Expects the URL to contain intro.
  await expect(page).toHaveURL(/.*intro/);
});
```

Nós vemos um comando familiar que já vimos no teste anterior.

Isso fará exatamente o mesmo, que é abrir a página e ir para "https://playwright.dev".

A seguir, vamos ver que ele realiza um clique em um elemento com a função link e o nome "Get started."

Aqui, podemos ver que ele está realizando um clique() - novamente - uma ação.

Em seguida, conforme já aprendemos, temos a afirmação, que vamos ver através do comando expect para ter /.*intro/.

Significando que, assim que abrir esta página e clicar neste elemento, precisamos verificar se o "intro" está presente na URL.

Para exercitar o que aprendemos, vamos dar uma olhada neste caso de teste. Dentro de example.spec.ts, você verá uma lista de sete etapas.

Se abrir o navegador, verá o que ele faz.

Primeira etapa é abrir a página - "https://playwright.dev".

Clicar em "Get started", passar o mouse sobre a lista suspensa de idiomas, que é esta mesmo, e clicar em "Java".

Verificar se a URL mudou conforme esperado - "https://playwright.dev/java/docs/intro".

Verificar se o texto "Installing Playwright" não está sendo exibido.

Conforme podemos ver aqui na versão Node.js, temos o texto aqui e queremos garantir que não esteja sendo exibido na página Java.

Etapa 7 é verificar se o texto abaixo "Playwright is distributed…." está sendo exibido.

Na página Node, ele não é exibido, então queremos garantir que esta informação está sendo exibida.

Isso é apenas um teste simples para exercitar alguns comandos.

Voltando ao VS Code, vamos começar a nomear nosso teste.

Iniciamos com

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');
});
```

A primeira coisa é abrir a página.

Já sabemos que temos o page.goto e podemos passar a URL - "https://playwright.dev".

Uma pequena correção aqui é que o page.goto() tem o await no início.

Como esta é uma aplicação Node, queremos executá-la de forma sincrona, então precisamos do await para garantir que a próxima etapa só será executada quando o page.goto() terminar.

Considere isso a partir de agora e, em nosso repositório do GitHub, o código está atualizado.

A próxima coisa é clicar em "Get started" - já sabemos que precisamos do await para fazê-lo de forma sincrona após a primeira etapa ser concluída. Vamos pegar o elemento por role 'link' e name 'Get started' e clicar nele.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();
});
```

Próxima, faremos um hover sobre a dropdown de linguagens.

Se abrirmos o Chrome e inspecionarmos esta dropdown, vamos ver que há um botão aqui, então podemos usar a role 'button' em hover.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.3-img1.png)

Lembre-se de que precisamos clicar em "Node.js" porque é assim que ele vem por padrão.

Vamos adicionar isso agora.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();

  await page.getByRole('button', {name: 'Node.js'}).hover();
});
```

Nesta vez, será a role 'button' e o nome é "Node.js".

Como mencionamos, será uma ação de hover.

Assim que fizermos o hover, queremos clicar no link Java.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();

  await page.getByRole('button', {name: 'Node.js'}).hover();
  await page.getByText('Java', { exact:true }).click();
});
```

Vamos obter o elemento obtendo o texto exato 'Java' aqui e, por curso, faremos o clique aqui.

Após clicar em 'Java', verificaremos a URL.

Agora, vem a etapa de afirmação.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();

  await page.getByRole('button', {name: 'Node.js'}).hover();
  await page.getByText('Java', { exact:true }).click();

  await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
});
```

Aqui, poderíamos usar uma expressão regular, como vimos anteriormente - em vez de /.* intro/ nós usariamos java ou nós usaríamos a URL exata e ela faria a mesma coisa.

Depende do seu projeto, etc. Vamos fazer a URL exata apenas para ver um exemplo diferente.

Em seguida, vamos verificar o texto.

Para verificar o texto, precisamos fazer um getByText.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();

  await page.getByRole('button', {name: 'Node.js'}).hover();
  await page.getByText('Java', { exact:true }).click();

  await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
  await expect(page.getByText('Installing Playwright', {exact:true})).not.toBeVisible();
});
```

Vamos obter o texto "Installing Playwright" e também queremos obter o texto exato.

Vamos fazer .not.toBeVisible() porque queremos que ele não esteja visível.

Finalmente, vamos verificar se o texto abaixo está sendo exibido.

Como é um texto grande, vamos criar uma variável para armazenar isso, de modo que ela não crie um comando enorme.

```
test('check Java page', async ({ page }) => {
  await page.goto('https://playwright.dev');

  await page.getByRole('link', {name: 'Get started'}).click();

  await page.getByRole('button', {name: 'Node.js'}).hover();
  await page.getByText('Java', { exact:true }).click();

  await expect(page).toHaveURL('https://playwright.dev/java/docs/intro');
  await expect(page.getByText('Installing Playwright', {exact:true})).not.toBeVisible();

  const javaDescription = `Playwright is distributed as a set of Maven modules. The easiest way to use it is to add one dependency to your project's pom.xml as described below. If you're not familiar with Maven please refer to its documentation.`;
  await expect(page.getByText(javaDescription)).toBeVisible();

});
```

Então usaremos `const javaDescription`, ou qualquer nome que você preferir, e usaremos crases para agrupar caracteres especiais.

Então queremos verificar se o texto está sendo exibido.

Para executar o teste, vamos adicionar `.only` no início do teste.

Isso é usado quando você quer executar apenas um ou alguns testes, você pode adicionar isso a cada teste.

Salve o documento e abra o Terminal.

Para este, podemos usar qualquer comando no package.json file, mas neste caso, gostaria de digitar `npx playwright test tests/`, que vai executar apenas o teste dentro da pasta test, e adicionar --project=chromium.

Isso vai executar esse teste apenas no Chromium e passou.

Aqui está, executamos nosso primeiro teste.

Se você quiser ver o relatório, como de costume, pode copiar o comando e ele abrirá diretamente.

Você pode ver que todos os passos foram executados com sucesso.

Sim, chegamos ao fim do capítulo 2. Parabéns de novo.

Dê uma olhada novamente no quiz abaixo e não se esqueça de verificar os exercícios extras.

Espero que você tenha gostado e que vou te ver no próximo capítulo. Divirta-se testando.


## Resources
- [Introduction to Playwright - GitHub repository - Branch 2](https://github.com/raptatinha/tau-introduction-to-playwright/tree/chapter-2)
- [Chapter 2 - Extra Exercises](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/exercises/chapter2.MD)
- [Chapter 2 - Extra Resources](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/extra-resources/chapter2.MD)
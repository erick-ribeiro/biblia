Olá, bem-vindo ao Capítulo 2.

Neste capítulo, vamos ver o arquivo `playwright.config.ts`, que tem muitas opções de configuração para configurar como os testes serão executados em seu projeto de teste.

Também vamos ver como os comandos de `runner de teste` são criados e como usar os scripts do `package.json` para facilitar sua digitação.

Além disso, vamos aprender a escrever seu primeiro teste. Espero que você goste.

# Compreendendo a configuração do Playwright

O primeiro arquivo que vamos ver é o `playwright.config.ts`. Este arquivo tem todas as configurações para os testes que vamos implementar em nosso projeto.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.1-img1.png)

A primeira linha possui a importação inicial.

Ela vem do `@playwright/test` e possui o `defineConfig`, que é o objeto com todas as configurações e os dispositivos em que os testes são executados - os navegadores ou os dispositivos móveis, caso sejamos necessários.

Segundo, adicionei aqui uma importação para `baseEnvUrl`. Vamos dar uma olhada nela em breve.

Remova o comentário para `require('dotenv').config()`. Isso permitirá que tenhamos as variáveis de ambiente e definamos as informações lá.

Também importante lembrar - você pode passar o mouse sobre os comandos aqui, então você tem todas as docs para cada comando.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.1-img2.png)

Agora vamos começar este primeiro bloco.

```
export default defineConfig({
  // testDir: './tests',
```

O primeiro que vemos é o diretório de testes.

Por enquanto, vamos comentar essa linha porque queremos rodar todos os testes.

Em seguida, temos a definição da paralelismo.

```
  /* Run tests in files in parallel */
  fullyParallel: true,
```

Aqui temos true, como valor padrão - o Playwright permite que todos os testes dentro de um arquivo sejam executados em paralelo, definido este comando como true podemos acelerar os testes ainda mais.

`forbidOnly` falhará sua pipeline caso você esqueça colocar .only em cada um dos seus testes - iremos entender melhor isso no futuro.

```
  /* Fail the build on CI if you accidentally left test.only in the source code. */
  forbidOnly: !!process.env.CI,
```

Aqui temos a definição de "retries".

```
  /* Retry on CI only */
  // retries: process.env.CI ? 2 : 0,
  retries: 2,
```

Playwright também possui um recurso interno para tentar novamente o teste automaticamente.

Ele vem com a linha de comentário como a opção padrão, o que significa que, se a variável de ambiente CI for verdadeira, o Playwright irá tentar novamente no CI, mas não irá tentar localmente ou em nenhum outro ambiente.

Para nosso teste agora queremos ver as "retries", então vamos definir como 2.

Isso é para definir o número de "workers", que basicamente são processos em paralelo.

```
/* Opt out of parallel tests on CI. */
  workers: process.env.CI ? 1 : undefined,
```

Por padrão, ele tem um em ambiente de CI, o que significa que não irá rodar em paralelo, mas localmente queremos que o Playwright gere automaticamente, então vamos deixar como undefined.

Em seguida, temos o relator, e por padrão temos html.

```
  /* Reporter to use. See https://playwright.dev/docs/test-reporters */
  reporter: 'html',
```

Playwright também possui um relatório completo para nós e ele será aberto caso o teste falhe.

Se você quiser mudar isso, você pode ter essa opção aqui como "sempre":

```
  // reporter: [['html', { open: 'always' }]], //always, never and on-failure (default).
```

Se você quiser mudar a pasta de saída, você também pode fazer isso usando este comentário:

```
  // reporter: [['html', { outputFolder: 'my-report' }]], // report is written into the playwright-report folder in the current working directory. override it using the PLAYWRIGHT_HTML_REPORT
```

Lembre-se de que, se você passar o mouse sobre, poderá ver a lista de todos os relatórios disponíveis, e aqui você também pode escolher outro, como 'dot' ou 'list'.

```
  // reporter: 'dot',
  // reporter: 'list',
```

Vejamos um exemplo da execução em lista.


![Alt text](https://testautomationu.applitools.com/course86/chapter2.1-img3.png)

E outro exemplo de execução em dot.

Aqui também é possível observar o número de workers.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.1-img3b.png)

```
  /* Opt out of parallel tests on CI. */
  workers: process.env.CI ? 1 : undefined,
```

Você também pode ter uma lista combinada de relatores.

Você pode adicionar quantos quiser aqui e, claro, pode mudar o arquivo de saída e também pode ter relatórios personalizados - basta dar uma olhada neste link e ele vai te guiar.

Em seguida, vamos ir para o bloco "use".

```
  use: {
    /* Base URL to use in actions like `await page.goto('/')`. */
    // baseURL: 'http://127.0.0.1:3000',

    /* Collect trace when retrying the failed test. See https://playwright.dev/docs/trace-viewer */
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    // headless: false,
    // ignoreHTTPSErrors: true,
    // viewport: { width: 1280, height: 720 },
    // video: 'on-first-retry',
  },
```

Aqui, você pode encontrar o baseURL - em caso de sua aplicação sempre usar a mesma URL, você pode definir aqui.

Você também pode em seu teste, ter um simples comando de linha como esse um exemplo - await page.goto('/') - em vez de ter duplicado em cada de seus testes.

O trace ajudará a entender os testes falhados e neste caso ele traçará quando a primeira nova tentativa falhar. Você também pode mudar esse valor para alguns outros, conforme você pode ver aqui.

o screenshot fará uma captura de tela apenas em caso de falha. Você também pode mudar esse valor se quiser - aqui estão os valores e você também pode ter algumas outras opções.

Por padrão, o Playwright roda em modo headless - você pode mudar para falso se quiser.

Em caso de sua aplicação não ter um certificado válido, por exemplo, em ambientes internos - você pode usar ignoreHTTPSErrors e para qualquer outro tipo de necessidade.

Você pode definir o viewport das suas testes.

Você também pode gravar um vídeo se quiser, e novamente, ele usa os mesmos parâmetros de trace e screenshot.

Você pode mudar o tempo de espera para qualquer comando - por exemplo, click - e também para o expect. Eles são dois parâmetros diferentes.

```
    // timeout: 30000, //https://playwright.dev/docs/test-timeouts
    // expect: {
      /**
       * Maximum time expect() should wait for the condition to be met.
       * For example in `await expect(locator).toHaveText();`
       */
      // timeout: 10000,
    // },
```

Uma correção rápida é que o tempo de espera e o tempo de espera do expect estão fora do bloco use.

Eles não estão dentro, conforme eu mostrei. Basta mover para baixo.

O repositório no GitHub já tem o código atualizado e a partir de agora, se você precisar, basta considerar como sendo fora do bloco use.

Você também pode mudar a pasta de saída dos resultados dos testes e, em seguida, vamos ver os projetos.

```
  /* Configure projects for major browsers */
  projects: [
    {
      name: 'chromium',
      use: { 
        ...devices['Desktop Chrome'],
        // viewport: { width: 1280, height: 720 },
      },
    },

    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },

    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
```


Os projetos são um grupo de configurações que suas testes irão passar por padrão.

Por padrão, ele vem com os navegadores, que são Chromium, Firefox e Webkit para Safari, mas normalmente uma melhor utilização é através de ambientes.

```
    // Example only
    {
      name: 'local',
      use: { 
        baseURL: baseEnvUrl.local.home,
      },
    },
```

Neste exemplo, estamos utilizando um arquivo externo, conforme podemos ver no início, e se abrirmos o arquivo, podemos ver que temos URLs diferentes para diferentes ambientes aqui.

```
export default {
  ci: {
    prefix: 'https://dev-myapp-',
    suffix: '.mydomain.com',
  },
  local: {
    api: 'https://local-myapp.mydomain.com/api',
    home: 'https://local-myapp.mydomain.com',
  },
  production: {
    api: 'https://myapp.mydomain.com/api',
    home: 'https://myapp.mydomain.com',
  },
  staging: {
    api: 'https://staging-myapp.mydomain.com/api',
    home: 'https://staging-myapp.mydomain.com',
  },
};
```

Isso é opcional, mas eu só estou mostrando que você poderia fazer isso.

E novamente, para o CI, você também pode construir uma URL com base em variáveis de ambiente.

```
  // Example only
    {
      name: 'ci',
      use: { 
         baseURL: process.env.CI
          ? baseEnvUrl.ci.prefix + process.env.GITHUB_REF_NAME + baseEnvUrl.ci.suffix //https://dev-myapp-chapter-2.mydomain.com
          : baseEnvUrl.staging.home,
      },
```

O GitHub e o GitLab possuem um grupo de variáveis que podem ser utilizadas durante a execução do teste, portanto é importante que você se familiarize com esses conceitos.

```
     /**
       * GitHub variables: https://docs.github.com/en/actions/learn-github-actions/variables
       * GitLab variables: https://docs.gitlab.com/ee/ci/variables/predefined_variables.html#predefined-variables-reference
       */
    },
```

Outra opção que você pode usar é ter dispositivos móveis e aqui estão alguns exemplos e navegadores de marca em vez do Chromium e do Webkit, você pode usar os navegadores reais, mas eles simplesmente usarão um canal diferente, como você pode ver aqui

```
    /* Test against mobile viewports. */
    // {
    //   name: 'Mobile Chrome',
    //   use: { ...devices['Pixel 5'] },
    // },
    // {
    //   name: 'Mobile Safari',
    //   use: { ...devices['iPhone 12'] },
    // },

    /* Test against branded browsers. */
    // {
    //   name: 'Microsoft Edge',
    //   use: { ...devices['Desktop Edge'], channel: 'msedge' },
    // },
    // {
    //   name: 'Google Chrome',
    //   use: { ..devices['Desktop Chrome'], channel: 'chrome' },
    // },
```

Você também pode executar o servidor local antes de iniciar os testes alterando o comando webServer aqui.

```
  /* Run your local dev server before starting the tests */
  // webServer: {
  //   command: 'npm run start',
  //   url: 'http://127.0.0.1:3000',
  //   reuseExistingServer: !process.env.CI,
  // },
```

Este é o fim da primeira seção do Capítulo 2. Vamos continuar com algumas mais vídeos, mas você pode verificar o quiz agora e pode deixar os exercícios extras para o final do todo capítulo. Eu vou te ver na próxima vídeo.

# Resources
- [Introduction to Playwright - GitHub repository - Branch 2](https://github.com/raptatinha/tau-introduction-to-playwright/tree/chapter-2)
- [Chapter 2 - Extra Exercises](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/exercises/chapter2.MD)
- [Chapter 2 - Extra Resources](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/extra-resources/chapter2.MD)

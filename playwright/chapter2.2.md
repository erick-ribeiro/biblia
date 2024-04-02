
O primeiro passo que vamos fazer é abrir o arquivo `demo-todo-app.spec.ts`, que está localizado em `tests-examples` e fazer algumas alterações.

Adicionei a tag `@smoke` a algumas testes - por favor, certifique-se de adicionar no primeiro teste nesse arquivo.

Também, altere a linha 24 - originalmente o arquivo tem o valor 0 aqui e vamos mudar para 1.

```
    // Make sure the list only has one todo item.
    await expect(page.getByTestId('todo-title')).toHaveText([
      TODO_ITEMS[1] // intentionally changed here to fail the test
    ]);
```

Esta mudança é intencional e fará com que o teste falhe.

Em seguida, adicione `@smoke` a alguns outros testes.

Como você pode ver aqui, a tag pode estar em qualquer teste de sua preferência, mas certifique-se de adicionar pelo menos cinco deles, por favor.

# Compreendendo o arquivo package.json e os scripts do runner de testes
Em seguida, vamos entender o arquivo package.json.

Nesse arquivo, você pode encontrar todas as versões das dependências que você está usando para seu projeto, e também pode criar scripts para tornar sua execução mais rápida.

Aqui você pode ver que o primeiro script que temos é o teste de ponta a ponta - test:e2e.

```
    "test:e2e": "npx playwright test tests/",
```

Em vez de digitar "npx playwright test tests/" no nosso Terminal, podemos simplesmente digitar "npm run test:e2e" e pressionar Enter, o que fará a mesma coisa que "npx playwright test tests/".

É importante mencionar aqui que, se você tiver pastas que começarem com as mesmas palavras, é importante ter a barra ao final para garantir que está se direcionando apenas para essa pasta específica.

Por exemplo, no segundo script temos a pasta "tests" sem a barra no final, portanto, ela irá rodar para todas as pastas que começarem com a palavra "tests".

Vamos dar uma olhada nesse comando.

```
    "test:e2e:all": "npx playwright test tests --project=all-browsers-and-tests",
```

Também possui o "npx playwright test" e a pasta, mas também possui o "--project" aqui.

Isso se liga ao arquivo playwright.config.ts que definimos no final, então vai pegar todas as configurações definidas aqui.

Uma pequena esclarecimento aqui - para rodar todos os navegadores, precisamos do nome do navegador dentro do projeto, e para cada navegador iremos ter uma instância do projeto com o mesmo nome.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.2-img1.png)

Dessa forma, sempre que chamarmos o projeto "all-browsers-and-tests" no arquivo package.json ou no comando playwright, todas as três instâncias serão chamadas aqui e todos os três navegadores serão executados.

Se você quiser mais navegadores e dispositivos, basta adicionar uma nova instância dele.

Podemos ver que estamos rodando contra todos os navegadores, então é isso que faremos.

Podemos ir em frente e digitar "npm run test:e2e:all" e rodarão 28 testes.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.2-img2.png)

Nesse caso, novamente, vamos rodar para todas as pastas que possuem a palavra "tests".

Aqui você pode ver que há muitos mais testes do que estes aqui, e porque definimos a retry como 2, como vimos anteriormente, temos um teste que falhou e foi retentado duas vezes.

No final, como definimos, para abrir o relatório HTML em caso de falha, você pode ver aqui que ele foi aberto.

Se você quiser dar uma olhada, aqui está como fica.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.2-img3.png)


Vamos pressionar Ctrl+C para encerrar o Terminal - dessa forma, o prompt estará pronto para nosso próximo comando de execução.

A seguir temos o test:e2e:ci.

```
    "test:e2e:ci": "CI=1 npx playwright test --project=ci --shard=$CI_NODE_INDEX/$CI_NODE_TOTAL",
```

Aqui você pode ver que defini a variável de ambiente como 1.

No nosso arquivo playwright.config.ts, lembre-se que tivemos essa informação aqui.

```
  /* Opt out of parallel tests on CI. */
  workers: process.env.CI ? 1 : undefined,
```

Portanto, caso desejemos executar nosso teste no CI (Continuous Integration), isso é uma forma de configurar.

Você também pode ter um arquivo .env onde você pode definir sua variável de ambiente CI como 1.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.2-img4.png)


Você pode ter este arquivo no seu servidor, então quando a execução do teste estiver ocorrendo, ele terá acesso a ele.

Em seguida, vamos ver os mesmos comandos novamente - npx playwright test - nesse caso, o projeto será ci, então, se você for para a configuração do playwright e encontrar o projeto ci, você vai ver que ele vai usar o baseURL com base na informação combinada, e você também pode passar algumas outras variáveis de ambiente aqui que virão, por exemplo, da máquina, do GitHub ou de qualquer outra fonte.

Nesse caso, como não temos a configuração do CI, não vamos executar este comando e vamos direto para o próximo.

```
    "test:e2e:dev": "npx playwright test tests-examples/ --project=chromium --headed --retries=0 --reporter=line",
```

Por outro lado, você pode definir esses nomes como você quiser.

Neste caso, temos "dev" aqui. Você pode fazer o modo de desenvolvimento ou o que quiser - é totalmente up a você.

Neste modo, temos o mesmo que já vimos. A opção --project aqui vai ser chromium, significando que só vai rodar no Chromium.

Estou também definindo como --headed, então vamos ver o navegador aberto.

Definimos o --retries como 0, então, em caso de falha, não vai tentar novamente - não vai retry nada, e definimos o reporter como line em vez de HTML.

Você pode ver isso passando esses parâmetros na linha de comando - você vai substituir as informações de configuração do playwright.

Vamos dar uma tentativa ao executar npm run test:e2e:dev.

Você pode ver o navegador aberto, e porque está rodando todos os testes em paralelo, é super rápido - você não vê nada.

Pois não definimos o reporter como HTML, você pode ver aqui apenas a lista dos testes e você pode ver que um dos testes falhou.

Próximo, vamos dar uma olhada em smoke.

```
"test:e2e:smoke": "npx playwright test tests-examples/ --grep @smoke --project=chromium",
```

Para isso, estamos utilizando tags.

Se você abrir um teste, por exemplo, veja este arquivo de teste a seguir - é possível ver que ele possui a tag @smoke.

![Alt text](https://testautomationu.applitools.com/course86/chapter2.2-img5.png)

Isso é como você define tags em seus testes.

Você pode fazer isso em qualquer teste que desejar, simplesmente adicionando a palavra e aqui em package.json, você pode usar --grep @smoke e ele vai rodar apenas os testes que têm essa tag nela.

Podemos dar uma tentativa com npm run test:e2e:smoke.

Portanto, apenas 12 testes e você pode ver aqui que tem a tag @smoke.

Para isso, não passamos nada sobre as novas tentativas, portanto, ele vai usar o padrão da configuração do playwright, que é dois, e assim que ele terminar, ele vai abrir o relatório HTML com apenas os testes com a tag smoke, e você pode ver aqui que um deles falhou.

Por fim, vamos ver os testes não relacionados a smoke.

```
    "test:e2e:non-smoke": "npx playwright test tests-examples/ --grep-invert @smoke --project=firefox"
```

A maneira de fazer isso é usando --grep-invert, portanto, é bastante semelhante à que fizemos recentemente.

Vamos entrar aqui e executar npm run test:e2e:non-smoke e ele vai rodar todos os testes que não possuem a tag @smoke.

Não mencionei isso antes, mas todos esses comandos são parte do Playwright Test Runner e você pode encontrar mais informações sobre eles em "Running tests" nos docs.

Você pode navegar por essas seções para ir diretamente para onde você deseja e você pode ver algumas opções aqui.

Por exemplo, para executar o teste com o título, você pode passar -g e o título do teste e então ele fará isso para você.

Você pode combinar esses parâmetros com --debug - que iniciará o modo de depuração, e também --ui para o modo de interface.

Vamos ver mais sobre isso em um pouco.

No resumo, os scripts dentro do package.json são totalmente opcionais.

Você pode digitar qualquer um desses comandos no terminal e eles fazerão exatamente a mesma coisa.

É apenas uma maneira de criar scripts e fazer sua digitação mais fácil.

Isso é o fim do segundo vídeo do Capítulo 2.

Ainda temos mais um vídeo para ir na fase.

Dê uma olhada na quiz abaixo e você pode deixar os exercícios extras para o final do todo o capítulo. Eu vou ver no próximo vídeo.

## Resources
- [Introduction to Playwright - GitHub repository - Branch 2](https://github.com/raptatinha/tau-introduction-to-playwright/tree/chapter-2)
- [Chapter 2 - Extra Exercises](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/exercises/chapter2.MD)
- [Chapter 2 - Extra Resources](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/extra-resources/chapter2.MD)
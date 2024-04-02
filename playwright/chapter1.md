Oi, bem-vindo ao capítulo 1.

Neste capítulo, vamos aprender como manter-se atualizado sobre as novidades do Playwright, como usar a documentação do Playwright através dos menus, como usar a documentação do Playwright através da pesquisa, como instalar o Playwright e como executar os testes. Espero que você goste.

# Explorar e aprender sobre o Playwright
Ao começar a aprender, um recurso muito importante é a documentação oficial do Playwright. Nela, você encontra tudo para se manter auto-suficiente em seus estudos e manter-se atualizado com a evolução do framework.

Certifique-se de selecionar a linguagem correta para a documentação.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img1.png)

Nesta treinamento, estamos usando Node.js, que já é a linguagem padrão.

Uma destaque é o canal do Discord do Playwright, onde você pode se conectar à comunidade e obter ajuda se necessário.

Rolando até o rodapé, você encontrará alguns links adicionais que podem ser úteis.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img2.png)

Outra destaque é o canal do YouTube, onde você pode acessar um conteúdo de alta qualidade dos especialistas, e o GitHub, onde você pode dar um "Star" no projeto e "Watch" para receber notificações por email sobre as novas releases ou qualquer outra coisa relacionada a isso.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img3.png)

Aqui na parte "Releases", você verá as notas de lançamento com novas funcionalidades, correções de bugs e também alterações que podem ser "breaking changes", então é importante manter de olho nisto.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img4.png)

Voltando ao site, temos acesso aos docs e aqui temos algumas coisas importantes para ver.

O menu "Getting Started" vai lhe dar as primeiras etapas básicas para conhecer o framework e entender como escrever e manter seus testes; a maioria deles vamos ver aqui neste treinamento.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img5.png)

Outra opção interessante do menu é o "Playwright Test", onde você pode encontrar algumas funcionalidades mais avançadas e como criar um framework mais completo.

Outra opção importante do menu é "Guides".

Aqui, você encontrará orientações para muitos dos recursos que o Playwright oferece a você.

Eu convido você a dar uma olhada na opção "Best Practices".

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img6.png)

No lado direito, você pode navegar facilmente pelo tópico principal e a decisão de seguir as melhores práticas ou não é extremamente importante para otimizar a execução dos testes e aumentar a manutenibilidade do código.

Portanto, é fundamental que você decida se segue as melhores práticas ou não, mas é importante entender porque elas existem.

Além disso, você também pode encontrar algumas opções para migrar seu código de um framework para outro, bem como como integrar seu framework com outras terceiras partes, como o Docker, o Selenium Grid ou qualquer integração contínua, sob a seção "Migration" e "Integrations", respectivamente.

Outra funcionalidade incrível que temos aqui na documentação é a pesquisa.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img7.png)

Por exemplo, se eu quiser saber como funciona o "click" - você tem Guides, Classes e Getting started - então você tem informações relacionadas à função do "click".

Vamos ver o que isso nos dá.

Aqui temos um clique de mouse - ele realiza um clique humano simples.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img8.png)

Aqui estão algumas opções de como usar com `getByRole('button').click()`:

Você pode usar `getByText` para fazer um clique duplo.

Você também pode usar um clique do botão direito com modificadores como "Shift" e etc, e você pode entender abaixo do chapéu como funciona dessa função e, claro, alguns exemplos.

Mas vamos supor que eu esteja interessado em algo mais sobre o clique.

Posso voltar aqui novamente, por exemplo, e, por exemplo, vou para a classe.

Aqui você vê como funciona, como usar e os argumentos que podem ser passados..

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img9.png)

Você pode ver, por exemplo, se você deseja adicionar um atraso ao seu clique, por exemplo, clique e mantenha o botão pressionado por alguns segundos, aqui está como usar.

A pesquisa é uma ferramenta excelente, então certifique-se de usar-la para qualquer pergunta relacionada a suas práticas de codificação.

## Configurando e Instalando Playwright
Agora que já aprendeu a usar a documentação, nosso próximo passo é instalar o Playwright. Se está pronto, abra o Terminal e divirta-se!

Depois de ter o Terminal aberto, vá para a pasta desejada e digite:

```
npm init playwright@latest
```

Isso irá instalar a versão mais recente do Playwright.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img10.png)

A primeira pergunta é qual linguagem queremos usar.

Nós vamos pressionar "Enter" para usar o TypeScript, que é a linguagem padrão.

A segunda pergunta é onde você coloca suas testes de ponta a ponta.

Nós vamos pressionar "Enter" para manter o padrão, que é a pasta "tests" e pressionar "Enter" novamente.

Pergunta se você quer adicionar um fluxo de trabalho do GitHub. Para este treinamento, não estamos indo pelo fluxo de trabalho, então vamos pressionar "Enter" novamente para "false".

A última pergunta é se você quer instalar os navegadores. Vamos pressionar "Enter" novamente para "sim".

Ele irá baixar e instalar os navegadores e Playwright, e no final você verá uma mensagem de sucesso.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img11.png)

Próximo, você verá alguns comandos úteis. Por exemplo, se deseja executar o teste, você pode digitar `npx playwright test`.

Isso irá executar o teste que você adicionou ao seu projeto.

Ele cria por padrão alguns arquivos de exemplo que podemos dar uma olhada depois, e também vincula a documentação novamente.

Agora que temos o Playwright instalado, vamos abrir o IDE.

Vamos digitar `code .` no Terminal e o VS Code será aberto para nós.

Aqui podemos ver que algumas pastas foram criadas. Temos um exemplo de arquivo aqui, outro exemplo aqui e algumas outras pastas.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img12.png)

Agora vamos ver mais sobre eles em um pouco, mas vamos para frente e vá para a aba "Extensions" e digite "playwright".

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img13.png)

Existem algumas opções disponíveis - vamos certificar-nos de selecionar a que pertence da Microsoft.

Aqui você pode ver os detalhes sobre a extensão e vamos prosseguir e instalá-la.

Após a instalação, vamos ver o ícone à esquerda.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img14.png)

Podemos chegar aqui e você pode observar algumas opções. Você pode executar todos os testes através deste menu.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img15.png)

E também existem outras opções - você pode filtrar a lista de testes disponíveis para este projeto.

Uma coisa importante para se notar é que todos os testes que serão exibidos aqui serão vinculados à pasta de testes listada aqui no arquivo de configuração do Playwright.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img16.png)

Certifique-se, se você deseja ver seus testes lá, que essa pasta esteja atualizada ou certifique-se de que seus testes estejam nessa pasta.

Por hora, vamos dar um clique nesse botão e você verá que o teste será executado.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img17.png)

Outra maneira de executar o teste é através do Terminal, que você pode habilitar clicando neste ícone ou usando esses comandos.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img17.png)

Aqui você pode digitar:

```
npx playwright test
```

O teste será executado. Nesse caso, estamos executando por meio do Terminal.

Você também pode usar seu terminal normal, conforme mostrado aqui, e você verá os resultados aqui.

Se você quiser abrir o relatório, basta digitar este comando e um navegador será aberto com os resultados.

![Alt text](https://testautomationu.applitools.com/course86/chapter1-img18.png)

Novamente, iremos ver mais detalhes sobre eles, mas é apenas para dar uma ideia de o que o Playwright pode fazer agora.

Parabéns. Terminamos o capítulo um.

Boa sorte com o quiz abaixo e não se esqueça de dar uma olhada nos exercícios extras também.

Irei te ver na próxima capítulo. Feliz testing.


# Resources
- [Playwright website](https://playwright.dev/)
- [Introduction to Playwright - GitHub repository - Branch 1](https://github.com/raptatinha/tau-introduction-to-playwright/tree/chapter-1)
- [Chapter 1 - Extra Exercises](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/exercises/chapter1.MD)
- [Chapter 1 - Extra Resources](https://github.com/raptatinha/tau-introduction-to-playwright/blob/main/extra-resources/chapter1.MD)
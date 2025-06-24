Cucumber é uma ferramenta que suporta [Desenvolvimento Orientado a Comportamento(BDD)](/docs/bdd).
Se você ainda não sabe o que é Desenvolvimento Orientado a Comportamento (BDD), leia a [introdução ao BDD](/docs/bdd)
antes.

# O que é Cucumber?
Agora que você sabe que BDD é sobre descoberta, colaboração e exemplos
(e não teste), vamos dar uma olhada em Cucumber.

Cucumber lê especificações executáveis escritas em texto simples e valida
que o software faz o que essas especificações dizem. As especificações
consiste em vários *exemplos*, ou *cenários*. Por exemplo:

```gherkin
Scenario: Adicionar um produto disponível ao carrinho
    Given que estou na página do produto "Camiseta Preta Tamanho M" disponível em estoque
    When clico em "Adicionar ao Carrinho"
    Then "Camiseta Preta Tamanho M" é adicionado ao carrinho
    And a página mostra "Produto adicionado ao carrinho com sucesso"
```

Cada cenário é uma lista de *steps* para o Cucumber trabalhar. O Cucumber verifica se o software segue a especificação e gera um relatório indicando ✅ sucesso ou ❌ falha para cada cenário.

Para que o Cucumber entenda os cenários, eles devem seguir algumas regras de sintaxe básicas, chamadas de [Gherkin](/docs/gherkin/).

# O que é Gherkin?

Gherkin é um conjunto de regras de gramática que torna o texto estruturado o suficiente para o Cucumber entender. O cenário acima está escrito em Gherkin.

Gherkin atua em várias finalidades:

- Especificação executável clara e não ambigua
- Teste automatizado usando o Cucumber
- Documenta como o sistema *realmente* se comporta

![Single source of Truth](./img/single-source-of-truth-256x256.png)

Os documentos Gherkin são armazenados em arquivos de texto `.feature` e geralmente
são versionados junto com o software no controle de versão.

Veja a referência do [Gherkin](/docs/gherkin) para mais detalhes.

# O que são Step Definitions?

[Step definitions](/docs/cucumber/step-definitions) servem para conectar os steps do Gherkin ao código de programação. Um "Step Defnition" realiza a ação que deve ser executada pela "step". Assim, os "steps defnitions" conectam diretamente a especificação à implementação.


```
┌────────────┐                     ┌──────────────┐                     ┌───────────┐
│   Steps    │                     │     Step     │                     │           │
│ em Gherkin ├──correspondem aos──>│ Definitions  ├───que manipulam o──>│  Sistema  │
│            │                     │              │                     │           │
└────────────┘                     └──────────────┘                     └───────────┘
```

Step definitions podem ser escritas em várias linguagens de programação. Aqui está um exemplo
usando JavaScript:

```javascript
When("{maker} starts a game", function(maker) {
maker.startGameWithWord({ word: "whale" })
})
```

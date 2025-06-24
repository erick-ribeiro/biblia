# Gherkin lint

Utiliza [Gherkin](https://github.com/cucumber/gherkin-javascript) para analisar arquivos de funcionalidades e executa a verificação de regras padrão, além das regras configuradas no arquivo `.gherkin-lintrc`.

## Regras Disponíveis

| Nome                                                | Funcionalidade                                                                                                        |
|-----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `no-unnamed-features`                               | Proíbe nome de Feature vazio                                                                                          |
| [`allowed-tags`](#allowed-tags)                     | Apenas as tags listadas são permitidas                                                                                |
| [`file-name`](#file-name)                           | Restringe os nomes dos arquivos de funcionalidades a um estilo comum                                                  |
| [`no-restricted-patterns`](#no-restricted-patterns) | Lista de padrões para proibir globalmente ou especificamente em features, backgrounds, cenários ou esboços de cenário |
| `no-tags-on-backgrounds` *                          | Proíbe tags em Background                                                                                             |
| `one-feature-per-file` *                            | Proíbe múltiplas definições de Feature no mesmo arquivo                                                               |
| `up-to-one-background-per-file` *                   | Proíbe múltiplas definições de Background no mesmo arquivo                                                            |
| `no-multiline-steps` *                              | Proíbe passos multilinha                                                                                              |
| [`indentation`](#indentation)                       | Permite ao usuário especificar regras de indentação                                                                   |
| [`max-scenarios-per-file`](#max-scenarios-per-file) | Permite ao usuário especificar o número máximo de cenários por arquivo de funcionalidade                              |
| [`name-length`](#name-length)                       | Permite restringir o comprimento dos nomes de Feature/Cenário/Passo                                                   |
| [`new-line-at-eof`](#new-line-at-eof)               | Proíbe/obriga uma nova linha no EOF                                                                                   |
| `no-background-only-scenario`                       | Proíbe background quando há apenas um cenário                                                                         |
| `no-dupe-feature-names`                             | Proíbe nomes de Feature duplicados                                                                                    |
| [`no-dupe-scenario-names`](#no-dupe-scenario-names) | Proíbe nomes de Cenário duplicados                                                                                    |
| `no-duplicate-tags`                                 | Proíbe tags duplicadas na mesma Feature ou Cenário                                                                    |
| `no-empty-background`                               | Proíbe features com backgrounds sem passos                                                                            |
| `no-empty-file`                                     | Proíbe arquivos de funcionalidade vazios                                                                              |
| `no-examples-in-scenarios`                          | Proíbe o uso de "Exemplos" em Cenários, permitido apenas em Esboços de Cenário                                        |
| `no-files-without-scenarios`                        | Proíbe arquivos sem cenários                                                                                          |
| `no-homogenous-tags`                                | Proíbe tags presentes em todos os Cenários de uma Feature, ao invés de na própria Feature                             |
| `no-multiple-empty-lines`                           | Proíbe múltiplas linhas vazias                                                                                        |
| `no-partially-commented-tag-lines`                  | Proíbe linhas de tag parcialmente comentadas                                                                          |
| [`no-restricted-tags`](#no-restricted-tags)         | Proíbe o uso de @tags específicas                                                                                     |
| `no-scenario-outlines-without-examples`             | Proíbe esboços de cenário sem exemplos                                                                                |
| `no-superfluous-tags`                               | Proíbe tags presentes em uma Feature e um Cenário dessa Feature                                                       |
| `no-trailing-spaces`                                | Proíbe espaços em branco finais                                                                                       |
| `no-unnamed-scenarios`                              | Proíbe nome de Cenário vazio                                                                                          |
| `no-unused-variables`                               | Proíbe variáveis não utilizadas em esboços de cenário                                                                 |
| `one-space-between-tags`                            | Tags na mesma linha devem ser separadas por um único espaço                                                           |
| [`required-tags`](#required-tags)                   | Exige tags/padrões de tags em Cenários                                                                                |
| [`scenario-size`](#scenario-size)                   | Permite restring                                                                                                      |

ir o número máximo de passos em um cenário, esboço de cenário e background |
| `use-and`       | Proíbe nomes de passos repetidos exigindo o uso de E em vez disso |
| `-------------` | E---------------------------------------------------------------a |
| `only-one-when` | Exige que haja no máximo um passo When para cada cenário          |

\* Estas regras não podem ser desativadas porque detectam funcionalidades não documentadas do cucumber que causam a falha do parser [gherkin](https://github.com/cucumber/gherkin-javascript).


## Configuração de Regra

As regras podem ser configuradas por meio de um arquivo JSON. O nome do arquivo pode ser definido pela variável de ambiente `GHERKIN_LINT_CONFIG_FILE`. Se esta variável não for definida, o arquivo padrão é `.gherkin-lint.json`.

O arquivo JSON deve conter um objeto com chaves que são nomes de regras e valores que são objetos com chaves que são nomes de configurações e valores que são os valores de configuração.

---
### no-unnamed-features: 
Proíbe a criação de Features sem nome. Isso é útil para garantir que cada Feature tenha um nome descritivo, tornando a leitura e a navegação dos arquivos de Feature mais fácil.

exemple.feature
```
Funcionalidade:

Cenário: Este é um Cenário para no-unnamed-features
Dado Eu tenho uma Feature sem nome
Então Eu deveria ver um erro no-unnamed-features
```

Output
```
./EmptyFeature.feature
0    Missing Feature name                  no-unnamed-features
```

Config
```json
{
"no-unnamed-features": "on"
}
```

---

### allowed-tags

Esta regra permite configurar a lista de tags permitidas em seus arquivos de Feature.

A regra `allowed-tags` deve ser configurada com a lista de tags permitidas, além de padrões opcionais.


```json
{
  "allowed-tags": [
    "on", 
    {
      "tags": [
        "@smoke", 
        "@automated", 
        "@backend", 
        "@frontend"
      ], 
      "patterns": [
        "^URANO-\\d+$"
      ]
    }
  ]
}
```

Qualquer tag não incluída nesta lista não será permitida.

---

### file-name

A regra `file-name` permite que você defina um estilo para o nome dos arquivos de Feature. Isso é útil para padronizar os nomes dos arquivos e ajudar a identificar e encontrar os arquivos de Feature. O estilo recomendado é `kebab-case`, que é uma convenção de nomenclatura em que as palavras são separadas por traços e todas as letras são minúsculas: `meu-arquivo-de-feature.feature`.

```json
{
  "file-name": [
    "on", 
    { 
      "style": "kebab-case"
    }
  ]
}
```

A lista de estilos suportados é:

- `PascalCase` - primeira letra de cada palavra maiúscula (sem espaços) ex: "MyFancyFeature.feature"
- `Title Case` - primeira letra de cada palavra maiúscula (com espaços) ex: "My Fancy Feature.feature"
- `camelCase` - primeira letra de cada palavra maiúscula, exceto a primeira ex: "myFancyFeature.feature"
- `kebab-case` - todas em minúsculas, delimitado por hífen ex: "my-fancy-feature.feature"
- `snake_case` - todas em minúsculas, delimitado por sublinhado ex: "my_fancy_feature.feature"

---

### no-restricted-patterns

`no-restricted-patterns` é uma lista de padrões exatos ou parciais cujas correspondências são proibidas no nome e descrição da funcionalidade, e no nome, descrição e passos de background, cenário e esboço de cenário.
Todos os padrões são tratados como insensíveis a maiúsculas e minúsculas.

```json
{
  "no-restricted-patterns": [
    "on", 
    {
      "Global": [
        "^globally restricted pattern"
      ],
      "Feature": [
        "poor description",
        "validate",
        "verify"
      ],
      "Background": [
        "show last response",
        "a debugging step"
      ],
      "Scenario": [
        "show last response",
        "a debugging step"
      ]
    }
  ]
}
```

Notas:
- Palavras-chave de passos `Dado`, `Quando`, `Então` e `E` não devem ser incluídas nos padrões.
- Violações de descrição sempre são relatadas na linha de definição da Feature/Cenário/etc. Isso ocorre porque a árvore gherkin analisada não possui informações sobre em que linha a descrição aparece.

---

### indentation

`indentation` é uma regra que serve ser configurada de forma mais granular a configuração da identação do gherkin  e tem as seguintes regras padrão:
- Indentação esperada para Features, Backgrounds, Scenarios e Examples headings: 0 espaços
- Indentação esperada para Steps e cada exemplo de cada Esboço de Cenário: 2 espaços


```json
{
  "indentation": [
    "on", 
    {
      "Feature": 0,
      "Background": 0,
      "Scenario": 0,
      "Step": 2,
      "Examples": 0,
      "example": 2,
      "given": 2,
      "when": 2,
      "then": 2,
      "and": 2,
      "but": 2,
      "feature tag": 0,
      "scenario tag": 0
    }
  ]
}
```

---

### max-scenarios-per-file
A `max-scenarios-per-file` é responsavel por definir a quantidade maxima de cenários por arquivo e aceita dois argumentos na configuração:

- `maxScenarios` (número) o número máximo de cenários por arquivo após o qual a regra falha - padrão é `10`
- `countOutlineExamples` (booleano) se deve contar cada linha de exemplo para um "Exemple Outline", ao invés de apenas 1 para todo o bloco - padrão é `true`

```json
{
  "max-scenarios-per-file": [
    "on", 
    {
      "maxScenarios": 19, 
      "countOutlineExamples": true
    }
  ]
}
```
---
### name-length

`name-length` serve para definir o numero maximo de caracteres nos nomes de Feature, Cenário e Passo.
O padrão é 70 caracteres para cada um destes:

```json
{
  "name-length" : [
    "on", 
    { 
      "Feature": 70, 
      "Scenario": 70, 
      "Step": 70
    }
  ]
}
```
---
### new-line-at-eof

`new-line-at-eof` é uma regra que pode ser configurada para impor ou proibir a presença de uma nova linha no final de um arquivo. 

```json
{
"new-line-at-eof": ["on", "yes"]
}
```

---
### no-dupe-scenario-names


`no-dupe-scenario-names` é uma regra que verifica se existem nomes de cenários duplicados dentro de cada arquivo de feature individual ou em todo projeto.


```json
{
  "no-dupe-scenario-names": [
    "on", 
    "in-feature"
  ]
}
```

ou

```json
{
  "no-dupe-scenario-names": [
    "on", 
    "anywhere"
  ]
}
```
---

### no-restricted-tags
`no-restricted-tags` é uma regra que proíbe o uso de tags específicas. É possível configurar esta regra para permitir apenas um conjunto predefinido de tags, ou para proibir tags que correspondam a um padrão.

```json
{
  "no-restricted-tags": [
    "on", 
    {
      "tags": [
        "@watch", 
        "@wip"
      ], 
      "patterns": [
        "^@todo$"
      ]
    }
  ]
}
```
---
### required-tags


`required-tags` é uma regra que exige a presença de uma ou mais tags em cada cenário. Essa regra pode ser configurada de várias formas, como por exemplo:

- `tags` (array): especifica um conjunto de padrões de tags que devem corresponder a pelo menos uma tag em cada cenário. O padrão é um array vazio, o que significa que nenhuma tag é exigida.
- `ignoreUntagged` (booleano): indica se os cenários sem tag devem ser ignorados ou não. O padrão é `true`, o que significa que os cenários sem tag serão ignorados. Se for definido como `false`, o linter irá relatar cenários sem tag.

Por exemplo, a configuração abaixo exige a presença de uma tag que corresponde ao padrão `^@issue:\d*$` em todos os cenários:


```json
{
  "required-tags": [
    "on", 
    { 
      "tags": [
        "^@issue:[1-9]\\d*$"
      ], 
      "ignoreUntagged": false
    }
  ]
}
```

---
### scenario-size


A regra `scenario-size` permite especificar um comprimento máximo de passos para cenários e backgrounds. Essa configuração pode ser útil em situações em que você quer garantir que seus cenários estejam simples e não tenham muitos passos.

A configuração `Scenario` aplica-se tanto a cenários quanto a "Scenario Outline".

```json
{
  "scenario-size": [
    "on", 
    { 
      "steps-length": { 
        "Background": 15,   
        "Scenario": 15 
      }
    }
  ]
}
```


## Arquivo de Configuração
O nome padrão para o arquivo de configuração é `.gherkin-lintrc` e espera-se que esteja no seu diretório de trabalho.

O conteúdo do arquivo deve ser JSON válido, embora permita comentários.

Se você estiver usando um arquivo com um nome diferente ou um arquivo em uma pasta diferente, precisará especificar a opção `-c` ou `--config` e passar

o caminho relativo para o seu arquivo de configuração. Ex: `gherkin-lint -c caminho/para/arquivo/de/configuração.gherkin-lintrc`


## Regras Personalizadas
Você pode especificar um ou mais diretórios de regras personalizadas usando a opção de linha de comando `-r` ou `--rulesdir`. Regras nos diretórios dados estarão disponíveis além das regras padrão.

Exemplo:
```
gherkin-lint --rulesdir "/caminho/para/meu/rulesdir" --rulesdir "a partir/do/diretório/atual/rulesdir"
```

# Json Schema
O JSON Schema é uma linguagem declarativa que permite anotar e validar documentos JSON.
O JSON Schema possibilita o uso confiante e confiável do formato de dados JSON.

## Beneficios
Descreve seus formatos de dados existentes.
Fornece documentação clara e legível para humanos e máquinas.
Valida dados, o que é útil para:
  Testes automatizados.
  Garantir a qualidade dos dados submetidos pelo cliente.

## Introdução 
Vamos fingir que estamos interagindo com um catálogo de produtos baseado em JSON. Este catálogo tem um produto que possui:

Um identificador: productId
Um nome de produto: productName
Um custo de venda para o consumidor: price
Um conjunto opcional de tags: tags.
Por exemplo:

```
{
  "productId": 1,
  "productName": "Uma porta verde",
  "price": 12.50,
  "tags": [ "casa", "verde" ]
}
```

Embora geralmente direto, o exemplo deixa algumas perguntas em aberto. Aqui estão apenas algumas delas:

O que é _productId_?
O _productName_ é obrigatório?
O preço pode ser zero (0)?
Todas as tags são valores de string?

Quando você está falando sobre um formato de dados, você quer ter metadados sobre o que as chaves significam, incluindo as entradas válidas para essas chaves. JSON Schema é um padrão IETF proposto sobre como responder a essas perguntas para dados.

## Entendendo o JSON Schema
O JSON Schema é uma ferramenta poderosa para validar a estrutura dos dados JSON. No entanto, aprender a usá-lo lendo sua especificação é como aprender a dirigir um carro olhando seus projetos. Você não precisa saber como um motor elétrico se encaixa se tudo o que você quer é buscar as compras. Portanto, este livro tem como objetivo ser o instrutor de direção amigável para o JSON Schema. É para aqueles que querem escrever e entender, mas talvez não estejam interessados em construir seu próprio carro - isto é, escrever seu próprio validador de JSON Schema - ainda.


## O que é um esquema?
Se você já usou XML Schema, RelaxNG ou ASN.1, provavelmente já sabe o que é um esquema e pode felizmente pular para a próxima seção. Se tudo isso parece gobbledygook para você, você veio ao lugar certo. Para definir o que é JSON Schema, provavelmente devemos primeiro definir o que é JSON.

JSON é a sigla para "JavaScript Object Notation", um formato simples de troca de dados. Começou como uma notação para a world wide web. Como o JavaScript existe na maioria dos navegadores web, e o JSON é baseado em JavaScript, é muito fácil suportá-lo lá. No entanto, ele se mostrou útil o suficiente e simples o suficiente para que agora seja usado em muitos outros contextos que não envolvem navegação na web.

Em seu coração, o JSON é construído nas seguintes estruturas de dados:


```json
// objeto:
{ "chave1": "valor1", "chave2": "valor2" }
```

```json
// array:
[ "primeiro", "segundo", "terceiro" ]
```

```json
// número:
42
3.1415926
```

```json
// string:
"Esta é uma string"
```

```json
// booleano:
true
false
```

```json
// nulo:
null
```
Com esses tipos de dados simples, todos os tipos de dados estruturados podem ser representados. Com essa grande flexibilidade vem uma grande responsabilidade, entretanto, já que o mesmo conceito poderia ser representado de inúmeras maneiras. Por exemplo, você poderia imaginar representar informações sobre uma pessoa em JSON de diferentes maneiras:

```json
{
  "nome": "George Washington",
  "aniversario": "22 de fevereiro de 1732",
  "endereco": "Mount Vernon, Virginia, Estados Unidos"
}
```

```json
{
  "primeiro_nome": "George",
  "sobrenome": "Washington",
  "aniversario": "1732-02-22",
  "endereco": {
    "endereco_rua": "3200 Mount Vernon Memorial Highway",
    "cidade": "Mount Vernon",
    "estado": "Virginia",
    "pais": "Estados Unidos"
  }
}
```
Ambas as representações são igualmente válidas, embora uma seja claramente mais formal que a outra. O design de um registro dependerá em grande parte de seu uso pretendido dentro do aplicativo, então não há uma resposta certa ou errada aqui. No entanto, quando um aplicativo diz "me dê um registro JSON para uma pessoa", é importante saber exatamente como esse registro deve ser organizado. Por exemplo, precisamos saber quais campos são esperados e como os valores são representados. É aí que entra o JSON Schema. O seguinte fragmento de JSON Schema descreve como o segundo exemplo acima é estruturado. Não se preocupe muito com os detalhes por enquanto. Eles são explicados nos capítulos subsequentes.

```json
{
  "type": "object",
  "properties": {
    "primeiro_nome": { "type": "string" },
    "sobrenome": { "type": "string" },
    "aniversario": { "type": "string", "format": "date" },
    "endereco": {
      "type": "object",
      "properties": {
        "endereco_rua": { "type": "string" },
        "cidade": { "type": "string" },
        "estado": { "type": "string" },
        "pais": { "type" : "string" }
      }
    }
  }
}
```

Você deve ter notado que o próprio JSON Schema é escrito em JSON. Ele é um dado em si, não um programa de computador. É apenas um formato declarativo para "descrever a estrutura de outros dados". Esta é tanto a sua força quanto a sua fraqueza (que compartilha com outras linguagens de esquema similares). É fácil descrever concisamente a estrutura superficial dos dados e automatizar a validação dos dados contra ela. No entanto, como um JSON Schema não pode conter código arbitrário, existem certas restrições sobre as relações entre os elementos de dados que não podem ser expressas. Qualquer "ferramenta de validação" para um formato de dados suficientemente complexo, portanto, provavelmente terá duas fases de validação: uma no nível do esquema (ou estrutural) e uma no nível semântico. A última verificação provavelmente precisará ser implementada usando uma linguagem de programação de propósito geral.

## Iniciando o esquema
Para iniciar uma definição de esquema, vamos começar com um esquema JSON básico.

Começamos com quatro propriedades chamadas palavras-chave que são expressas como chaves JSON.

Sim. O padrão usa um documento de dados JSON para descrever documentos de dados, mais frequentemente que também são documentos de dados JSON, mas podem estar em qualquer número de outros tipos de conteúdo, como text/xml.

A palavra-chave _'$schema'_ afirma que este esquema é escrito de acordo com um rascunho específico do padrão e usado por uma variedade de razões, principalmente controle de versão.

A palavra-chave _'$id'_ define um URI para o esquema e o URI base que outras referências de URI dentro do esquema são resolvidas.

As palavras-chave _'title'_ e _'description'_ são apenas anotações do esquema e não impõem restrições aos dados sendo validados. Elas servem para declarar a intenção do esquema.

A palavra-chave _'type'_, é responsável pela validação, impõe a primeira restrição aos nossos dados JSON, que neste caso, precisam ser um objeto JSON.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto no catálogo",
  "type": "object"
}
```

## Definindo as propriedades
productId é um valor numérico que identifica exclusivamente um produto. Como este é o identificador canônico para um produto, não faz sentido ter um produto sem um, então é obrigatório.

Em termos de JSON Schema, atualizamos nosso esquema para adicionar:

A palavra-chave de validação de propriedades.
A chave productId.
A anotação de esquema de descrição e a palavra-chave de validação de tipo são observadas - nós cobrimos ambas na seção anterior.
A palavra-chave de validação obrigatória listando productId.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    }
  },
  "required": [ "productId" ]
}
```

productName é um valor de string que descreve um produto. Como não há muito em um produto sem um nome, ele também é necessário.
Como a palavra-chave de validação requerida é uma matriz de strings, podemos notar várias chaves como necessárias; Agora incluímos productName.
Não há realmente nenhuma diferença entre productId e productName - incluímos ambos para completude, pois os computadores normalmente prestam atenção a identificadores e os humanos normalmente prestam atenção a nomes.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    },
    "productName": {
      "description": "Nome do produto",
      "type": "string"
    }
  },
  "required": [ "productId", "productName" ]
}
```

## Aprofundando nas propriedades
De acordo com o dono da loja, não existem produtos gratuitos. ;)

A chave price é adicionada com a anotação de esquema de descrição usual e as palavras-chave de validação de tipo cobertas anteriormente. Também está incluído na matriz de chaves definida pela palavra-chave de validação necessária.
Especificamos que o valor de price deve ser algo diferente de zero usando a palavra-chave de validação exclusiveMinimum.
Se quiséssemos incluir zero como um preço válido, teríamos especificado a palavra-chave de validação minimum.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    },
    "productName": {
      "description": "Nome do produto",
      "type": "string"
    },
    "price": {
      "description": "O preço do produto",
      "type": "number",
      "exclusiveMinimum": 0
    }
  },
  "required": [ "productId", "productName", "price" ]
}
```

A seguir, chegamos à chave tags.

O dono da loja disse o seguinte:

Se houver tags, deve haver pelo menos uma tag,
Todas as tags devem ser únicas; sem duplicação dentro de um único produto.
Todas as tags devem ser texto.
As tags são legais, mas não é necessário que estejam presentes.
Portanto:

A chave tags é adicionada com as anotações e palavras-chave usuais.
Desta vez, a palavra-chave de validação de tipo é array.
Introduzimos a palavra-chave de validação de itens para que possamos definir o que aparece na matriz. Neste caso: valores de string através da palavra-chave de validação de tipo.
A palavra-chave de validação minItems é usada para garantir que haja pelo menos um item na matriz.
A palavra-chave de validação uniqueItems observa que todos os itens na matriz devem ser únicos em relação uns aos outros.
Não adicionamos esta chave à matriz da palavra-chave de validação necessária porque ela é opcional.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    },
    "productName": {
      "description": "Nome do produto",
      "type": "string"
    },
    "price": {
      "description": "O preço do produto",
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "description": "Tags para o produto",
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1,
      "uniqueItems": true
    }
  },
  "required": [ "productId", "productName", "price" ]
}
```

## Estruturas de dados aninhadas
Até este ponto, estávamos lidando com um esquema muito plano - apenas um nível. Esta seção demonstra estruturas de dados aninhadas.

A chave dimensions é adicionada usando os conceitos que descobrimos anteriormente. Como a palavra-chave de validação de tipo é object, podemos usar a palavra-chave de validação de properties para definir uma estrutura de dados aninhada.
Omitimos a palavra-chave de anotação de description para brevidade no exemplo. Embora geralmente seja preferível anotar completamente, neste caso a estrutura e os nomes das chaves são bastante familiares para a maioria dos desenvolvedores.
Você notará que o escopo da palavra-chave de validação required é aplicável à chave dimensions e não além.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    },
    "productName": {
      "description": "Nome do produto",
      "type": "string"
    },
    "price": {
      "description": "O preço do produto",
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "description": "Tags para o produto",
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1,
      "uniqueItems": true
    },
    "dimensions": {
      "type": "object",
      "properties": {
        "length": {
          "type": "number"
        },
        "width": {
          "type": "number"
        },
        "height": {
          "type": "number"
        }
      },
      "required": [ "length", "width", "height" ]
    }
  },
  "required": [ "productId", "productName", "price" ]
}
```
## Referências fora do esquema
Até agora, nosso esquema JSON foi totalmente autônomo. É muito comum compartilhar esquemas JSON entre muitas estruturas de dados para reutilização, legibilidade e manutenibilidade, entre outras razões.

Para este exemplo, introduzimos um novo recurso de esquema JSON e para ambas as propriedades nele:

Usamos a palavra-chave de validação mínima notada anteriormente.
Adicionamos a palavra-chave de validação máxima.
Combinados, estes nos dão uma faixa para usar na validação.
```json
{
  "$id": "https://example.com/geographical-location.schema.json",
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Longitude and Latitude",
  "description": "A coordenada geográfica em um planeta (mais comumente Terra).",
  "required": [ "latitude", "longitude" ],
  "type": "object",
  "properties": {
    "latitude": {
      "type": "number",
      "minimum": -90,
      "maximum": 90
    },
    "longitude": {
      "type": "number",
      "minimum": -180,
      "maximum": 180
    }
  }
}
```
Em seguida, adicionamos uma referência a este novo esquema para que ele possa ser incorporado.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/product.schema.json",
  "title": "Product",
  "description": "Um produto do catálogo da Acme",
  "type": "object",
  "properties": {
    "productId": {
      "description": "O identificador único para um produto",
      "type": "integer"
    },
    "productName": {
      "description": "Nome do produto",
      "type": "string"
    },
    "price": {
      "description": "O preço do produto",
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "description": "Tags para o produto",
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1,
      "uniqueItems": true
    },
    "dimensions": {
      "type": "object",
      "properties": {
        "length": {
          "type": "number"
        },
        "width": {
          "type": "number"
        },
        "height": {
          "type": "number"
        }
      },
      "required": [ "length", "width", "height" ]
    },
    "warehouseLocation": {
      "description": "Coordenadas do armazém onde o produto está localizado.",
      "$ref": "https://example.com/geographical-location.schema.json"
    }
  },
  "required": [ "productId", "productName", "price" ]
}
```

## Dando uma olhada nos dados para o nosso esquema JSON definido

Certamente expandimos o conceito de um produto desde nossos primeiros dados de amostra (role até o topo). Vamos dar uma olhada nos dados que correspondem ao esquema JSON que definimos.

```json
{
  "productId": 1,
  "productName": "Uma escultura de gelo",
  "price": 12.50,
  "tags": [ "frio", "gelo" ],
  "dimensions": {
    "length": 7.0,
    "width": 12.0,
    "height": 9.5
  },
  "warehouseLocation": {
    "latitude": -78.75,
    "longitude": 20.4
  }
}
```


## Referencia

Documentação Oficial
https://json-schema.org/specification.html
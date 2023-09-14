# Garantindo a Qualidade de APIs com Validação de JSON Schema

No mundo digital de hoje, a capacidade de compreender e manipular dados é essencial. Isso se torna ainda mais relevante em um cenário onde temos várias aplicações se comunicando uma com a outra. O JSON, surgiu como um "padrão" para esta comunicação, facilitando a troca de dados entre diferentes plataformas e linguagens. 

Entender sua estrutura e nuances não é apenas uma habilidade técnica, mas uma necessidade para garantir a fluidez e a eficiência da troca de informações. Neste artigo, vamos explorar o JSON Schema, um aliado essencial que nos ajuda a assegurar que os dados transmitidos mantenham sua integridade e conformidade.

> Na próxima seção vamos abordar primeiramente a estrutura de um JSON, então, se você já tem conhecimentos sobre o que é JSON e como funciona sua estrutura, sinta-se à vontade para pular para o próximo capítulo (clique aqui). Mas se você está pronto para uma revisão ou está apenas começando sua jornada com JSON, continue lendo e vamos juntos entender mais sobre JSON.

## Estrutura JSON

Vamos começar pelo básico. JSON é a sigla para "JavaScript Object Notation", que em uma tradução livre significa "Notação de Objeto em JavaScript". Esta nomenclatura pode parecer técnica, mas seu conceito é simples, ser um formato de texto leve e de fácil leitura para troca de dados.

### Objetos e Arrays

A "raiz" da estrutura JSON se apoia em dois elementos fundamentais: `objetos` e `arrays`. Esses elementos são a base para a construção de qualquer estrutura de dados em JSON, permitindo a representação de informações de forma organizada e hierárquica.

Objetos: Delimitados por chaves `{}`, representam uma estrutura que contém informações. Por exemplo:
```json
{
  "id": 123,
  "nome": "Maria"
}
```

Arrays: Delimitados por colchetes `[]`, são listas de itens. Por exemplo:
```json
["Maçã", "Banana", "Cereja"]
```

E ainda podemos ter Arrays de Objetos, que combinam ambos os conceitos. Por exemplo:
```json
[
  {
    "id": 123,
    "nome": "Maria"
  },
  {
    "id": 124,
    "nome": "João"
  },
  {
    "id": 125,
    "nome": "Erick"
  }
]
```
Estes são os conceitos básicos de objetos e arrays em JSON. Detalharemos agora um pouco mais sobre cada tipo e sua estrutura.

### Propriedades na Estrutura JSON

A estrutura do JSON é composta por pares de chave e valor. Cada item desse par é chamado de "propriedade" do objeto raiz.

```json
{
  "chave": "valor",
  "outraChave": "outroValor"
}
```

Diferentemente dos objetos em JavaScript, as chaves das propriedades JSON devem estar entre aspas duplas.

#### O que é uma String em JSON?

Por definição, uma string é uma sequência de caracteres. Em JSON, tanto a chave quanto o valor de uma propriedade do tipo string devem estar entre aspas duplas.

```json
{
  "chave": "Este texto é uma string, string sempre fica dentro de aspas"
}
```

Se um objeto JSON tiver mais de uma propriedade, elas devem ser separadas por vírgulas. Além disso, é comum usar alguma notação para palavras compostas, como por exemplo o camelCase, snake_case, kebab-case e outros padrões para nomear as chaves. Ao escolher um padrão, é essencial manter a consistência ao longo do projeto para garantir clareza e facilidade de manutenção.

#### O que é um Número em JSON?

Enquanto algumas linguagens têm estruturas de dados específicas para inteiros, em JSON, usamos a estrutura genérica de número.

```json
{
  "umaString": "Texto aqui",
  "umNumero": 10,
  "umNumeroDecimal": 4.99
}
```

Os números podem ser inteiros ou de ponto flutuante. No entanto, em JSON, não há distinção específica entre eles. Números entre aspas são interpretados como strings.

#### O que é um Booleano em JSON?

Um booleano é uma estrutura de dados binária. Pode ser verdadeiro (true) ou falso (false).

```json
{
  "umaString": "Texto aqui",
  "umNumero": 123,
  "umBooleano": true,
  "umOutroBooleano": false
}
```

#### O que é um Objeto em JSON?

Um objeto em JSON é um conjunto de pares chave-valor. Ele pode conter propriedades de qualquer tipo. Objetos podem conter outros objetos como valores de uma chave.

```json
{
  "id": 124,
  "nome": "João",
  "informacoesAdicionais": {
    "tocaGuitarra": true,
    "tocaPiano": false
  }
}
```

A propriedade "informacoesAdicionais" neste exemplo é um objeto aninhado (Objeto aninhado é basicamente quando um objeto esta dentro dentro de outro objeto).

#### O que é um Array em JSON?

Um array é uma lista ordenada de valores.

```json
{
  "id": 124,
  "nome": "João",
  "frutasPreferidas": ["Pêra", "Abacate", "Morango"]
}
```

Arrays podem conter diferentes tipos de estruturas de dados.

```json
{
  "diferentesTipos": ["João", 123, null, 1.99, "123", {"nome": "Maria"}]
}
```

#### O que é null em JSON?

`Null` representa a ausência de valor.

```json
{
  "id": 124,
  "nome": "João",
  "informacoesAdicionais": null
}
```

## Aprendi o que é JSON?

Para fixarmos o nosso conhecimento em JSON e darmos continuidade nas próximas seções vamos subir um pequena API que retorna uma resposta JSON.

## JSON Schema

### Por que usar a validação de JSON Schema?

A validação através do JSON Schema é mais do que uma técnica: é uma estratégia que potencializa a eficiência e confiabilidade das APIs. Se ainda não decidiu se deve usar aqui estão quatro razões fundamentais para adotá-la:

#### Garantia de Qualidade e Consistência

A validação assegura que os dados processados sejam consistentes e de alta qualidade. Isso evita erros e falhas que podem surgir de dados mal formatados.

> Exemplo: Suponha que sua API deva retornar informações sobre um usuário, incluindo nome, idade e e-mail. Com o JSON Schema, você pode definir que o nome e o e-mail são strings obrigatórias, enquanto a idade é um número inteiro. Qualquer resposta que não atenda a esses critérios será sinalizada.

#### Documentação Simplificada e Precisa

O JSON Schema serve como uma referência clara para o esquema da API, facilitando a comunicação entre equipes e garantindo que todos estejam alinhados quanto à estrutura esperada.

> Exemplo: Se sua API deve retornar uma lista de produtos, o esquema pode especificar que cada produto tem um nome (string), preço (número decimal) e disponibilidade (booleano). Isso torna claro para os desenvolvedores o formato esperado da resposta.

#### Feedback Detalhado e Acionável

A validação proporciona feedbacks específicos sobre erros, facilitando a correção e garantindo que os consumidores da API saibam exatamente o que precisa ser ajustado.

> Exemplo: Se um campo obrigatório estiver ausente na resposta da API, o JSON Schema pode retornar uma mensagem como "O campo 'nome' é obrigatório e está ausente na resposta".

#### Otimização do Processo de Desenvolvimento

Com um esquema bem definido, é possível automatizar muitos aspectos do desenvolvimento, desde a geração de código até testes.

>Exemplo: Ao desenvolver uma nova funcionalidade que retorna detalhes de um pedido, o esquema pode ser usado para gerar automaticamente testes que verificam se a resposta da API está em conformidade com o esperado.

Em resumo, a validação de JSON Schema é uma ferramenta poderosa que, quando bem aplicada, pode melhorar significativamente a robustez e confiabilidade de uma API.

### Como criar um JSON Schema

Ao desenvolver uma API, é crucial garantir a consistência e a qualidade dos dados transmitidos. Uma das maneiras de fazer isso é através do uso de um JSON Schema. Vamos explorar como criar um esquema para a resposta da rota GET `/estudantes` da nossa API:

```json
{
  "matricula": 12345,
  "primeiroNome": "João",
  "sobrenome": "Silva",
  "idade": 22,
  "email": "joao.silva@gmail.com",
  "escolaridade": "Ensino Medio",
  "cursos": [
    {
      "materia": "Matemática",
      "notaAtual": 85
    }, 
    {
      "materia": "Física",
      "notaAtual": 78.5
    },
    {
      "materia": "Química",
      "notaAtual": 68
    }
  ],
  "pais": {
    "mae": "Antonia Silva",
    "pai": "Geraldo Silva"
  }
}
```

Neste exemplo, temos informações sobre um estudante, incluindo detalhes como nome, idade, matrícula e os cursos que está cursando. Ao analisar a estrutura, podemos identificar diferentes tipos de dados, como inteiros (`matricula` e `idade`), strings com validações simples (`primeiroNome`, `sobrenome`), strings com formatos específicos (`email` e `escolaridade`), arrays (`cursos`) e objetos aninhados (`pais`).

Para começar a definir nosso JSON Schema, iniciamos com:

```json
{
  "$id": "https://api-school-demo.erickribeiro.me/json-schemas/estudantes.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Estudantes",
  "description": "Esquema para a resposta da API que retorna informações dos estudantes.",
  "type": "object"
}
```

Aqui, o `"$id"` serve como um identificador único para o nosso esquema, útil para referências futuras. O `"$schema"` especifica a versão do padrão JSON Schema que estamos usando. Os campos `"title"` e `"description"` fornecem informações adicionais sobre o esquema, mas não impõem validações. Por fim, o campo `"type"` define que os dados devem ser um objeto JSON.

Com esse esquema básico em mãos, podemos começar a adicionar mais detalhes e restrições para garantir que os dados recebidos se alinhem às nossas expectativas.

#### Definindo as propriedades

A `matricula` é um valor numérico que identifica exclusivamente um estudante. Sendo o identificador único do aluno, é um valor obrigatório para cada estudante.

Podemos atualizar nosso JSON Schema da seguinte forma:

```json
"properties": {
  "matricula": {
    "description": "O identificador único do estudante.",
    "type": "integer"
  }
},
"required": ["matricula"]
```

Aproveitando a inclusão de validações para propriedades numéricas, vamos adicionar uma validação para a propriedade `idade`. Além de tornar este campo obrigatório, queremos garantir que o valor não seja menor que 0 e nem maior que 120. Isso se baseia na premissa de que um estudante dificilmente terá mais de 120 anos e certamente não terá menos de 0 anos. Lembrando que o mais correto é esta validação estar alinhada com as regras de negocio da aplicação.

```json
"properties": {
  "matricula": {
    "description": "O identificador único do estudante.",
    "type": "integer"
  },
  "idade": {
    "description": "Idade do estudante.",
    "type": "integer",
    "minimum": 0,
    "maximum": 120
  }
},
"required": ["matricula", "idade"]
```

Dessa forma, garantimos que os dados inseridos estejam de acordo com as regras estabelecidas para cada propriedade.

Para validar "strings comuns", ou seja, aquelas que não seguem um padrão específico, podemos estabelecer limites para o número de caracteres. Tomando como exemplo os campos `primeiroNome` e `sobrenome`, podemos adicionar as seguintes definições ao nosso JSON Schema:

```json
"properties": {
  "primeiroNome": {
    "description": "O primeiro nome do estudante.",
    "type": "string",
    "minLength": 3,
    "maxLength": 128
  },
  "sobrenome": {
    "description": "O sobrenome do estudante.",
    "type": "string",
    "minLength": 3,
    "maxLength": 128
  }
},
"required": ["matricula", "idade", "primeiroNome", "sobrenome"]
```

Este documento é bastante claro. Os campos `primeiroNome` e `sobrenome` são do tipo string e devem conter, no mínimo, 3 caracteres e, no máximo, 128 caracteres. Além disso, são campos obrigatórios, assim como os campos `matricula` e `idade` que foram definidos anteriormente.

> Vale ressaltar que, para números, utilizamos `minimum` e `maximum` para definir valores mínimos e máximos, respectivamente. Já para strings, quando queremos estabelecer um limite de caracteres, usamos `minLength` e `maxLength`.



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

Criar um esquema JSON para sua API pode ser facilitado com a ajuda de várias ferramentas e recursos. Por exemplo, o JSON Schema Generator é uma ferramenta online que permite criar um esquema JSON a partir de uma amostra de dados JSON ou de um rascunho de esquema JSON. O JSON Schema Editor é outra ferramenta online que permite editar e visualizar um esquema JSON em um modo gráfico ou de texto. Ele também ajuda a validar seus dados contra seu esquema. JSON Schema Lint é uma ferramenta online que permite validar seu esquema JSON e dados. Ele verifica a compatibilidade, além das melhores práticas. Por último, o JSON Schema Faker é uma ferramenta online que permite gerar dados fictícios a partir do seu esquema JSON, bem como exportar seus dados para vários formatos.

Embora as ferramentas online possam ser convenientes, elas nem sempre são a opção mais segura.

Ao usar esses serviços, você está confiando que o provedor da ferramenta protegerá seus dados. Embora eu acredite que eles levam a segurança a sério, sempre há um risco de seus dados serem comprometidos. Para alguns casos de uso, pode ser mais seguro confiar em uma ferramenta offline (por exemplo, GenSON, uma biblioteca Python que pode ser usada para gerar Esquemas JSON, que também possui uma ferramenta CLI).

Ao optar por usar algo que funciona online, é crucial sempre verificar os dados que você está alimentando na ferramenta e garantir que não esteja colando informações ou dados sensíveis que não deseja compartilhar ou armazenar.

## Como usar a validação de esquema JSON

Usar a validação de esquema JSON para sua API pode ser benéfico dependendo das suas necessidades e preferências. Por exemplo, você pode usá-lo como um middleware para seu servidor de API para validar dados de requisição e resposta. Também pode enviar mensagens de erro apropriadas e códigos de status. Existem várias bibliotecas e frameworks que suportam isso, como Express, Fastify, Hapi e Restify. Além disso, você pode usá-lo como uma biblioteca do lado do cliente para seu consumidor de API para validar dados enviados e recebidos da sua API, bem como tratar erros e exceções com elegância. Existem várias bibliotecas e frameworks que suportam isso, como Ajv, JSON Schema Validator e tv4. Por último, você pode usá-lo como um gerador de código para sua API para gerar código do lado do cliente e do servidor a partir do seu esquema JSON. Também pode ser usado para dados fictícios e testes. Existem várias ferramentas e plataformas que suportam isso, como OpenAPI Generator, JSON Schema to TypeScript e Postman.

Além disso, o esquema JSON pode ser usado para simplificar a reformulação de dados de formatos que são específicos para sua aplicação em formatos que a API (que o esquema JSON descreve) consome.

Vai desde tipos que podem ser gerados em TypeScript para o JSON em questão e ver sugestões de preenchimento automático no seu editor de código ou IDE até o uso de ChatGPT, GitHub Copilot ou LLMs similares para escrever automaticamente conversores de dados. O que não seria tão possível sem ter um esquema JSON.

Em uma Arquitetura Orientada a Eventos, usamos o esquema JSON como APIs para garantir que os dados compartilhados em nosso sistema distribuído aderem ao contrato, mantendo os dados legíveis por humanos.

### Como documentar seu esquema JSON

Documentar seu esquema JSON é uma etapa importante para tornar sua API fácil de entender e usar. Existem vários métodos para documentar seu esquema JSON, como usar anotações e palavras-chave de esquema JSON para adicionar descrições, exemplos, títulos e outros metadados, bem como definir regras de validação personalizadas e mensagens de erro. Além disso, você pode usar geradores de documentação de esquema JSON para criar documentação legível e interativa, ou padrões de documentação de esquema JSON para seguir melhores práticas e convenções. Existem várias ferramentas e plataformas que suportam esses métodos, como JSON Schema Docs, Docson, Redoc, OpenAPI Specification, JSON Hyper-Schema e JSON API.








---

Ao trabalhar com JSON, é crucial garantir a qualidade e a integridade dos dados. A compreensão clara de cada tipo e sua representação ajuda a evitar erros e garante uma comunicação de dados eficiente.













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
        "numero": {"type": "number"},
        "cidade": { "type": "string" },
        "estado": { "type": "string" },
        "pais": { "type" : "string" }
      }
    }
  }
}
```

Você deve ter notado que o próprio JSON Schema é escrito em JSON. Ele é um dado em si, não um programa de computador. É apenas um formato declarativo para "descrever a estrutura de outros dados". Esta é tanto a sua força quanto a sua fraqueza (que compartilha com outras linguagens de esquema similares). É fácil descrever concisamente a estrutura superficial dos dados e automatizar a validação dos dados contra ela. No entanto, como um JSON Schema não pode conter código arbitrário, existem certas restrições sobre as relações entre os elementos de dados que não podem ser expressas. Qualquer "ferramenta de validação" para um formato de dados suficientemente complexo, portanto, provavelmente terá duas fases de validação: uma no nível do esquema (ou estrutural) e uma no nível semântico. A última verificação provavelmente precisará ser implementada usando uma linguagem de programação de propósito geral.




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



## Iniciando o esquema
P

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
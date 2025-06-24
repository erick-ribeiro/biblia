### Guia Definitivo para Automação de Testes de API com Playwright: Parte 2 - Adicionando Verificações Mais Detalhadas

Bem-vindo de volta! Se você perdeu a Introdução e a Parte 1, certifique-se de entender o contexto do que estamos fazendo e por que estamos fazendo isso. Neste artigo, vou focar em adicionar mais profundidade às nossas verificações automatizadas de API. Isso nos permitirá cobrir tanto cenários positivos quanto negativos para cada endpoint que queremos testar.

Usamos a documentação do endpoint de reservas para entender o que estávamos testando e criamos um plano para começar. À medida que continuamos nossa jornada, fique à vontade para deixar a documentação aberta e disponível.

Se você está acompanhando o código no GitHub, notará que, no Pull Request (P/R), estou dando alguns passos para construir ferramentas que me permitam criar dados de teste de uma maneira que permita que meus testes sejam executados em paralelo. Eu tinha planejado adicionar isso mais tarde, mas meu código estava ficando desorganizado e precisava ser controlado um pouco. Vou detalhar esses helpers em um futuro post no blog, mas sinta-se à vontade para conferir o código se quiser.

### Vamos adicionar mais profundidade à nossa cobertura!
Nos últimos projetos em que trabalhei, descobri uma estratégia muito boa para usar ao começar do zero: primeiramente, trabalhar para adicionar cobertura em largura. O que quero dizer com isso é adicionar o maior número possível de cenários de *happy path* para cada endpoint e, em algum momento ou quando você atingir cerca de 70-80% de cobertura de largura, começar a adicionar cobertura profunda ou de profundidade para seus endpoints mais críticos e, eventualmente, para todos eles. Isso deve permitir que você obtenha uma cobertura decente e, em seguida, concentre seus esforços em riscos específicos. Essa estratégia também é útil para automação de UI.

### Próximos Passos

Para continuar nossa jornada de automação, vamos começar a adicionar cenários de testes negativos, verificando como a API responde a entradas inválidas, valores ausentes e outras condições adversas. Essas verificações são importantes para garantir que nosso sistema se comporte corretamente não apenas quando tudo está funcionando bem, mas também quando os usuários enviam dados inesperados ou inválidos. 

Esse processo nos permitirá ter uma suíte de testes mais robusta e abrangente, cobrindo não apenas os fluxos ideais, mas também as exceções e erros que podem ocorrer em um ambiente de produção.

Tomar essa abordagem ajudará você a ter uma noção dos pontos problemáticos que enfrentará, das áreas onde pode querer adicionar data factories e funções auxiliares para manter seu código DRY.

Vamos direto para o código!
Na parte 1, finalizamos com um arquivo na pasta tests/booking/ chamado booking.get.spec.ts. Fiz isso por uma razão específica, pois ajudará a manter nossos testes organizados em vez de colocar todos os nossos arquivos em uma única pasta grande. A nomenclatura também segue uma convenção, {name}.{method}.spec.ts. Quando terminamos a parte 1, tínhamos 3 testes diferentes de GET no nosso arquivo spec, e neste artigo vamos finalizar com 8 nesse diretório.

A primeira coisa que vale a pena destacar no arquivo de spec de GET é que peguei a função que usávamos para verificar se uma data era válida e a movi para uma pasta em /lib/helpers e importei esse arquivo, juntamente com a criação de uma chamada de API de autenticação que importei para a pasta /lib/datafactory.

test.beforeAll()
O início do meu teste agora se parece com isto: no nível mais alto, criei uma variável let para cookies, que atualizo no bloco beforeAll. Este método recebe um nome de usuário e uma senha, e retorna a string de cookies para que, quando eu precisar adicionar um header de autorização, ele se pareça com headers: { cookie: cookies }

```typescript
import { test, expect } from "@playwright/test";
import { auth } from "../../lib/datafactory/auth";
import { isValidDate } from "../../lib/helpers/date";

test.describe("booking/ GET requests", async () => {
  let cookies;

  test.beforeAll(async ({ request }) => {
    cookies = await auth("admin", "password");
  });
  ...
```

Verificações adicionais de requisições GET
Para as chamadas de API GET do booking existem apenas 3 caminhos diferentes que são:

- booking/summary?roomid=1 (GET resumo de reservas com um room id específico)
- booking/ (GET todas as reservas com detalhes, requer auth)
- booking/1 (GET reserva por id com detalhes, requer auth)

Para a primeira booking/summary?roomid={id}, adicionei os seguintes testes:

- Com room id válido
- Com room id numérico que não existe
- Sem room id fornecido

```typescript
test("GET booking summary with specific room id", async ({ request }) => {
    const response = await request.get("booking/summary?roomid=1");

    expect(response.status()).toBe(200);

    const body = await response.json();
    expect(body.bookings.length).toBeGreaterThanOrEqual(1);
    expect(isValidDate(body.bookings[0].bookingDates.checkin)).toBe(true);
    expect(isValidDate(body.bookings[0].bookingDates.checkout)).toBe(true);
  });

  test("GET booking summary with specific room id that doesn't exist", async ({ request }) => {
    const response = await request.get("booking/summary?roomid=999999");

    expect(response.status()).toBe(200);

    const body = await response.json();
    expect(body.bookings.length).toBe(0);
  });

  test("GET booking summary with specific room id that is empty", async ({ request }) => {
    const response = await request.get("booking/summary?roomid=");

    expect(response.status()).toBe(500);

    const body = await response.json();
    expect(isValidDate(body.timestamp)).toBe(true);
    expect(body.status).toBe(500);
    expect(body.error).toBe("Internal Server Error");
    expect(body.path).toBe("/booking/summary");
  });
```

Antes de avançar muito, fui muito intencional com o meu espaçamento. Como você pode ver, tento separar áreas nas minhas asserções, primeiro uma pausa entre fazer a requisição e a primeira asserção. Depois disso, sempre que defino uma nova variável contra a qual quero fazer uma asserção, deixo um espaço acima. Seja qual for o estilo de formatação que você adote, à medida que você cria mais testes, ter um estilo consistente pode tornar o trabalho com a base de código mais agradável.

Em cada um desses cenários, eu fiz asserções tanto sobre o response.status() quanto sobre o corpo da resposta. Para esses testes específicos, mantive-os muito genéricos, pois em requisições GET eu não tenho 100% de controle sobre os dados retornados. Por exemplo, se outra pessoa estiver usando o sistema e criando dados que meu teste não conhece, ele não será capaz de fazer asserções contra esses dados específicos, mas pode fazer coisas como verificar o tipo de informação que é retornada: isValidDate, é uma string, é maior que 1, é um boolean, é um número, etc. 

No exemplo abaixo, não temos muitos elementos contra os quais testar, mas eu também sei que essa data de checkin "2022-02-01" estará sempre presente, pois é um dado que é criado pela plataforma. Se isso não existisse, precisaríamos fazer uma chamada POST para criar os dados em um beforeAll ou beforeEach para poder fazer a asserção contra esses dados.

```json
// Corpo de resposta de booking/summary?roomid=1
{
  "bookings": [
    {
      "bookingDates": {
        "checkin": "2022-02-01",
        "checkout": "2022-02-05"
      }
    }
  ]
}
```

O próximo conjunto de testes GET booking/ realmente requer autenticação. Não há parâmetros adicionais que possamos passar, então mantenho as verificações simples:

- Obter todas as reservas com detalhes usando autenticação
- Obter todas as reservas com detalhes sem autenticação

```typescript
test("GET all bookings with details", async ({ request }) => {
    const response = await request.get("booking/", {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(200);

    const body = await response.json();
    expect(body.bookings.length).toBeGreaterThanOrEqual(1);
    expect(body.bookings[0].bookingid).toBe(1);
    expect(body.bookings[0].roomid).toBe(1);
    expect(body.bookings[0].firstname).toBe("James");
    expect(body.bookings[0].lastname).toBe("Dean");
    expect(body.bookings[0].depositpaid).toBe(true);
    expect(isValidDate(body.bookings[0].bookingdates.checkin)).toBe(true);
    expect(isValidDate(body.bookings[0].bookingdates.checkout)).toBe(true);
  });

  test("GET all bookings with details with no authentication", async ({ request }) => {
    const response = await request.get("booking/", {
      headers: { cookie: "test" },
    });

    expect(response.status()).toBe(403);

    const body = await response.text();
    expect(body).toBe("");
  });
```

Nesses testes, temos alguns valores fixos que são usados como dados estáticos, como o bookingId #1. Portanto, nas nossas asserções, decidimos manter esses dados hardcoded. A curto prazo, acho que essa é uma abordagem aceitável, mas há um certo risco envolvido.

O que acontece se alguém fizer uma requisição PUT no bookingId #1 e alterar algum detalhe? Se você adivinhou que nosso teste falharia, você está certo. Nesse cenário, a melhor prática seria refatorar para criar uma nova reserva e, em seguida, fazer asserções sobre essa reserva criada. Faremos isso mais tarde, mas por enquanto, sabendo que os dados são redefinidos a cada 10 minutos para um estado padrão, o risco é relativamente baixo.

O último conjunto de verificações neste arquivo são os testes GET booking/{id}, que também requerem autenticação.

- booking/1 (GET reserva por id com detalhes com autenticação)
- booking/999999 (GET reserva por id que não existe com autenticação)
- booking/1 (GET reserva por id sem autenticação)

```typescript
test("GET booking by id with details", async ({ request }) => {
    const response = await request.get("booking/1", {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(200);

    const body = await response.json();
    expect(body.bookingid).toBe(1);
    expect(body.roomid).toBe(1);
    expect(body.firstname).toBe("James");
    expect(body.lastname).toBe("Dean");
    expect(body.depositpaid).toBe(true);
    expect(isValidDate(body.bookingdates.checkin)).toBe(true);
    expect(isValidDate(body.bookingdates.checkout)).toBe(true);
  });

  test("GET booking by id that doesn't exist", async ({ request }) => {
    const response = await request.get("booking/999999", {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(404);

    const body = await response.text();
    expect(body).toBe("");
  });

  test("GET booking by id without authentication", async ({ request }) => {
    const response = await request.get("booking/1");

    expect(response.status()).toBe(403);

    const body = await response.text();
    expect(body).toBe("");
  });
```

A primeira verificação deste conjunto também verifica dados específicos que esperamos estarem presentes. Uma coisa a se notar é que estamos fazendo asserções em todas as chaves do corpo da resposta. Descobri que essa é uma excelente prática a ser seguida, fazer asserções o mais específicas possível. Neste cenário, decidi apenas verificar se as datas de reserva são válidas, e não os valores hardcoded.

Até este ponto, temos 8 verificações de API (115 linhas de código) com uma cobertura decente em torno de todos os endpoints GET de reservas. Sempre existem maneiras de aprofundar mais, mas sinto-me confiante de que, se houverem mudanças significativas nesses endpoints, seremos alertados através de uma falha nos testes e poderemos tomar medidas para explorar mais a fundo.


Verificações de requisições POST
Vamos apimentar as coisas 🌶️ com uma requisição POST. Ainda não vou me aprofundar nesse conjunto de testes, pois ainda temos as requisições PUT e DELETE para cobrir neste artigo. A seguinte verificação levou um tempo para ser elaborada porque foi necessário criar algumas funções auxiliares e de data factory para simplificar a spec de POST. Não vou entrar nos detalhes dessas funções, mas vou descrever em alto nível:

- `createRandomBookingBody` - requer um room id, data de check-in e data de check-out e cria um corpo que podemos usar para fazer uma requisição POST no endpoint booking/. 💡Podemos também usar isso para fazer asserções no corpo da resposta!!!
- `futureOpenCheckinDate` - requer um room id e retorna uma string de data "2023-03-31T00:00:00.000Z" (isso utiliza a API de obter reservas e faz alguns cálculos rápidos).
- `stringDateByDays` - requer uma string de data e um número opcional. Ele pega o número e adiciona ou subtrai dias com base na data atual e retorna uma string de data "2023-03-24".

```typescript
import { test, expect } from "@playwright/test";
import {
  createRandomBookingBody,
  futureOpenCheckinDate,
} from "../../lib/datafactory/booking";
import { stringDateByDays } from "../../lib/helpers/date";

test.describe("booking/ POST requests", async () => {
  let requestBody;
  let roomId = 1;

  test.beforeEach(async ({ request }) => {
    let futureCheckinDate = await futureOpenCheckinDate(roomId);
    let checkInString = futureCheckinDate.toISOString().split("T")[0];
    let checkOutString = stringDateByDays(futureCheckinDate, 2);

    requestBody = await createRandomBookingBody(
      roomId,
      checkInString,
      checkOutString
    );
  });

  test("POST new booking with full body", async ({ request }) => {
    const response = await request.post("booking/", {
      data: requestBody,
    });

    expect(response.status()).toBe(201);

    const body = await response.json();
    expect(body.bookingid).toBeGreaterThan(1);

    const booking = body.booking;
    expect(booking.bookingid).toBe(body.bookingid);
    expect(booking.roomid).toBe(requestBody.roomid);
    expect(booking.firstname).toBe(requestBody.firstname);
    expect(booking.lastname).toBe(requestBody.lastname);
    expect(booking.depositpaid).toBe(requestBody.depositpaid);

    const bookingdates = booking.bookingdates;
    expect(bookingdates.checkin).toBe(requestBody.bookingdates.checkin);
    expect(bookingdates.checkout).toBe(requestBody.bookingdates.checkout);
  });
});
```

Com cada uma dessas funções no bloco `beforeEach`, posso adicionar testes adicionais e ter novos dados de corpo de requisição e datas disponíveis para check-in para cada teste que eu queira escrever. O teste de requisição POST realmente fará uma nova reserva com o corpo completo preenchido (incluindo parâmetros obrigatórios e opcionais). Note que essa chamada de API não requer autenticação, então nenhum header é passado, e eu passo o `requestBody` que foi gerado através da nossa função `createRandomBookingBody()`.

O bom de criar nosso Body da maneira que fizemos (sem hardcoding) é que agora posso usar a variável e os dados que definimos para o postBody e utilizá-los nas nossas asserções.

```json
// Corpo de resposta da requisição POST booking
{
  "bookingid": 2,
  "booking": {
    "bookingid": 2,
    "roomid": 1,
    "firstname": "Testy",
    "lastname": "McTesterSon",
    "depositpaid": true,
    "bookingdates": {
      "checkin": "2023-05-10",
      "checkout": "2023-05-11"
    }
  }
}
```

As asserções abaixo também estão na mesma ordem do corpo da resposta, isso é intencional para manter a organização! Note que nesse cenário eu pego o body, que é a resposta completa em JSON, crio uma nova variável chamada `booking` e uso isso para as asserções. Gosto de fazer isso pois me ajuda a visualizar rapidamente que estou fazendo asserções no objeto `booking`, e logo abaixo, no objeto `bookingdates`. Para respostas de API pequenas, isso pode não ser tão importante, mas quando você tem um corpo de resposta com 20-50 itens ou precisa iterar por um array de objetos, vai ficar muito feliz por ter seguido esse padrão.

```typescript
const body = await response.json();
expect(body.bookingid).toBeGreaterThan(1);

const booking = body.booking;
expect(booking.bookingid).toBe(body.bookingid);
expect(booking.roomid).toBe(requestBody.roomid);
expect(booking.firstname).toBe(requestBody.firstname);
expect(booking.lastname).toBe(requestBody.lastname);
expect(booking.depositpaid).toBe(requestBody.depositpaid);

const bookingdates = booking.bookingdates;
expect(bookingdates.checkin).toBe(requestBody.bookingdates.checkin);
expect(bookingdates.checkout).toBe(requestBody.bookingdates.checkout);
```

Poderíamos ir mais além e adicionar testes como:

- POST booking com apenas os inputs obrigatórios
- POST booking faltando um input obrigatório
- POST booking com tipos incorretos ("true" em vez de true ou "1" em vez de 1)
- POST booking com autenticação, embora não seja necessária
- POST booking com datas que se sobrepõem a uma reserva existente
- POST booking com datas no passado
- etc...

Mas já validamos que o endpoint com este conjunto de parâmetros está fazendo o que esperamos, então vamos seguir em frente!

Verificações de requisições DELETE
Para este conjunto de verificações, vou usar novamente as funções de data factory que criei, como `createFutureBooking` e `auth`, para configurar o teste. Observe que estou criando variáveis `let` no bloco `describe`, algumas com valores definidos e outras que terão seus valores atribuídos no `beforeAll` (se não estiver mudando) ou no bloco `beforeEach`, que atribuirá um novo valor à variável cada vez antes que um teste seja executado. Mantive o `roomId` hardcoded neste teste como 1, pois sei que o sistema terá ele disponível, mas isso é um risco porque alguém poderia deletar o `roomId` 1. Nesse caso, eu precisaria criar um quarto e uma reserva para executar nosso teste de exclusão de uma reserva. Os 3 testes que estou incluindo são:

- DELETE booking com um id de quarto específico
- DELETE booking com um id que não existe
- DELETE booking sem autenticação

```typescript
import { test, expect } from "@playwright/test";
import { auth } from "../../lib/datafactory/auth";
import {
  getBookingSummary,
  createFutureBooking,
} from "../../lib/datafactory/booking";

test.describe("booking/{id} DELETE requests", async () => {
  let cookies;
  let bookingId;
  let roomId = 1;

  test.beforeAll(async () => {
    cookies = await auth("admin", "password");
  });

  test.beforeEach(async () => {
    let futureBooking = await createFutureBooking(roomId);
    bookingId = futureBooking.bookingid;
  });

  test("DELETE booking with specific room id:", async ({ request }) => {
    const response = await request.delete(`booking/${bookingId}`, {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(202);

    const body = await response.text();
    expect(body).toBe("");

    const getBooking = await getBookingSummary(bookingId);
    expect(getBooking.bookings.length).toBe(0);
  });

  test("DELETE booking with an id that doesn't exist", async ({ request }) => {
    const response = await request.delete("booking/999999", {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(404);

    const body = await response.text();
    expect(body).toBe("");
  });

  test("DELETE booking id without authentication", async ({ request }) => {
    const response = await request.delete(`booking/${bookingId}`);

    expect(response.status()).toBe(403);

    const body = await response.text();
    expect(body).toBe("");
  });
});
```

No primeiro teste, você pode ver que primeiro criamos uma reserva no bloco `beforeEach`, usamos o `bookingId` que é passado para o método `delete` junto com os headers, pois precisamos de autorização para isso. Este é um padrão que eu sempre uso: qualquer endpoint contra o qual planejo fazer as principais asserções sempre terá a variável `response` atribuída a ele. Isso nos dá vantagens à medida que continuamos a desenvolver nossos testes no futuro.

```typescript
const response = await request.delete(`booking/${bookingId}`, {
  headers: { cookie: cookies },
});
```

Para este primeiro teste, faço 2 asserções: uma no `response.status()` e outra esperando que o `response.text()` seja uma string vazia "". Quis validar mais detalhadamente se a reserva foi realmente deletada, então, em vez de usar o método de requisição do Playwright no teste, criei uma outra data factory `getBookingSummary(bookingId)`, que retorna o corpo do `GET booking/summary?roomid=${bookingId}` como algo contra o qual posso fazer uma asserção. Nesse caso, quero garantir que `bookings.length` seja 0.

Verificações de requisições PUT
Nesta próxima seção, deixei as coisas um pouco verbosas, pois estou criando um novo `putBody` dentro de cada seção de teste. Existem várias maneiras de organizar o código. Eu poderia ter abstraído isso de maneira semelhante à forma como crio um corpo de requisição no endpoint de criação de nova reserva (POST). Nesse caso, queria deixar bem claro quais informações estavam sendo passadas, já que em um dos meus testes tento fazer uma requisição PUT sem o primeiro nome presente.

Um padrão que estou seguindo é criar todas as minhas variáveis dentro do bloco `describe`. Isso me permite ter acesso às variáveis dentro do teste e dos `test.steps` para asserções. Atualmente, estou hardcoding algumas das variáveis também. Eu poderia usar uma ferramenta como o `faker` para tornar os dados únicos, e, se fizesse isso, definiria a variável dentro do `beforeEach()` para que cada teste receba um novo nome. Ficaria assim:

```typescript
test.describe("booking/{id} PUT requests", async () => {
  let firstname;

  test.beforeEach(async ({ request }) => {
    firstname = faker.name.firstName();
    ...
  });
```

Os testes que estou implementando são:

- PUT booking com um `room id` específico + Verificar se a reserva foi atualizada
- PUT booking sem o `firstname` no corpo da requisição
- PUT booking com um `id` que não existe
- PUT booking com um `id` que é texto
- PUT booking com um `id` com autenticação inválida
- PUT booking com um `id` sem autenticação
- PUT booking com um `id` sem corpo da requisição

Na minha opinião, esse é um dos endpoints mais interessantes, pois há muitas combinações diferentes de coisas que você pode testar.

Um ponto específico que gostaria de destacar é que, no primeiro teste "PUT booking com um room id específico", utilizo o método `test.step()` para ajudar a dividir o teste. No meu `test.step`, estou realmente usando o `getBookingById()`, um método de data factory que criei para retornar o corpo atual para o Booking ID enviado. 💡 IMPORTANTE: se você usar `test.step`, certifique-se de colocar `await` na frente do `test.step`. Se você esquecer disso, como eu fiz ao escrever meus testes pela primeira vez, vai acabar se frustrando tentando entender o que está acontecendo.

Ao escrever a automação para esses testes pela primeira vez, me deparei com muitas mensagens de erro 409 da aplicação em teste. Esse código indica um conflito. O conflito específico estava relacionado às datas de check-in e check-out que já estavam em uso, o que me levou a criar métodos de data factory que facilitaram a escrita desses testes. Sem essas funções, eu teria gasto muito tempo atualizando manualmente as datas ao solucionar problemas, e teríamos dados hardcoded que eventualmente causariam mais erros 409 e inconsistências nos nossos testes.

Esses métodos de data factory me permitiram fazer com que o teste criasse os dados de que precisa de forma independente de qualquer outro dado ou teste.

**Você não deve depender do Teste 1 para configurar os dados do Teste 2. Isso é uma armadilha!!**

Aqui está um exemplo de como estou usando o `test.step` para verificar se a reserva foi atualizada:

```typescript
await test.step("Verify booking was updated", async () => {
  const getBookingBody = await getBookingById(bookingId);
  expect(getBookingBody.bookingid).toBeGreaterThan(1);
  expect(getBookingBody.bookingid).toBe(bookingId);
  expect(getBookingBody.roomid).toBe(putBody.roomid);
  expect(getBookingBody.firstname).toBe(putBody.firstname);
  expect(getBookingBody.lastname).toBe(putBody.lastname);
  expect(getBookingBody.depositpaid).toBe(putBody.depositpaid);

  const getBookingDates = getBookingBody.bookingdates;
  expect(getBookingDates.checkin).toBe(putBody.bookingdates.checkin);
  expect(getBookingDates.checkout).toBe(putBody.bookingdates.checkout);
});
```

Ao estruturar os testes dessa forma, você assegura que cada teste é independente e pode ser executado sem depender de nenhum estado compartilhado ou configuração criada por outro teste. Isso é essencial para garantir a consistência e a confiabilidade de suas automações de testes.

Estou particularmente orgulhoso de `futureOpenCheckinDate()` e `createFutureBooking()`. Incluí a descrição do JSDoc que criei para cada uma delas abaixo. (Acabei de aprender sobre JSDoc e estou adorando!!!)

Todos os testes de requisição PUT podem ser encontrados abaixo.

```typescript
import { test, expect } from "@playwright/test";
import { auth } from "../../lib/datafactory/auth";
import {
  getBookingById,
  futureOpenCheckinDate,
  createFutureBooking,
} from "../../lib/datafactory/booking";
import { isValidDate, stringDateByDays } from "../../lib/helpers/date";

test.describe("booking/{id} PUT requests", async () => {
  let cookies;
  let bookingId;
  let roomId = 1;
  let firstname = "Happy";
  let lastname = "McPathy";
  let depositpaid = false;
  let email = "testy@mcpathyson.com";
  let phone = "5555555555555";
  let futureBooking;
  let futureCheckinDate;

  test.beforeAll(async () => {
    cookies = await auth("admin", "password");
  });

  test.beforeEach(async ({ request }) => {
    futureBooking = await createFutureBooking(roomId);
    bookingId = futureBooking.bookingid;
    futureCheckinDate = await futureOpenCheckinDate(roomId);
  });

  test(`PUT booking com ID de quarto específico`, async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      firstname: firstname,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };
    const response = await request.put(`booking/${bookingId}`, {
      headers: { cookie: cookies },
      data: putBody,
    });

    expect(response.status()).toBe(200);

    const body = await response.json();
    expect(body.bookingid).toBeGreaterThan(1);

    const booking = body.booking;
    expect(booking.bookingid).toBe(bookingId);
    expect(booking.roomid).toBe(putBody.roomid);
    expect(booking.firstname).toBe(putBody.firstname);
    expect(booking.lastname).toBe(putBody.lastname);
    expect(booking.depositpaid).toBe(putBody.depositpaid);

    const bookingdates = booking.bookingdates;
    expect(bookingdates.checkin).toBe(putBody.bookingdates.checkin);
    expect(bookingdates.checkout).toBe(putBody.bookingdates.checkout);

    await test.step("Verificar se a reserva foi atualizada", async () => {
      const getBookingBody = await getBookingById(bookingId);
      expect(getBookingBody.bookingid).toBeGreaterThan(1);
      expect(getBookingBody.bookingid).toBe(bookingId);
      expect(getBookingBody.roomid).toBe(putBody.roomid);
      expect(getBookingBody.firstname).toBe(putBody.firstname);
      expect(getBookingBody.lastname).toBe(putBody.lastname);
      expect(getBookingBody.depositpaid).toBe(putBody.depositpaid);

      const getBookingDates = getBookingBody.bookingdates;
      expect(getBookingDates.checkin).toBe(putBody.bookingdates.checkin);
      expect(getBookingDates.checkout).toBe(putBody.bookingdates.checkout);
    });
  });

  test("PUT booking sem firstname em putBody", async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };
    const response = await request.put(`booking/${bookingId}`, {
      headers: { cookie: cookies },
      data: putBody,
    });

    expect(response.status()).toBe(400);

    const body = await response.json();
    expect(body.error).toBe("BAD_REQUEST");
    expect(body.errorCode).toBe(400);
    expect(body.errorMessage).toContain(
      "Validation failed for argument [0] in public org.springframework.http.ResponseEntity"
    );
    expect(body.fieldErrors[0]).toBe("Firstname should not be blank");
  });

  test("PUT booking com um id que não existe", async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      firstname: firstname,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };

    const response = await request.delete("booking/999999", {
      headers: { cookie: cookies },
      data: putBody,
    });

    expect(response.status()).toBe(404);

    const body = await response.text();
    expect(body).toBe("");
  });

  test(`PUT booking id que é texto`, async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      firstname: firstname,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };

    const response = await request.put(`booking/asdf`, {
      headers: { cookie: cookies },
      data: putBody,
    });

    expect(response.status()).toBe(404);

    const body = await response.json();
    expect(isValidDate(body.timestamp)).toBe(true);
    expect(body.status).toBe(404);
    expect(body.error).toBe("Not Found");
    expect(body.path).toBe("/booking/asdf");
  });

  test("PUT booking id com autenticação inválida", async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      firstname: firstname,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };

    const response = await request.put(`booking/${bookingId}`, {
      headers: { cookie: "test" },
      data: putBody,
    });

    expect(response.status()).toBe(403);

    const body = await response.text();
    expect(body).toBe("");
  });

  test("PUT booking id sem autenticação", async ({ request }) => {
    let putBody = {
      bookingid: bookingId,
      roomid: roomId,
      firstname: firstname,
      lastname: lastname,
      depositpaid: depositpaid,
      email: email,
      phone: phone,
      bookingdates: {
        checkin: stringDateByDays(futureCheckinDate, 0),
        checkout: stringDateByDays(futureCheckinDate, 1),
      },
    };

    const response = await request.put(`booking/${bookingId}`, {
      data: putBody,
    });

    expect(response.status()).toBe(403);

    const body = await response.text();
    expect(body).toBe("");
  });

  test("PUT booking id sem corpo put", async ({ request }) => {
    const response = await request.put(`booking/${bookingId}`, {
      headers: { cookie: cookies },
    });

    expect(response.status()).toBe(400);

    const body = await response.json();
    expect(isValidDate(body.timestamp)).toBe(true);
    expect(body.status).toBe(400);
    expect(body.error).toBe("Bad Request");
    expect(body.path).toBe(`/booking/${bookingId}`);
  });
});
```

Como sempre, há mais validações e verificações que podemos adicionar, mas por enquanto minha confiança está elevada, e sinto que se houver alguma alteração disruptiva introduzida nos endpoints de booking, nossa automação deve nos alertar para que possamos investigar e explorar como o sistema maior será afetado.

No geral, tivemos 19 verificações que rodaram na minha máquina local em 15,9 segundos usando 4 workers.
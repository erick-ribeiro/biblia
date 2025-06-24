## Guia Definitivo para Automação de Testes de API com Playwright: Parte 1 - Conceitos Básicos de Testes de API com Requisição GET Com e Sem Autorização

É hora de começar a construir seus testes de API com Playwright. A primeira coisa que vamos precisar é de um site para testar. Eu tenho uma lista incrível de sites que mantenho atualizada.

https://github.com/BMayhew/awesome-sites-to-test-on?ref=playwrightsolutions.com

Desta lista, vou escolher um site que sei que possui um Frontend e um Backend que continuarão disponíveis no futuro próximo: https://automationintesting.online/

Este site é um complemento para o workshop Automation in Testing, criado por Mark Winteringham e Richard Bradshaw, além de ser o sistema em destaque para testes no livro Testing Web APIs (ótimo livro que recomendo muito). O livro apresenta exemplos de automação de API usando Java, e eu vou construir os mesmos exemplos, além de outros adicionais apresentados no livro, usando Playwright.

### Endpoints de Reserva
Vamos focar primeiro nos endpoints de reserva para a plataforma restful booker. Felizmente, há uma página Swagger disponível (https://automationintesting.online/booking/swagger-ui/index.html) que nos permite visualizar quais endpoints estão disponíveis para trabalhar.

Antes de começar a escrever código, eu sempre começo explorando os endpoints usando uma ferramenta como o Postman ou, desta vez, estou testando o Thunder Client, uma extensão que posso usar diretamente no VS Code.

A primeira coisa que notei é que alguns endpoints requerem um token.

- https://automationintesting.online/booking/summary?roomid=1 (não precisa de token)
- https://automationintesting.online/booking/ (precisa de token)

Depois de explorar um pouco, descobri que existe um endpoint de autenticação com uma página Swagger (https://automationintesting.online/auth/swagger-ui/index.html#/) que permite gerar um token. Posso passar esse token na chamada para o endpoint /booking/ como um header HTTP | cookie: token={token-goes-here}. Existem várias maneiras diferentes de APIs autenticarem usuários, e isso geralmente é a primeira coisa que você precisa descobrir ao criar automação de API. Um artigo interessante sobre algumas das diferentes tecnologias usadas em autenticação pode ser encontrado aqui: *Beginners Guide to HTTP Part 5 Authentication*.

### Explore o Sistema em Teste!!!
Esse passo é extremamente importante. Se você não tem um entendimento sólido de como funciona o sistema que está testando, pare e faça isso primeiro. No meu caso, eu criei uma coleção no Thunder Client com todos os endpoints. Ao fazer isso, aprendi quais endpoints requerem autenticação, quais endpoints requerem parâmetros e também descobri as limitações não documentadas do corpo JSON (ex: o número de telefone requer pelo menos 11 caracteres e espera uma string no corpo da requisição). Abaixo está uma sessão de exploração que gravei interagindo com todos os endpoints. Eu parametrizei o token como uma variável de ambiente, pois percebi que precisava atualizá-lo em cada requisição. Isso me leva a saber que definitivamente salvarei isso como uma variável na minha automação de API à medida que a suíte de testes crescer!

Uma coisa que observei enquanto explorava o aplicativo pela primeira vez e começava a pensar em como automatizar tudo, é como gerenciaremos nossos dados de teste. Uma coisa muito interessante que notei durante os testes é que, a cada 10 minutos ou mais, qualquer dado criado é apagado do banco de dados, e ele é reconfigurado com alguns dados estáticos (especificamente uma reserva de James Dean onde a data de check-in é no passado, 2022-02-01). Faremos a maioria de nossas asserções com esses dados neste tutorial, assumindo que eles estarão sempre disponíveis. Se não estivessem, precisaríamos criar dados para realizar as asserções toda vez (o que abordaremos em tutoriais futuros).

### Vamos escrever nosso primeiro teste!
Crie um diretório onde você deseja armazenar sua suíte de testes. Se esta for a sua primeira vez, vá em frente e use uma pasta vazia no seu computador. Supondo que você tenha o Node instalado, navegue até o diretório com a pasta vazia e execute o comando:

```bash
npm init playwright@latest
```

Isso irá guiá-lo através de uma série de perguntas no terminal. Minhas respostas são:

- **Typescript**
- **tests** (diretório onde os testes serão salvos)
- **n** (não precisamos de um arquivo GitHub Actions por enquanto)
- **n** (não precisamos dos navegadores, pois estamos testando a API!)

Vamos configurar o ambiente e escrever nosso primeiro teste de API com Playwright.

### Configuração do Projeto

1. Instale o pacote `dotenv`, que permitirá o uso de um arquivo `.env` na raiz do nosso projeto para variáveis de ambiente:

   ```bash
   npm install dotenv --save
   ```

2. Em seguida, apague o diretório `/tests-examples/` para manter a estrutura do projeto limpa.

### Arquivo de Configuração Playwright

Aqui está um exemplo de como configurar o arquivo `playwright.config.ts`:

```typescript
import { defineConfig, devices } from "@playwright/test";
import { config } from "dotenv";

config();

export default defineConfig({
  use: {
    baseURL: process.env.URL,
    ignoreHTTPSErrors: true,
    trace: "retain-on-failure",
  },
  retries: 0,
  reporter: [["list"], ["html"]],
});
```

Este arquivo utiliza o `dotenv` para carregar variáveis de ambiente definidas no arquivo `.env`. As opções de configuração incluem ignorar erros HTTPS, manter os traces em falhas, e definir o número de tentativas de repetição de testes para zero.

### Escrevendo o Primeiro Teste de API

Vamos modificar o arquivo `example.spec.ts` para criar a requisição GET mais simples para a API. De acordo com a documentação oficial, existem duas formas de fazer chamadas de API: usando o `request` fixture (que vamos usar) ou usando o `request context` para chamadas fora do bloco de teste.

Aqui está um exemplo de teste básico:

```typescript
import { test, expect } from "@playwright/test";

test("GET booking summary", async ({ request }) => {
  const response = await request.get(
    "https://automationintesting.online/booking/summary?roomid=1"
  );

  expect(response.status()).toBe(200);
  const body = await response.json();
  console.log(JSON.stringify(body));
});
```

### Explicação do Teste

- **Requisição GET**: Este teste faz uma chamada GET para o endpoint `/summary?roomid=1` sem autenticação.
- **Verificação de Status**: O teste faz uma asserção para garantir que o `status` da resposta seja `200`.
- **Manipulação do Corpo JSON**: Mostramos como interagir com o corpo JSON da resposta para que possamos fazer asserções adicionais no futuro.

Este teste básico armazena a resposta em uma variável `response`, que representa a classe `APIResponse`. Isso nos permite acessar o corpo da resposta como um objeto, JSON, texto, cabeçalhos, código de status, texto de status, URL, e usar o método `.ok()`, que retorna `true` se o código de status estiver entre 200-299.

Para este primeiro teste, estamos fazendo uma asserção simples no `response.status()` esperando que seja `200`. Também mostramos como interagir com o corpo JSON da resposta, pois faremos asserções mais detalhadas nele nos próximos testes.

Vamos organizar e aprimorar nossos testes de API usando Playwright com algumas melhorias e boas práticas.

### Configuração do Ambiente

1. **Crie um arquivo `.env`** na raiz do projeto com a linha abaixo para definir a URL base, conforme configurado no `playwright.config.ts`:

   ```
   URL=https://automationintesting.online/
   ```

2. **Organize o diretório de testes**:
   - Crie uma pasta `/booking/` dentro do diretório de testes.
   - Renomeie `example.spec.ts` para `booking.get.spec.ts`.

### Melhorias no Arquivo de Teste

Vamos organizar os testes usando um bloco `describe`, melhorar os nomes dos testes e adicionar asserções mais robustas. Também adicionaremos uma função auxiliar `isValidDate()` para validar se as datas retornadas são válidas.

```typescript
import { test, expect } from "@playwright/test";

// Bloco de descrição para os testes relacionados ao endpoint booking
test.describe("booking/ GET requests", async () => {
  const savedToken = "r2dBKvt8rCo5p74s"; // Valor do token salvo para autenticação

  // Teste para verificar o resumo da reserva com um ID de quarto específico
  test("GET booking summary with specific room id", async ({ request }) => {
    const response = await request.get("booking/summary?roomid=1");

    expect(response.status()).toBe(200);
    const body = await response.json();
    expect(body.bookings.length).toBeGreaterThanOrEqual(1);
    expect(isValidDate(body.bookings[0].bookingDates.checkin)).toBe(true);
    expect(isValidDate(body.bookings[0].bookingDates.checkout)).toBe(true);
  });

  // Teste para obter todas as reservas com detalhes usando autenticação
  test("GET all bookings with details", async ({ request }) => {
    const response = await request.get("booking/", {
      headers: { cookie: `token=${savedToken}` },
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

  // Teste para obter detalhes de uma reserva específica por ID usando autenticação
  test("GET booking by id with details", async ({ request }) => {
    const response = await request.get("booking/1", {
      headers: { cookie: `token=${savedToken}` },
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
});

// Função auxiliar para validar se a data é válida
export function isValidDate(date: string) {
  return !isNaN(Date.parse(date));
}
```

### Melhoria das Asserções e Observações

- Adicionamos verificações para garantir que os dados retornados são válidos e correspondem ao que esperamos.
- Descobrimos uma pequena inconsistência no formato das datas: `bookingDates` usa camel case em uma chamada e `bookingdates` é minúsculo em outra. Isso pode ser um bug que deve ser reportado aos desenvolvedores.

### Próximos Passos

Agora que temos um conjunto sólido de verificações para os cenários de happy path das três chamadas GET na API de reservas, vamos criar uma chamada de API que pode ser usada no passo `beforeAll()` do teste Playwright para salvar o cookie de autenticação.

### Autenticação com Requisição POST

Vamos criar uma requisição POST para autenticar o usuário passando o nome de usuário e senha no corpo da requisição. Podemos usar a resposta dessa chamada para salvar o token de autenticação que será utilizado nos testes subsequentes. Essa abordagem será útil para automatizar o processo de login antes de rodar nossos testes de API.

Ao utilizar o debugger do VS Code para Playwright, conseguimos entender melhor como as chamadas de API funcionam e como estruturar nossas automações de forma mais eficaz. Isso é bastante útil para inspecionar as respostas e refinar nossas asserções.

Este processo nos permite manter uma automação robusta e escalável, com menos dependências manuais e maior controle sobre os testes realizados.


# Guia Definitivo para Automação de Testes de API com Playwright: Introdução

Algumas pessoas me perguntaram se é possível fazer testes de API com Playwright, a resposta curta é SIM! Com esta nova série de posts, vou explorar todos os detalhes de como utilizar o Playwright para todas as suas necessidades de Testes de API. Este será o primeiro artigo de uma série de muitos. Planejo manter esta página e os posts seguintes atualizados com as informações mais recentes.

Primeiro de tudo, você deve estar se perguntando por que alguém iria querer construir testes de API em uma ferramenta como o Playwright, que foi projetada para testes de UI. Bem, no final de 2021, eu fiz uma breve prova de conceito para nossos testes de UI end-to-end e selecionei o Playwright como a ferramenta escolhida. Após essa decisão, levei mais uma semana para fazer uma prova de conceito com diferentes ferramentas para selecionar uma para as minhas necessidades de testes de API. A aplicação que nossa equipe estava desenvolvendo foi construída com uma abordagem API-first para que os desenvolvedores pudessem se integrar à nossa plataforma.

### Meus Critérios

- Os testes devem ser escritos em JavaScript ou TypeScript  
  Escolhi isso como critério, pois todo o nosso código de produção foi escrito em TypeScript, e eu queria que nossa equipe de desenvolvimento pudesse criar e atualizar os testes facilmente, caso a lógica de negócios mudasse.

- Eu queria ser capaz de fazer chamadas de API de qualquer lugar e a qualquer momento, especificamente para criar dados de teste conforme necessário.
- Eu queria a capacidade de obter detalhes do corpo da resposta dentro de uma execução de teste e salvar valores em variáveis.
- Eu queria que, se uma verificação automatizada falhasse, fosse possível ver os cabeçalhos de resposta e o corpo da resposta da solicitação para facilitar a depuração.
- Eu queria ter controle total sobre a URL completa ao fazer chamadas de API (tenho cenários onde posso precisar usar um baseURL diferente no meio de um teste).

Eu avaliei Frisby.js, PactumJS, SuperTest e Playwright. Além disso, também avaliei o Postman, que estávamos usando na época para um conjunto de testes smoke que poderiam ser executados após uma build. Para o nível de profundidade que eu queria alcançar, decidi que o Postman, embora uma excelente ferramenta para testes exploratórios, seria um esforço muito grande para gerenciar uma suíte completa de regressão de testes.

Ao longo da semana de testes e aprendizado, achei todas essas ferramentas fáceis de começar a usar, mas através do processo de prova de conceito e tomada de decisão, o Playwright foi o vencedor para mim. Uma coisa que o Playwright faz muito bem é permitir que você use programação orientada a objetos dentro dos seus testes de API. Eu sou capaz de atribuir uma variável para os métodos GET, PUT, POST, DELETE, e então usar essa variável para obter informações sobre a resposta que o servidor retornou. Isso me permitiu fazer de 5 a 30 asserções em uma resposta de API sem uma quantidade excessiva de encadeamentos. Outro ponto positivo foi a redução da carga cognitiva sobre os colaboradores. Ter que aprender apenas um framework para automação de UI e API significa um tempo de adaptação mais rápido para novos colaboradores.

### Refatorando e Melhorando o Gerenciamento de Cookies nos Testes de API

Agora que estamos capturando os cookies diretamente dos cabeçalhos da resposta durante a autenticação, podemos melhorar a maneira como gerenciamos esses cookies em nossos testes. Vamos revisar a implementação passo a passo.

### Código Atualizado para Captura de Cookies

Modificamos o código para capturar os cookies no bloco `beforeAll`, garantindo que todas as requisições subsequentes utilizem esses cookies corretamente:

```typescript
import { test, expect } from "@playwright/test";

test.describe("booking/ GET requests", async () => {
  let cookies = "";

  // Autenticação e captura de cookies antes de todos os testes
  test.beforeAll(async ({ request }) => {
    const response = await request.post("auth/login", {
      data: {
        username: "admin",
        password: "password",
      },
    });
    expect(response.status()).toBe(200);
    const headers = await response.headers();
    cookies = headers["set-cookie"];
  });

  // Teste para obter resumo de reserva com ID de quarto específico
  test("GET booking summary with specific room id", async ({ request }) => {
    const response = await request.get("booking/summary?roomid=1");

    expect(response.status()).toBe(200);
    const body = await response.json();
    expect(body.bookings.length).toBeGreaterThanOrEqual(1);
    expect(isValidDate(body.bookings[0].bookingDates.checkin)).toBe(true);
    expect(isValidDate(body.bookings[0].bookingDates.checkout)).toBe(true);
  });

  // Teste para obter todas as reservas com detalhes usando os cookies de autenticação
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

  // Teste para obter uma reserva específica por ID usando os cookies de autenticação
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
});

// Função auxiliar para validar se a data é válida
export function isValidDate(date: string) {
  return !isNaN(Date.parse(date));
}
```

### Alterações Realizadas

- **Armazenamento de Cookies**: Movemos a captura dos cookies para uma variável `cookies` que é definida no bloco `beforeAll`, permitindo que essa variável seja reutilizada em todos os testes.
- **Melhoria na Flexibilidade**: Alteramos a requisição para passar todo o cookie ao invés de apenas o token específico, o que é uma prática mais robusta.
- **Uso de `let`**: A variável `cookies` foi declarada com `let` para que possa ser atualizada facilmente dentro do bloco `beforeAll`.

### Execução de Testes Repetidos

Para garantir que nosso código não apresente falhas intermitentes (flakey tests), executamos os testes repetidamente usando o comando:

```bash
npx playwright test --repeat-each=10
```

Isso executa cada teste 10 vezes para verificar a estabilidade, e todos os testes passaram com sucesso! 🎉

### Conclusão

Agora temos uma automação robusta que utiliza autenticação com cookies, organizando nossos testes de uma maneira que seja reutilizável e fácil de manter. Esse setup melhora a confiabilidade dos testes e facilita a adaptação para novos cenários de testes conforme a suíte cresce.
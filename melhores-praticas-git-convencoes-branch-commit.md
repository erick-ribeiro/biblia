# Melhores Práticas no Git: Convenções de Branch e Commit

## Introdução

Ao lidar com a criação de nomes de branch e mensagens de commit no Git, é comum que os a gente se sinta indeciso sobre o que inserir.

> Quando comecei a trabalhar com Git, sentia muita incerteza ao escrever uma mensagem de commit, ficava muito tempo pensando em como resumir adequadamente as mudanças que havia feito e por que as fiz. Elaborei este guia descrevendo o caminho que trilhei para aprimorar minhas mensagens de commit.

Este guia pressupõe que você já compreende o fluxo de trabalho básico do Git. Caso contrário, sugiro que leia o guia "Aprenda conceitos de git, não comandos" (https://github.com/PauloGoncalvesBH/treinamento-git).

Também é importante observar que devemos seguir, antes de tudo, as convenções de nossa equipe. Essas dicas são baseadas em sugestões provenientes de pesquisas e do consenso geral da comunidade. Porém, ao final deste artigo, você pode ter algumas implementações a sugerir que podem te ajudar no fluxo de trabalho da sua equipe.


## Por que dar tanta atenção à mensagem de commit?

Desafio você a abrir um projeto pessoal ou qualquer repositório, acessar o histórico de commits e ver aquela lista de mensagens de commit antigas. A grande maioria de nós que já passou por tutoriais ou fez correções rápidas dirá: "Sim... Eu realmente não faço ideia do que quis dizer com 'Refatoração do codigo, melhoria no desempenho' 4 meses atrás."

Uma mensagem de commit não é apenas um "rótulo" que colocamos nas mudanças que fizemos, mas sim uma peça essencial da documentação do projet que nos ajuda a teum histórico claro e compreensível das mudanças e facilita a revisão do código auxiliando no rastreamento de problemas.

## Como fazer um commit 🆙

Há basicamente duas formas de fazer um commit: a simples e a detalhada. Vamos explorar ambas:

**Básica**:
```bash
git commit -m "alguma mensagem aqui"
```

**Detalhada**:
```bash
git commit -m "título do commit" -m "texto contendo a descrição do commit"
```

Neste guia, focaremos na forma detalhada. Utilizando-a, podemos descrever com mais precisão a intenção daquele commit.


## Padrões de Commits

Quando comecei minha busca por um padrão que pudesse me auxiliar, deparei-me com o padrão gitmoji (https://gitmoji.dev/)  onde cada emoji tem um significado específico, como 🐛 para correções de bugs e ✨ para novas funcionalidades.

Adotei a prática de incluir emojis nos meus commits e comecei a detalhá-los mais, utilizando o padrão de commit detalhado, no qual, além de um título, incluímos também uma descrição.

No entanto, esse padrão trouxe um novo desafio: nem todos estão familiarizados com ele, e os emojis podem gerar diferentes interpretações dependendo do leitor.

Alguns outros padrões de commits:

1. **Conventional Commits**:

É um padrão que busca estabelecer uma convenção para adicionar clareza às mensagens de commit, tornando-as mais legíveis. (https://www.conventionalcommits.org/)

```bash
<type>[optional scope]: <description>
[optional body]
[optional footer(s)]
```

2. **Angular Commit Guidelines**:

Desenvolvido e utilizado pela equipe do Angular, este padrão influenciou muitos outros, incluindo o Conventional Commits. (https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)

```bash
<type>(<scope>): <short summary>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

3. **Emoji-log**:

Semelhante ao Gitmoji, mas com uma abordagem mais simples e com um conjunto mais restrito de emojis. (https://github.com/ahmadawais/Emoji-Log)

```bash
<emoji type>: IMPERATIVE_MESSAGE_GOES_HERE
```

Estes são apenas alguns dos padrões de commit mais populares e reconhecidos, mas há muitos outros por aí, e novos padrões ou variações surgem frequentemente à medida que as equipes buscam maneiras de melhorar a clareza e a eficácia das mensagens de commit. Independentemente do padrão escolhido, a chave é manter a consistência e garantir que ele seja compreendido por todos os membros da equipe ou contribuidores do projeto.

Claro! A seguir, um esboço de seção que aborda a importância de escrever commits melhores, além de adotar padrões:


## Além dos Padrões: Como Escrever Commits Melhores

Ter um padrão de commits é essencial, mas tão importante quanto é a qualidade da escrita. Aqui vão algumas dicas para aprimorar a redação de suas mensagens de commit:

1. **Seja Claro e Direto**: Evite ambiguidades. O objetivo é que qualquer pessoa que leia seu commit entenda imediatamente o que foi feito e por quê. Sempre prefira utilizar o modo imperativo no assunto. (Exemplo: "Corrige bug no carregamento da página principal")


2. **Contextualize**: Se a mudança foi feita em resposta a um bug, uma solicitação de recurso ou um feedback, mencione isso. Isso ajuda a entender o contexto por trás da decisão. É recomendado e pode ser ainda mais benéfico ter um conjunto consistente de palavras para descrever suas mudanças. Exemplo: Correção, Atualização, Refatoração, Atualização, etc.

3. **Mantenha-se Focado**: Cada commit deve representar uma unidade lógica de mudança. Se você está fazendo várias mudanças não relacionadas, é melhor dividi-las em commits separados.

4. **Use o Corpo do Commit**: O título do commit deve ser curto e direto (não deve ter mais do que 50 caracteres), mas se precisar explicar algo em detalhes, use o corpo da mensagem de commit (que deve ser limitado a 72 caracteres, se precissar de mais quebre a linha). O Corpo do commmit é o lugar perfeito para contextualizar, explicar o raciocínio por trás das decisões tomadas ou listar mudanças mais detalhadas.

5. **Referencie Issues ou Tarefas Relacionadas**: Se sua mudança estiver relacionada a uma issue, história de usuário ou qualquer outra tarefa de rastreamento, mencione-a no commit. Isso facilita a correlação entre a mudança e a tarefa original.

6. **Revise Antes de Enviar**: Assim como você revisa o código antes de um pull request, leia sua mensagem de commit. Garanta que ela comunica efetivamente o que você deseja.

Lembre-se, o objetivo final de uma boa mensagem de commit é proporcionar clareza e entendimento. Em um projeto, especialmente em equipes grandes ou distribuídas, uma comunicação clara é a chave para a colaboração eficaz e para a manutenção do código a longo prazo.

## Um Exemplo de Commit

### Bom Exemplo:

```
Corrige API para habilitar cálculo X

Este commit resolve o comportamento defeituoso do componente ao fazer xyz. 
Antes desta correção, a API não estava habilitando o cálculo X, resultando em 
respostas incorretas. Com esta atualização, os usuários agora receberão um 
valor corrigido na resposta da API.

JIRA-123
```

Este é um exemplo de uma mensagem de commit bem elaborada. Ela:

- Começa com uma breve descrição concisa do que foi feito.
- Fornece detalhes sobre o problema e como ele foi resolvido.
- Referencia uma tarefa ou ticket associado para mais contexto.

### O que não fazer:

1. **Mensagens Vagas**:
   - **Ruim**: "Correções na API"
   - **Por quê?** Não indica o que foi corrigido especificamente.
   
2. **Falta de Contexto**:
   - **Ruim**: "Mudanças feitas"
   - **Por quê?** Não fornece qualquer informação sobre as razões ou o impacto das mudanças.

3. **Mensagens muito Longas sem Formatação**:
   - **Ruim**: "Corrigi a API que estava retornando valores errados quando tentávamos calcular X e também fiz algumas outras correções menores e atualizei a documentação e também ..."
   - **Por quê?** É difícil de ler e não separa claramente os diferentes pontos ou tarefas abordados.

4. **Sem Referência a Tarefas/Tickets**:
   - **Ruim**: "Corrigido bug na API"
   - **Por quê?** Não há referência a um ticket ou tarefa, o que pode dificultar o rastreamento ou a revisão futura.

Ao elaborar mensagens de commit, é vital ser claro, conciso e fornecer o máximo de contexto possível para ajudar a equipe a entender o que foi feito e por quê.

## Conclusão

O ato de commitar no Git vai além de simplesmente salvar mudanças em um repositório. Ele desempenha um papel crucial na comunicação e colaboração dentro de uma equipe. Adotar boas práticas de mensagens de commit é um investimento em clareza, eficiência e produtividade para o futuro. Lembre-se de que, quando você escreve uma mensagem de commit, não está apenas documentando para si mesmo, mas para todos os membros da equipe, contribuidores futuros e até para você mesmo em um momento futuro.

Os padrões e exemplos que discutimos aqui são guias que podem ajudar a moldar uma cultura de clareza e responsabilidade em seus projetos. Ao seguir essas práticas recomendadas, você estará garantindo que o histórico do projeto seja não apenas uma lista de mudanças, mas um registro detalhado e compreensível da evolução do projeto.

Desejo a você sucesso em suas jornadas de desenvolvimento e espero que estas dicas contribuam para projetos mais organizados e equipes mais alinhadas!


adiciona teste automatizado para o login

neste commit, foi criado um novo teste automatizado para a
funcionalidade de login. O teste assegura que, ao usar credenciais 
corretas, o usuário é levado para a página principal sem erros.

jira-123


---

Summary:
- https://www.conventionalcommits.org/en/v1.0.0/
- https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit
- https://github.com/ahmadawais/Emoji-Log
- https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages/
- https://gist.github.com/luismts/495d982e8c5b1a0ced4a57cf3d93cf60
- https://yashb2023.medium.com/best-git-skills-branch-commit-conventions-guide-a0d59347942a
- https://github.com/cypress-io/cypress/blob/develop/CONTRIBUTING.md#committing-code
- https://www.selenium.dev/documentation/about/contributing/
- https://github.com/microsoft/playwright/blob/main/CONTRIBUTING.md#commit-messages
- https://edgeguides.rubyonrails.org/contributing_to_ruby_on_rails.html#contributing-to-the-rails-documentation
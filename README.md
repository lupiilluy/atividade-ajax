# Atividade AJAX — Linguagem de Programação para Web I

**Grupo:** Clara Gentil, Carol Gentil, Ana Luiza Martins, Maria Jeovana, Welyson Freitas  
**Disciplina:** Linguagem de Programação para Web I — 2º Semestre

---

## O que é AJAX?

AJAX (Asynchronous JavaScript and XML) é uma técnica que permite fazer requisições ao servidor sem precisar recarregar a página inteira. Com isso, a página fica mais rápida e a experiência do usuário melhora bastante.

---

## Métodos que estudamos

### XMLHttpRequest
A forma mais antiga de fazer AJAX. Funciona, mas o código fica bem verboso e difícil de ler, especialmente quando você precisa encadear várias requisições.

### Fetch API
Chegou no ES6 para substituir o XHR. Usa Promises, o que deixa o código bem mais limpo. Um ponto de atenção: ele não lança erro automaticamente em respostas 404 ou 500, então é necessário verificar o `response.ok` manualmente.

### Promises
Não é um método de requisição por si só, mas o mecanismo por baixo do Fetch e do async/await. Permite encadear operações com `.then()` e tratar erros com `.catch()`, evitando o famoso "callback hell" do XHR.

### Async/Await
A forma mais moderna e mais fácil de ler. O código parece síncrono mesmo sendo assíncrono. O tratamento de erros usa `try/catch`, que já é familiar pra qualquer um que programa.

---

## Comparação rápida

| | XHR | Fetch | Async/Await |
|---|---|---|---|
| Legibilidade | Ruim | Boa | Ótima |
| Tratamento de erro | Complicado | `.catch()` | `try/catch` |
| Suporte antigo | ✅ | ✅ | ⚠️ |

---

## Arquivos do projeto

- `ajax3.html` — código original do professor (XMLHttpRequest)
- `ajax3-fetch.html` — reescrito usando Fetch API
- `ajax3-promises.html` — reescrito usando Promises
- `ajax3-async.html` — reescrito usando Async/Await

Todos consultam a API do [ViaCEP](https://viacep.com.br) e preenchem os campos de endereço automaticamente.

---

## Desempenho

Na prática, os quatro métodos têm desempenho muito parecido — o tempo de resposta depende muito mais da internet e do servidor do que da forma como o JavaScript faz a requisição. A diferença real está na **facilidade de escrever e manter o código**. Por isso, hoje em dia o async/await é o mais recomendado para projetos novos.

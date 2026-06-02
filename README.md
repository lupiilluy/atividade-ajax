# 📡 Atividade AJAX — Linguagem de Programação para Web I

> **Disciplina:** Linguagem de Programação para Web I — 2º Semestre  
> **Professor:** Prof. Araya  
> **Grupo:** Clara Gentil · Carol Gentil · Ana Luiza Martins · Maria Jeovana · Welyson Freitas

---

## 📋 Índice

- [Pesquisa Comparativa](#-pesquisa-comparativa)
  - [XMLHttpRequest](#1-xmlhttprequest)
  - [Fetch API](#2-fetch-api)
  - [Promises](#3-promises)
  - [Async/Await](#4-asyncawait)
  - [Tabela Comparativa](#-tabela-comparativa)
- [Programas Implementados](#-programas-implementados)
- [Comparação de Desempenho e Facilidade de Uso](#-comparação-de-desempenho-e-facilidade-de-uso)
- [Conclusão](#-conclusão)

---

## 🔬 Pesquisa Comparativa

AJAX (*Asynchronous JavaScript and XML*) é uma técnica que permite que páginas web se comuniquem com servidores em segundo plano, sem recarregar a página inteira. Ao longo dos anos, a forma de implementar AJAX em JavaScript evoluiu bastante. Nesta pesquisa, comparamos as quatro abordagens principais.

---

### 1. XMLHttpRequest

O **XMLHttpRequest (XHR)** é a forma mais antiga de realizar requisições assíncronas no navegador. Foi introduzido pelo Internet Explorer 5 em 1999 e padronizado pelo W3C posteriormente.

**Como funciona:**

O XHR funciona por meio de um objeto que passa por diferentes estados (`readyState`) durante o ciclo de vida da requisição. O desenvolvedor define um callback na propriedade `onreadystatechange` que é chamado cada vez que o estado muda.

```javascript
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
        var dados = JSON.parse(this.responseText);
        // usa os dados aqui
    }
};
xhttp.open("GET", "https://viacep.com.br/ws/01001000/json", true);
xhttp.send();
```

**Características:**
- ✅ Suporte universal em todos os navegadores, inclusive muito antigos
- ✅ Permite acompanhar o progresso do upload/download (`onprogress`)
- ✅ Suporta cancelamento de requisições
- ❌ Sintaxe verbosa e difícil de ler
- ❌ Tratamento de erros complexo
- ❌ Facilmente cai no "callback hell" em requisições encadeadas

---

### 2. Fetch API

A **Fetch API** foi introduzida como padrão moderno em 2015 (ES6) para substituir o XHR. Ela é baseada em **Promises**, o que torna o código mais limpo e legível.

**Como funciona:**

`fetch()` retorna uma Promise que resolve com um objeto `Response`. É necessário chamar `.json()` (ou `.text()`) para extrair o corpo da resposta, o que retorna outra Promise.

```javascript
fetch("https://viacep.com.br/ws/01001000/json")
    .then(response => response.json())
    .then(dados => {
        // usa os dados aqui
    })
    .catch(error => {
        console.error("Erro:", error);
    });
```

**Características:**
- ✅ Sintaxe mais limpa e moderna
- ✅ Baseada em Promises (encadeável com `.then()`)
- ✅ Nativa no navegador, sem necessidade de bibliotecas externas
- ✅ Suporta `Request`, `Response` e `Headers` como objetos reutilizáveis
- ❌ Não rejeita a Promise em erros HTTP (como 404 ou 500) — precisa verificar `response.ok` manualmente
- ❌ Não suporta progresso de upload nativamente
- ❌ Não suporta cancelamento sem `AbortController`

---

### 3. Promises

**Promises** não são um método de requisição por si só, mas sim o mecanismo que sustenta tanto o `fetch` quanto o `async/await`. Uma Promise representa um valor que pode estar disponível agora, no futuro ou nunca.

Uma Promise possui três estados:
- **Pending:** aguardando resolução
- **Fulfilled:** operação concluída com sucesso
- **Rejected:** operação falhou

```javascript
function buscarCep(cep) {
    return new Promise((resolve, reject) => {
        var xhttp = new XMLHttpRequest();
        xhttp.onreadystatechange = function () {
            if (this.readyState == 4) {
                if (this.status == 200) {
                    resolve(JSON.parse(this.responseText));
                } else {
                    reject(new Error("Erro na requisição: " + this.status));
                }
            }
        };
        xhttp.open("GET", "https://viacep.com.br/ws/" + cep + "/json", true);
        xhttp.send();
    });
}

buscarCep("01001000")
    .then(dados => console.log(dados))
    .catch(erro => console.error(erro));
```

**Características:**
- ✅ Elimina o "callback hell" com encadeamento `.then()`
- ✅ Tratamento de erros centralizado com `.catch()`
- ✅ Permite execução paralela com `Promise.all()`
- ❌ Encadeamentos longos ainda podem ficar difíceis de ler
- ❌ Mais verboso do que `async/await`

---

### 4. Async/Await

O **async/await** foi introduzido no ES2017 (ES8) e é a forma mais moderna e legível de trabalhar com código assíncrono. Por baixo dos panos, ele é apenas açúcar sintático sobre Promises.

**Como funciona:**

Uma função marcada com `async` sempre retorna uma Promise. Dentro dela, a palavra-chave `await` pausa a execução até que a Promise seja resolvida, tornando o código assíncrono parecido com código síncrono.

```javascript
async function buscarCep(cep) {
    try {
        const response = await fetch("https://viacep.com.br/ws/" + cep + "/json");
        if (!response.ok) throw new Error("CEP não encontrado");
        const dados = await response.json();
        return dados;
    } catch (error) {
        console.error("Erro:", error);
    }
}
```

**Características:**
- ✅ Sintaxe mais clara e próxima de código síncrono
- ✅ Tratamento de erros com `try/catch` (familiar para qualquer programador)
- ✅ Fácil de depurar (stack traces mais claros)
- ✅ Ideal para operações sequenciais assíncronas
- ❌ Requer transpilação para suportar navegadores muito antigos
- ❌ O `await` bloqueia a função — uso descuidado pode prejudicar desempenho

---

### 📊 Tabela Comparativa

| Critério | XMLHttpRequest | Fetch + Promises | Promises (puro) | Async/Await |
|---|---|---|---|---|
| **Introdução** | 1999 (IE5) | 2015 (ES6) | 2015 (ES6) | 2017 (ES8) |
| **Legibilidade** | ⭐☆☆☆☆ | ⭐⭐⭐☆☆ | ⭐⭐⭐☆☆ | ⭐⭐⭐⭐⭐ |
| **Facilidade de uso** | ⭐⭐☆☆☆ | ⭐⭐⭐⭐☆ | ⭐⭐⭐☆☆ | ⭐⭐⭐⭐⭐ |
| **Tratamento de erros** | Verboso | `.catch()` | `.catch()` | `try/catch` |
| **Suporte a navegadores antigos** | ✅ Total | ✅ Bom | ✅ Bom | ⚠️ Requer transpilação |
| **Cancelamento** | `abort()` nativo | `AbortController` | `AbortController` | `AbortController` |
| **Progresso** | `onprogress` ✅ | ❌ Limitado | ❌ Limitado | ❌ Limitado |
| **Verbosidade do código** | Alta | Média | Média-Alta | Baixa |
| **Indicado para iniciantes** | ❌ Não | ✅ Sim | ⚠️ Parcialmente | ✅ Sim |

---

## 💻 Programas Implementados

| Arquivo | Método utilizado | Descrição |
|---|---|---|
| [`ajax3.html`](ajax3.html) | XMLHttpRequest | Código original do professor |
| [`ajax3-fetch.html`](ajax3-fetch.html) | Fetch API | Reescrita usando `fetch().then()` |
| [`ajax3-promises.html`](ajax3-promises.html) | Promises (XHR encapsulado) | Reescrita encapsulando XHR em Promise |
| [`ajax3-async.html`](ajax3-async.html) | Async/Await | Reescrita usando `async/await` com fetch |

Todos os programas realizam a mesma tarefa: **consultar um CEP na API ViaCEP** e preencher os campos de endereço automaticamente.

---

## ⚡ Comparação de Desempenho e Facilidade de Uso

### Desempenho

Em termos de **tempo de resposta de rede**, todos os métodos têm desempenho praticamente idêntico, pois o gargalo é sempre a latência da requisição HTTP — não a forma como o JavaScript a gerencia.

A diferença de desempenho se manifesta principalmente em:

- **XMLHttpRequest:** ligeiramente mais rápido em inicialização por ser mais baixo nível, mas a diferença é imperceptível ao usuário.
- **Fetch API:** tem um overhead mínimo em comparação ao XHR por criar objetos `Request` e `Response`, mas isso é insignificante em aplicações reais.
- **Async/Await:** idêntico ao Fetch em desempenho, pois é apenas uma camada de sintaxe sobre Promises.

> 💡 **Conclusão de desempenho:** Para aplicações web comuns, **não há diferença prática de desempenho** entre os métodos. A escolha deve ser baseada em **legibilidade e manutenibilidade do código**.

### Facilidade de Uso

| Aspecto | XHR | Fetch | Promises | Async/Await |
|---|---|---|---|---|
| Curva de aprendizado | Alta | Média | Média | Baixa |
| Quantidade de código | Muito | Médio | Médio | Pouco |
| Leitura do código | Difícil | Razoável | Razoável | Fácil |
| Debug | Difícil | Médio | Médio | Fácil |
| Tratamento de erro | Complexo | Simples | Simples | Intuitivo |

---

## ✅ Conclusão

A evolução das ferramentas de AJAX em JavaScript reflete a busca por código mais **legível, manutenível e menos propenso a erros**.

- O **XMLHttpRequest** é fundamental para entender as bases do AJAX, mas seu uso direto em projetos novos não é recomendado pela verbosidade.
- O **Fetch com Promises** é um grande avanço, tornando o código mais limpo e funcional.
- O **Async/Await** é atualmente o padrão recomendado pela indústria para novos projetos, combinando o poder das Promises com uma sintaxe próxima do código síncrono tradicional.

> **Recomendação do grupo:** Para projetos novos, use sempre **async/await com fetch**. Para entender como tudo funciona por baixo, vale estudar o XMLHttpRequest.

---

*Atividade desenvolvida para a disciplina de Linguagem de Programação para Web I — 2º Semestre*

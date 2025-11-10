![](attachments/u1251110-intro%20dev%20web%202025-11-09%2021.14.50.excalidraw.svg)
%%[🖋 Edit in Excalidraw](attachments/u1251110-intro%20dev%20web%202025-11-09%2021.14.50.excalidraw.md)%%

# Fichamento (10/11/2025)

## Tópico: Introdução ao Desenvolvimento Web com Java e Javalin

---

## 1. Tecnologias Definidas para o Projeto

Os alunos deverão utilizar as seguintes tecnologias no projeto da disciplina:

- **Backend:** [Javalin](https://javalin.io) — framework web Java simples e didático.  
- **Frontend:** [Thymeleaf](https://www.thymeleaf.org) — mecanismo de template HTML integrado ao Java.  
- **Servidor Web Java:** Jetty (embutido no Javalin).  
- **Banco de Dados:** PostgreSQL.  
- **Ferramenta de Log:** Log4J.  
- **Gerenciador de dependências:** Maven.

---

## 3. Arquitetura e Fundamentos de Servidores Web

### 3.1. O papel do servidor e do cliente
- O usuário acessa o sistema via **navegador ou celular**.
- O navegador faz uma **requisição HTTP** para o **servidor**.
- O servidor responde com **HTML, imagens ou dados**.

**Protocolo usado:**  
- HTTP (porta padrão: 80)  
- HTTPS (porta padrão: 443)

**Programas que atuam como servidores web:**
- Nginx  
- Apache HTTP Server  
- Apache Tomcat  
- Jetty (usado na aula)

### 3.2. O que é um servidor
Um **servidor** é um **programa** que:
- Fica **escutando uma porta TCP** (ex: 80 ou 8000);
- Recebe **requisições** e envia **respostas**.

### 3.3. Endereços e portas
- `localhost` ou `127.0.0.1` → refere-se à própria máquina.
- Cada computador na rede tem um **IP local** (ex: `10.0.1.47`).
- Para acessar outro computador: `http://10.0.1.47:8000`
- As portas indicam **qual processo** do computador deve receber a requisição.

---

## 4. Protocolo HTTP e Verbos Principais

**HTTP = protocolo de comunicação entre cliente e servidor.**

### 4.1. Verbos HTTP
| Verbo | Ação | Descrição |
|-------|------|------------|
| **GET** | Buscar | Recupera um recurso do servidor |
| **POST** | Enviar | Envia dados ao servidor |
| **PUT** | Atualizar | Atualiza dados existentes |
| **DELETE** | Remover | Exclui um recurso |
| **PATCH** | Atualizar parcialmente | Modifica parte de um recurso existente |
| **HEAD** | Verificar cabeçalho | Recupera apenas o cabeçalho de uma resposta |
| **OPTIONS** | Descobrir métodos suportados | Retorna os métodos HTTP aceitos por um endpoint |

> Estes verbos são amplamente utilizados no contexto do **REST**. Pesquise sobre o que é **REST (Representational State Transfer)** para compreender como o HTTP é usado em sistemas baseados nesse estilo arquitetural.

### 4.2. Exemplo
```http
GET /index.html HTTP/1.1
Host: ufpb.br
```

### 4.3. Respostas HTTP
- **200** → OK  
- **404** → Página não encontrada  
- **500** → Erro interno do servidor  

> A aula destacou que **mensagens de erro não devem ser exibidas ao usuário final**, mas registradas no **log**.

---

## 5. HTML e o Papel do Cliente

- HTML é a **linguagem de marcação** usada para estruturar páginas web.
- Cada página (`index.html`, `login.html`) contém elementos (`<form>`, `<input>`, `<button>` etc.) que o servidor envia ao navegador.
- O navegador **renderiza** o HTML e exibe a página ao usuário.

> **Tarefa de casa:** ler sobre HTML básico e compreender a estrutura de uma página simples.

---

## 6. Funcionamento do Projeto Java com Javalin

### 6.1. Estrutura principal
Arquivo principal: `App.java`

```java
app.get("/login", LoginController::mostrarPaginaDeLogin);
app.post("/login", LoginController::processarLogin);
```

- `app.get` e `app.post` definem **rotas**.
- Cada rota está associada a um **método Java** em uma **classe controladora** (Controller).

### 6.2. MVC (Model-View-Controller)
- **Model:** dados e regras de negócio.  
- **View:** interface com o usuário (HTML com Thymeleaf).  
- **Controller:** classes que tratam requisições e coordenam ações.

Exemplo:
```java
public class LoginController {
    public static void mostrarPaginaDeLogin(Context ctx) {
        ctx.render("login.html");
    }
}
```

### 6.3. Conceito de rota
- “Definir rotas” = mapear uma **URL** para um **método Java**.
- Toda framework web (Java, Python, etc.) segue esse mesmo princípio, pois todos usam HTTP.

---

## 7. Logs e Tratamento de Erros

### 7.1. Conceito
O **log** registra tudo o que ocorre no servidor:
- Sucesso de autenticação  
- Tentativas de login inválidas  
- Erros internos (exceções)  

```java
logger.info("Usuário autenticado com sucesso");
logger.warn("Tentativa de login falhou");
```

### 7.2. Boas práticas
- Mensagens de erro são para **desenvolvedores**, não para usuários.  
- O usuário deve ver apenas mensagens amigáveis (“Ocorreu um erro. Tente novamente”).  
- Os detalhes técnicos devem ser salvos no log (`app.log`).

---

## 8. Ambientes de Desenvolvimento

| Ambiente | Descrição |
|-----------|------------|
| **Desenvolvimento** | onde o sistema é criado e testado localmente |
| **Teste / Homologação** | versão usada para validar novas funcionalidades |
| **Produção** | sistema real acessado pelos usuários finais |

> “Homologar é verificar se o que foi desenvolvido bate com o que o cliente pediu.”

---

## 9. Exercício Prático Realizado em Sala

1. O professor executou a aplicação base (`App.java`) com o comando “Run Main”.
2. A aplicação rodou localmente em `http://localhost:8000`.
3. Os alunos acessaram a aplicação pelo IP da máquina do professor (`10.0.1.47:8000`).
4. Foi demonstrado:
   - Login e autenticação de usuários;
   - Cadastro e remoção de usuários;
   - Páginas de erro (404 e 500) personalizadas;
   - Estrutura de templates do Thymeleaf.

---

## 10. Recomendações de Estudo

- Ler a documentação oficial do **Javalin** e do **Thymeleaf**.
- Criar um projeto **“Hello World”** do zero, usando Maven.
- Entender a estrutura padrão do Maven (`src/main/java`, `src/main/resources`).
- Estudar o funcionamento do **pom.xml**.
- Evitar copiar e colar código sem compreender — estudar cada parte.
- Fazer pequenas modificações no código para entender o comportamento.

---

## 11. Boas Práticas para a disciplina

- Trabalhar em **parceria com colegas**: ajudar e pedir ajuda.
- Aprender a lidar com **erros e mensagens de log**.
- Dedicar **tempo contínuo** para estudo (4h seguidas, sem distrações).
- Usar ChatGPT (ou outra IA) para **aprender**.
- Fazer pausas curtas, mas **não se desligar nas férias**: continuar praticando.

---

## 1. Para fazer:

- Usar **Javalin** (backend), **Thymeleaf** (frontend) e **PostgreSQL** (banco de dados) (Em breve).  
- Estudar HTML e a documentação das ferramentas.  
- Criar e rodar o projeto na própria máquina antes da próxima aula.  
- Explorar o código: alterar, testar e observar os resultados.  
- Implementar páginas de erro personalizadas (404 e 500).  
- Ajudar os colegas que tiverem dificuldades.  
- Criar o arquivo `application.properties` com:
```
  porta.servidor=8000
```

---

## 2. Conceitos para se Aprofundar

- Protocolo **HTTP** e seus verbos (especialmente no contexto REST).  
- Conceito de **servidor** e **porta TCP**.  
- Estrutura **MVC** (Model–View–Controller).  
- Funcionamento do **Javalin** e **Jetty**.  
- **Thymeleaf templates** e o uso de layouts padrão.  
- Configuração e boas práticas de **log (Log4J)**.  
- Diferença entre ambientes: desenvolvimento, teste, produção.  
- Fundamentos de **HTML básico**.  
- Estrutura e funcionamento do **Maven** (`pom.xml`).  
- Introdução a **REST** e suas boas práticas.

---

## 3. Questões para Revisão

1. Quais são os principais verbos HTTP e qual a diferença entre eles?  
2. O que é REST e por que é tão associado ao uso de verbos HTTP?  
3. O que é um servidor e o que significa “escutar uma porta”?  
4. Qual a diferença entre HTTP e HTTPS e suas portas padrão?  
5. O que representa o código de erro 404? E o 500?  
6. O que é MVC e qual o papel do Controller no Javalin?  
7. Por que mensagens de exceção não devem ser exibidas ao usuário?  
8. O que é o arquivo `application.properties` e para que serve?  
9. O que diferencia os ambientes de desenvolvimento, teste e produção?  
10. Como o log auxilia o desenvolvedor na depuração de erros?  
11. O que significa “definir rotas” em um framework web?  
12. Quais boas práticas o professor sugeriu para o estudo e prática da disciplina?

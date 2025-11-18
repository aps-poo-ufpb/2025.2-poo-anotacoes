![](attachments/u1251117-arquitetura%20mvc%202025-11-17%2008.21.57.excalidraw.svg)
%%[🖋 Edit in Excalidraw](attachments/u1251117-arquitetura%20mvc%202025-11-17%2008.21.57.excalidraw.md)%%


# Fichamento (17 de novembro de 2025) – APS 2025.2

## Arquitetura de Software

---

### O que é Arquitetura de Software
- Conjunto de **decisões estruturais** sobre como um sistema será organizado.
- Define **tecnologias**, **componentes**, **estrutura**, **regras** e **padrões** que orientam o desenvolvimento.
- Suporta principalmente **requisitos não funcionais**:
  - Disponibilidade
  - Performance
  - Escalabilidade
  - Tolerância a falhas
  - Infraestrutura necessária

---

## Tipos de Projeto
- **Projeto de baixo nível**:
  - Classes, métodos, tabelas, atributos.
  - Decisões locais do código.
- **Projeto arquitetural** (alto nível):
  - Definição de camadas.
  - Escolha de tecnologias.
  - Padronização (ex.: MVC).
  - Estruturação do backend e frontend.

---

## Cenário Inicial: Mercadinho
### Problema proposto
- Apenas **um usuário** utilizará o sistema.
- Funcionamento entre **8 e 10 horas por dia**.
- Sistema precisa rodar em **um único computador**.

### Discussões
- Importância de avaliar a **infraestrutura disponível**.
- Se o sistema estiver lento, sempre perguntar:
  - Qual a **configuração da máquina**?
  - Quantidade de RAM?
  - Processador?
  - Máquina antiga? Suja? (cooler, poeira, etc.)
- Desenvolvimento exige atenção à diferença entre:
  - **Máquina de desenvolvimento** → precisa ser forte (16 GB recomendado)
  - **Máquina do cliente** → pode ser simples, mas depende do sistema

---

## Dimensões de Infraestrutura
### Recursos fundamentais de um servidor
1. **Memória (RAM)**
2. **Processamento (CPU)**
3. **Armazenamento (Disco)**
4. **Rede (banda e latência)**

Cada tipo de trabalho consome mais ou menos desses recursos:
- Upload/Download → disco e memória
- Conversão de arquivos → CPU e memória
- Banco de dados → memória e disco

---

## Disponibilidade
### Definição
- Percentual de tempo em que o sistema está **acessível** e funcionando adequadamente.

### Exemplos
- Disponibilidade de **90% ao mês** → até 72h fora do ar.
- Disponibilidade de **99,9%** → menos de 1 hora fora do ar por mês.

### Causas de Indisponibilidade
- Falha em serviços como **PIX**.
- Erros em provedores externos.
- Problemas de infraestrutura.
- Bugs que tornam o sistema lento.

---

## Escalabilidade
### Escala = quantidade de usuários simultâneos
- No mercadinho: **1 usuário**.
- Em e-commerces → centenas/milhares.

### Tipos de Escalabilidade
#### 1. **Escalabilidade Vertical**
- Melhorar a máquina: mais RAM, mais CPU.
- Aumenta performance, mas **não aumenta tolerância a falhas**.
- Custo costuma ser mais alto.

#### 2. **Escalabilidade Horizontal**
- Duplicar servidores e distribuir carga.
- Requer **balanceador de carga**.
- Melhora performance **e tolerância a falhas**.

---

## Balanceador de Carga
### Funcionamento
- Recebe requisições e distribui entre múltiplos servidores.
- Comum usar **Nginx**.

### Atividade sugerida
- Configurar no próprio computador um Nginx distribuindo entre duas portas:
  - Serviço A → retorna "A"
  - Serviço B → retorna "B"
- Testar round-robin (A B A B...).

### Algoritmos
- **Round-robin** (mais simples)
- Estratégias "sticky" (fixam cada cliente em um servidor)

### Problema clássico
- Estado guardado no servidor quebra balanceamento.
- Solução: mover estado para serviço externo → sistemas **stateless**.

---

## Tolerância a Falhas
### O que é
- Estratégias para manter o sistema funcional **mesmo quando algo falha**.

### Exemplos reais discutidos
- Caso da **Alê Pessoa**:
  - Um único servidor.
  - Risco de indisponibilidade.
  - Solução: enviar e-mails automáticos com pedidos → redundância simples.
- Não vale duplicar infraestrutura quando o custo é mais alto que o risco.

### Exemplo de falhas graves
- Acidentes de avião → vários sistemas redundantes.
- Sistemas só falham após **cadeia** de falhas simultâneas.

---

## Elasticidade
- Situações de demanda variável (ex.: finais de semana, Natal, Black Friday).
- Solução ideal: sistemas **autoescaláveis**.
- Aumentam máquinas conforme demanda, reduzem quando o uso cai.
- Depende de:
  - Estado externo (stateless)
  - Sistemas assíncronos

---

## Sistemas Assíncronos
### Problema do síncrono
- Um serviço depende da resposta do outro.
- Pode gerar lentidão e gargalos.

### Solução: Filas
- Enfileiram eventos para processamento posterior.
- Ex.: Kafka.
- Exemplos:
  - Envio de e-mails em lote
  - Processamento de pagamentos
  - Atualização de estoque

---

## Cache
### O que é
- Armazenamento temporário de dados para acesso rápido.

### Benefícios
- Reduz consultas ao banco.
- Melhora performance.

### Problemas
- Risco de inconsistência.
- Deve-se avaliar impacto no negócio.

### Exemplos vistos
- Lista de produtos (pode ser cacheada)
- Quantidade de curtidas em redes sociais (inconsistência tolerada)
- Informações críticas → **não** devem ser cacheadas (ex.: saldo bancário)

---

## Infraestrutura em Nuvem
### Digital Ocean (exemplo mostrado em aula)
- Escolha da região (Nova York, Atlanta, etc.)
- Latência influencia escolha.
- CPU compartilhada vs. CPU dedicada.
- Custo é fator central na decisão arquitetural.

### Curiosidades
- Máquina de 512 MB pode suportar sistemas reais.
- Custo para 16 GB de RAM → impacto direto no orçamento.
- Sistemas precisam ser otimizados para reduzir custo.

---

## MVC – Model, View, Controller
### Por que usar?
- Padronização.
- Separação de responsabilidades.
- Facilita manutenção.
- Facilita onboarding em equipes.

### Componentes
#### **Controller**
- Recebe requisições HTTP.
- Coordena ações.

#### **Service**
- Regras de negócio.
- Consulta repositórios ou serviços externos.

#### **Repository**
- Acesso a dados.
- Interface com a persistência (PostgreSQL, arquivos ou APIs externas).

#### **Model**
- Representa entidades do domínio.
- Ex.: Usuário, Produto, Item, Pedido.

### Argumentos importantes
- Evitar "macarrão" (código misturado).
- Java é verboso por design → mas mantém clareza.
- Lombok pode causar problemas de manutenção.

---

# 1. Orientações do Professor
- Devem abrir o projeto e colocar para rodar em suas máquinas.
- Testar consumo de RAM de um serviço Java utilizando JavaLink.
- Testar também rodando dentro do Docker.
- Bônus: escrever um script para realizar **teste de carga simples** no endpoint.
- Pesquisar: Kafka, mecanismos de cache, balanceadores, elasticidade.
- Sobre Lombok: o problema **não é** o volume de código em Java — as IDEs geram getters, setters e construtores facilmente, permitindo que você **leia e entenda o código gerado**. O Lombok, por outro lado, **esconde código**, gerando comportamentos invisíveis ao desenvolvedor, o que pode dificultar rastreamento de bugs e manutenção.

---

# TO DO (Atividades sugeridas pelo professor)
- Verificar o consumo de RAM de um serviço Java usando **Javalin**.
- Verificar o consumo de RAM ao rodar o mesmo serviço dentro de um **container Docker**.
- Criar um **script de teste de carga** simples enviando múltiplas requisições ao endpoint `/alomundo`.
- Configurar e testar um **balanceador de carga com Nginx** distribuindo requisições entre dois serviços.
- Pesquisar sobre **Kafka** e entender como filas funcionam.
- Pesquisar mecanismos de **cache** e quando devem ser usados.
- Testar uma aplicação simples **stateless** para entender o impacto na escalabilidade.
- Observar casos reais de escalabilidade em e-commerces durante períodos de alta demanda.

---

# 2. Conceitos para se Aprofundar
 Conceitos para se Aprofundar
- Arquitetura de software vs. projeto de baixo nível.
- Requisitos não funcionais (NFRs).
- Disponibilidade.
- Escalabilidade horizontal vs. vertical.
- Balanceador de carga (Nginx).
- Tolerância a falhas.
- Elasticidade.
- Sistemas stateless.
- Sistemas assíncronos.
- Filas (Kafka).
- Cache e critérios de validade.
- Infraestrutura em nuvem (Digital Ocean, AWS, etc.).
- MVC, Services, Controllers, Repositories.
- Separação de responsabilidades.

---

# 3. Questões para Revisão
1. O que caracteriza uma arquitetura de software?
2. Qual a diferença entre projeto de baixo nível e arquitetural?
3. O que define o tipo de infraestrutura necessária para um sistema?
4. Como calcular indisponibilidade mensal a partir de um SLA?
5. O que diferencia escalabilidade vertical de horizontal?
6. Para que serve um balanceador de carga?
7. Por que sistemas stateless facilitam escalabilidade?
8. O que é tolerância a falhas e como implementá-la?
9. Quando usar cache e quais riscos ele traz?
10. Por que sistemas assíncronos tendem a escalar melhor?
11. Para que servem filas como Kafka?
12. Quais vantagens do padrão MVC na organização do backend?
13. Por que Lombok pode gerar problemas de manutenção?

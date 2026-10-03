# CRIAITOR — Backend

API REST do **CRIAITOR**, uma aplicação que transforma temas e palavras-chave em ideias estruturadas com apoio de inteligência artificial. O backend centraliza autenticação, geração e histórico de ideias, favoritos, chat contextual e métricas de uso.

[![Java](https://img.shields.io/badge/Java-17-ED8B00?logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.3.1-6DB33F?logo=springboot&logoColor=white)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-database-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-cache-DC382D?logo=redis&logoColor=white)](https://redis.io/)
[![OpenAPI](https://img.shields.io/badge/OpenAPI-documentation-6BA539?logo=openapiinitiative&logoColor=white)](https://www.openapis.org/)

## Principais funcionalidades

- Geração de ideias a partir de tema e contexto usando modelos executados pelo Ollama.
- Opção **Surpreenda-me** para criar ideias sem parâmetros prévios.
- Cadastro e login com JWT, renovação de tokens e logout com blacklist.
- Histórico paginado com filtros por tema e período.
- Favoritos e estatísticas de geração por usuário.
- Chat com IA vinculado a uma ideia ou iniciado livremente.
- Limites de mensagens e tokens, moderação de conteúdo e sanitização de prompts.
- Cache de respostas e resumos com Redis/Caffeine.
- Perfis de acesso para usuários e administradores.
- Documentação interativa com Swagger/OpenAPI.

## Tecnologias

| Área | Tecnologias |
| --- | --- |
| Aplicação | Java 17, Spring Boot 3.3.1, Maven |
| Persistência | Spring Data JPA, PostgreSQL, H2 |
| Segurança | Spring Security, JWT, BCrypt |
| Inteligência artificial | Ollama API |
| Cache | Redis e Caffeine |
| Observabilidade | Spring Boot Actuator, Micrometer e Prometheus |
| Qualidade | JUnit 5, Mockito, MockWebServer, JaCoCo e SonarQube |
| Documentação | Springdoc OpenAPI / Swagger UI |

## Arquitetura

O projeto segue uma organização em camadas:

```text
controller  -> endpoints REST e validação de entrada
service     -> regras de negócio e integrações
repository  -> persistência com Spring Data JPA
model       -> entidades de domínio
dto         -> contratos de entrada e saída
security    -> autenticação e autorização JWT
config      -> cache, CORS, OpenAPI e filtros
exceptions  -> tratamento centralizado de erros
```

## Executando localmente

### Pré-requisitos

- Java 17
- Maven 3.9+
- PostgreSQL
- Ollama com o modelo configurado instalado
- Redis, caso o perfil utilizado esteja configurado para cache distribuído

### Configuração

1. Clone o repositório e acesse a pasta:

```bash
git clone https://github.com/galvessantos/projeto-gerador-ideias-backend.git
cd projeto-gerador-ideias-backend
```

2. Defina as variáveis de ambiente. O arquivo [`.env.example`](.env.example) contém a lista completa.

| Variável | Finalidade |
| --- | --- |
| `DEV_DATABASE_URL` | URL JDBC do PostgreSQL no ambiente de desenvolvimento |
| `DEV_DATABASE_USERNAME` | Usuário do banco de desenvolvimento |
| `DEV_DATABASE_PASSWORD` | Senha do banco de desenvolvimento |
| `DATABASE_URL` | URL JDBC do PostgreSQL em produção |
| `DATABASE_USERNAME` | Usuário do banco em produção |
| `DATABASE_PASSWORD` | Senha do banco em produção |
| `JWT_SECRET` | Chave usada para assinar os tokens JWT |
| `IP_ENCRYPTION_KEY` | Chave usada na proteção dos endereços IP armazenados |
| `OLLAMA_BASE_URL` | Endereço do Ollama; padrão: `http://localhost:11434` |
| `OLLAMA_MODEL` | Modelo utilizado; padrão: `mistral` |
| `REDIS_HOST` / `REDIS_PORT` | Conexão com o Redis |
| `MAIL_USERNAME` / `MAIL_PASSWORD` | Credenciais SMTP para notificações |
| `ADMIN_NOTIFICATION_EMAIL` | Destinatário das notificações administrativas |

Exemplo de URL JDBC:

```text
jdbc:postgresql://localhost:5432/criaitor
```

3. Inicie o Ollama e execute a aplicação:

```bash
ollama serve
mvn spring-boot:run
```

A API será iniciada em `http://localhost:8080`.

## Endpoints principais

| Grupo | Rota base | Responsabilidade |
| --- | --- | --- |
| Autenticação | `/api/auth` | Cadastro, login, refresh token e logout |
| Ideias | `/api/ideas` | Geração, histórico, favoritos e métricas |
| Chat | `/api/chat` | Sessões, mensagens, logs e resumos |
| Temas | `/api/themes` | Consulta e administração de temas |
| Usuários | `/api/users` | Atualização de perfil e estatísticas |

Com a aplicação em execução, a especificação completa fica disponível em:

- Swagger UI: `http://localhost:8080/swagger-ui/index.html`
- OpenAPI JSON: `http://localhost:8080/v3/api-docs`

## Testes

```bash
mvn test
```

Os testes cobrem controllers, serviços, repositórios, segurança, cache, integração com o Ollama e tratamento de erros. O relatório de cobertura do JaCoCo é gerado em `target/site/jacoco/`.

## Frontend

A interface web do projeto está em [projeto-gerador-ideias-frontend](https://github.com/galvessantos/projeto-gerador-ideias-frontend).

## Contexto

Projeto desenvolvido em equipe durante o programa **Acelera Maker**, promovido pela Montreal, com foco em construção de uma aplicação full stack, qualidade de código e colaboração ágil.

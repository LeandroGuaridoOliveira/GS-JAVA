# OrbitGuard API

API REST em Java/Spring Boot para a Global Solution 2026/1 da FIAP.

## Proposta da solucao

OrbitGuard conecta dados orbitais, boletins de defesa civil e analise preditiva para monitorar areas vulneraveis e gerar alertas de risco climatico. A solucao atende ao tema de economia espacial porque usa infraestrutura/dados de satelite para resolver problemas reais na Terra, especialmente prevencao de desastres em cidades e regioes costeiras.

## Tecnologias

- Java 17
- Spring Boot 3.5.14
- Spring Web
- Spring Data JPA e JpaRepository
- Spring Security com JWT
- Spring Validation
- Spring HATEOAS
- Lombok
- H2 Database
- Swagger/OpenAPI
- Maven Wrapper

## Como executar

```powershell
.\mvnw.cmd spring-boot:run
```

A API sobe em `http://localhost:8080`.

Links locais:

- Swagger: `http://localhost:8080/swagger-ui.html`
- OpenAPI JSON: `http://localhost:8080/api-docs`
- H2 Console: `http://localhost:8080/h2-console`
- Health: `http://localhost:8080/health`

Banco H2:

- JDBC URL: `jdbc:h2:mem:orbitguard`
- User: `sa`
- Password: vazio

Importante: no H2 Console, troque o valor padrao `jdbc:h2:~/test` por `jdbc:h2:mem:orbitguard`. O banco e em memoria e so existe enquanto a API estiver rodando.

## Deploy publico

O projeto possui `Dockerfile` e usa a variavel `PORT`, exigida por plataformas de deploy. Caminho recomendado:

1. Acesse Render ou Railway.
2. Crie um novo Web Service a partir do repositorio `https://github.com/LeandroGuaridoOliveira/GS-JAVA`.
3. Escolha deploy por Dockerfile.
4. Configure, se necessario:
   - Branch: `main`
   - Root directory: vazio
   - Health check path: `/health`
5. Aguarde o build finalizar.
6. Abra a URL publica gerada e teste:
   - `/health`
   - `/swagger-ui.html`
   - `/api-docs`

Exemplo do formato esperado:

```text
https://nome-do-servico.onrender.com/swagger-ui.html
```

## Usuario de demonstracao

```json
{
  "email": "admin@orbitguard.com",
  "senha": "orbitguard123"
}
```

Login:

```http
POST /auth/login
Content-Type: application/json

{
  "email": "admin@orbitguard.com",
  "senha": "orbitguard123"
}
```

Use o token retornado como `Authorization: Bearer <token>` nas rotas protegidas.

## Rotas principais

### Autenticacao

- `POST /auth/register`
- `POST /auth/login`

### Areas monitoradas

- `GET /areas`
- `GET /areas/{id}`
- `POST /areas`
- `PUT /areas/{id}`
- `DELETE /areas/{id}`

Para testar `DELETE /areas/{id}`, use preferencialmente uma area criada durante o teste. As areas carregadas automaticamente possuem observacoes, alertas ou inscricoes vinculadas; nesses casos a API retorna `409 Conflict` para preservar a integridade dos dados.

### Observacoes orbitais

- `GET /observacoes`
- `GET /observacoes/{id}`
- `POST /observacoes`
- `PUT /observacoes/{id}`
- `DELETE /observacoes/{id}`

### Alertas de risco

- `GET /alertas`
- `GET /alertas/{id}`
- `POST /alertas`
- `PUT /alertas/{id}`
- `DELETE /alertas/{id}`

## Exemplos de requisicao

Criar area monitorada:

```json
{
  "nome": "Marginal Tiete - Trecho Lapa",
  "cidade": "Sao Paulo",
  "uf": "SP",
  "descricao": "Trecho urbano sensivel a transbordamento e interrupcao de mobilidade.",
  "latitude": -23.5162,
  "longitude": -46.7074,
  "nivelVulnerabilidade": "ALTO"
}
```

Criar alerta:

```json
{
  "titulo": "Risco critico de alagamento",
  "descricao": "Indice de risco elevado por acumulado de chuva e solo saturado.",
  "severidade": "CRITICA",
  "status": "ABERTO",
  "areaId": 1,
  "observacaoBaseId": 1,
  "probabilidadePercentual": 91,
  "recomendacao": "Acionar plano de contingencia e orientar rotas alternativas.",
  "validoAte": "2026-06-07T23:59:00-03:00"
}
```

## Atendimento aos requisitos de Java Advanced

- API REST com Spring Boot: controllers em `br.com.fiap.orbitguard.controller`.
- Boas praticas e camadas: `controller`, `service`, `repository`, `domain`, `dto`, `security`, `exception`, `config`.
- Verbos HTTP, request/response e status codes: CRUD usa `GET`, `POST`, `PUT`, `DELETE`, `201 Created`, `204 No Content` e erros padronizados.
- HATEOAS: responses de areas, observacoes e alertas retornam links `_links`.
- Injeção de dependencia, Lombok e DevTools: configurados no POM e usados nas classes.
- Persistencia JPA/JpaRepository: entidades mapeadas e repositorios Spring Data.
- CRUD completo: areas, observacoes orbitais e alertas de risco.
- DTOs e Java Records: records em `br.com.fiap.orbitguard.dto`.
- Validacao: Bean Validation em todos os requests.
- Tratamento de excecoes: `GlobalExceptionHandler` e `ApiError`.
- Modelagem avancada: heranca JPA (`EventoMonitoramento` -> `AlertaRisco`), chave composta (`InscricaoAreaId`), `@Embedded` (`Coordenadas`, `MetricasClimaticas`) e multiplas tabelas.
- Spring Security/JWT: login, registro e filtro JWT.
- Swagger/OpenAPI: `springdoc-openapi` em `/swagger-ui.html`.
- CORS: origens locais configuradas em `application.properties`.

## Testes

```powershell
.\mvnw.cmd test
```

Os testes validam:

- endpoint publico `/health`;
- login com JWT;
- acesso autenticado a `/areas`.

## Entrega

Integrantes:

- Kaiky Pereira Rodrigues Da Silva - RM 564578 - Turma 2TDS
- Leandro Guarido de Oliveira - RM 561760 - Turma 2TDS
- Gabriel Costa Solano - RM 562325 - Turma 2TDS

- Link publico do GitHub: `https://github.com/LeandroGuaridoOliveira/GS-JAVA`
- Link publico do deploy: `https://gs-java-w0qo.onrender.com`
- Link do video de apresentacao: `https://youtu.be/5O5FWFUdbfQ`

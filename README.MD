# API Gateway e Kong na prática.

## Guia Prático: Implementação de API Gateway com Kong
Este guia detalha a implementação e utilização de um API Gateway usando o Kong, abordando desde conceitos fundamentais até a configuração prática. O objetivo é facilitar a gestão de APIs, centralizando a comunicação e adicionando camadas de segurança, autenticação, monitoramento e balanceamento de carga.

## Instalação
1. Clone o repositório:
```bash
git https://github.com/Eddiesantle/api-gateway-with-kong.git
cd api-gateway-with-kong
```

2. Suba o container do Kong:
```bash
docker-compose up
```

3. Execute a API:
```bash
cd node-api
```

4. Instale as dependências:
```bash
npm install
```

## Sem API Gateway
<img src="assets/api-gateway-sem.png">

### Hyperconnectivity mess

<img src="assets/what-is-networking-software-628x353.avif">

Bagunça de hyper connectividade milhares de aplicações ficar conectadas o tempo inteiro e são conectadas se comunicando, como controlar se as informações não estão expostas se estão seguro, controlar acessos, saber de fato o que esta contecendo. Malha de aplicações se comuinicando o tempo inteiro.

## Com API Gateway
<img src="assets/API Gateway draft-Dec-15-2020-02-29-51-50-PM.webp">


Um API Gateway é uma aplicação que fica em frente de um conjunto de APIs e age como um ponto único de entrada para um grupo definido de microsserviços/serviços, criando um único canal de mapeamento de tráfego externo para tráfego interno.

API Gateway é um padrão de comunicação externa para microsserviços, com o objetivo de:

- Receber todos os request de um tipo específico de client (mobile, web app, consumo de API);
- Determinar quais serviços são necessários; 
- Os combinar em uma experiência síncrona para o usuário.

### O que é uma API Gateway?
"Uma API Gateway é uma API de <b>gerenciamento de API</b> que vai estar presente entre uma coleção de seviços do lado do backend e do client"

### Benefícios de uma API Gateway

<img src="assets/api-gateway-reduce-rtt.png">

- Proteger as APIs contra uso excessivo, provendo autentifição/autorização. rate limit e etc.
- Entender como as pessoas estão usando as APIs coletando métricas e dados para monitoramento.
- Controlar a monetização das APIS.
Adoção de Backend for Front-end.
- Adoção de Arquitetura de microsserviçõs
- Manter consisstência de roteamento das chamadas

### Principais funcionalidades

São inúmeras as funcionalidades e utilização de um API Gateway. As suas principais funcionalidades são:

- Autenticação e Autorização;
- Aplicação de políticas de segurança (header, CSRS, outros);
- Balanceamento de carga;
- Gerenciamento de cache;
- Resolução de dependências (integração com service discovery);
- Gerenciamento de SLA;
- Políticas de retentativas, circuit breaker e QoS;
- Rate limiting e throttling;
- Ibservabilidade das APIS (Log, trace, correlação e metricas);
- Transformações de Headers, query string;
- IP Whitelist;
- SSL/TLS externo, clear text interno;

### Objetivos de uma API Gateway
<i>"Fornecer serviços ortogonais para APIS, serviços estes que seriam preocupações das próprias APIs. 
Normalmente a atuação se dá na camada de rede"</i>

## Tipos de API Gateway

### ENTERPRISE GATEWAY
- Normalmente ligada a um vendor (lock-in)
- Gestão do ciclo de vida das API:
    - Criação e design (dev, homolog, test)
    - Publicação e versionamento
    - Monitoramento
    - Monetização
    - etc
- Governança de API
- Suport a mútiplos protocolos: HTTP, SOAP, GraphQL, Websockets, etc.
- Geralmente grande e com algumas dependências externas.
- Outros serviços ortogonais

#### Exemplos de Enterprise Gateway
- WSO2
- Apigee (Google Cloud)
- IBM API Connect
- Azure API Management
- Amazon API Gateway
- etc

### MICROSERVICES GATEWAY
- Geralmente mais leve que enterprise
- Maioria <b>open-source (permite plugins e extersões)</b>
- Normalmente bem leves
- Foco mais em roteamento, observabilidade das APIs
- Configuração dinâmica e declarativa

#### Exemplo de Microservices Gateways
- Kong
- Kyk
- KrakenD
- API Gateway de vendor (google, Microsoft, Azure)
- etc

## KONG

<img src="assets/kong.png" style="width: 200px;"/>

#### O Que é Kong?
<i>"Uma empresa que provê um ecossistema de ferramentas/serviços para gerenciamento de APIs, um deles o Kong API Gateway"</i>

#### O Que é o Kong API Gateway?
- API Gateway de alta performance, alta extensível e escalável.
- Construída sob o <b>Nginx</b> e <b>OpenResty</b>.
- Usa linguagem <b>Lua</b> para aplicar funcionalidades e contruir plugins.
- Utiliza o PostgreSQL/Cassandra.
- É dividido em 2 versões: <b>Community (Kong)</b> e <b>Enterprise (Kong Gateway)</b>

## Pratica e implementação

### Endereços no Kong
- Gateway de Entrada: http://localhost:8000/
- Admin API: http://localhost:8001/
- Admin GUI: http://localhost:8002/

#### Criar Projeto de API Básica
1. Iniciar um projeto Node.js:
```bash
npm init -y
```
2. Instalar o Express:
```bash
npm install express
```
3. Executar API
```bash
node api.js
```

#### Como conetar API que esta na porta http://localhost:3000 com o kong?

Qualquer api que vamos permitir com que o kong administre, vamos ter que cadastrar como um serviço que o kong vai acabar interceptando sendo intermediario

##### Criando serviço

<i>Acessando>ADMIN GUI>Gateway Services>New Gateway Service</i>

General Information
General information will help identify and manage this Gateway Service.

Em Service Endpoint > Upstream URL - Quando for localmente configurar

```
# Windows
# C:\Windows\System32\drivers\etc\hosts (bloco de notas em modo admin)

127.0.0.1 host.docker.internal

# Linux ou Mac
# /etc/hosts (editar como admin)

127.0.0.1 host.docker.internal
```

o "host.docker.internal" vai sair da rede do docker vai bater na minha maquina, o OS não fai sabar o que faser e vai redirecionar para local host e bater no serviço

##### Criar Roteamento

<i>Acessando>ADMIN GUI>Routes>New Routes</i>

Agora é possivel acessar api pelo endereço: http://localhost:8000/test

<img src="assets/route-kong.png">

##### Criar Autentificação

<i>Acessando>ADMIN GUI>Plugins>New Plugin>Key Authentification</i>

Vai permitir passar no header|Body query da URL uma chave a autentificação para acessa API

É possivel ativar diretamente na rotas ou "Global"

Criar somente na rota:
<i>Acessando>ADMIN GUI>Routes>Select Route>Plugins>new Plugin</i>

##### Criar Consumidor

Considerado o priprio usuario da API, aplicação CLIENT que esta consumindo api

<i>Acessando>ADMIN GUI>Consumers>new Consumer</i>

Adicionar credencial:
<i>Acessando>ADMIN GUI>Consumers>select Consumer>credentials</i>

<img src="assets/credential-apikey.png">

Informar "apikey" para ter acesso

<img src="assets/apikey-informe.png">

##### Criar JWT

<i>Acessando>ADMIN GUI>Routes>Select Route>Plugins>new Plugin/JWT</i>

Adicionar credencial:
<i>Acessando>ADMIN GUI>Consumers>select Consumer>credentials/ New JWT Credential</i>

<img src="assets/jwt-gerado.png">

Deoded JWT - Incluir 

- PAYLOAD:
"iss": <b>[Key GERADA]</b>

- HMACSHA256:
base64UrlEncode(header) + "." +
base64UrlEncode(payload),
<b>[Secret GERADA]</b>

```
HEADER:
{
  "alg": "HS256",
  "typ": "JWT"
}
PAYLOAD:
{
  "iss":"WmAxuR0drdfRUTZdIYYQz60IDm0nb8Vz",
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
VERIFY SIGNATURE
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJXbUF4dVIwZHJkZlJVVFpkSVlZUXo2MElEbTBuYjhWeiIsInN1YiI6IjEyMzQ1Njc4OTAiLCJuYW1lIjoiSm9obiBEb2UiLCJpYXQiOjE1MTYyMzkwMjJ9.UyaQqKLKEgXQDjQiiITMrRM4IAEeTB2jl5Se1qPI408

) secret base64 encoded
```

Liberado acesso a API
<img src="assets/jwt-router.png">

##### Criar Rate Limiting


<i>Acessando>ADMIN GUI>Routes>Select Route>Plugins>new Plugin/Rate Limiting</i>

Definir quantidade de requisição por tempo.

Exemplo: 10 requisições por minuto - API rate limit exceeded
<img src="assets/rate-limit-http.png">

##### Criar Loadbalancer

<img src="assets/admin_ha_config.png">

###### Configurar Upstreams

<i>Acessando>ADMIN GUI>Upstreams>New Upstream</i>

Configurar os targests: replicas do serviçso que aceitam a chamada
<i>Acessando>ADMIN GUI>Upstreams>Select Upstream> targets>New Target</i>

Name: upstream-exemplo
Target Address: host.docker.internal:3000
Target Address: host.docker.internal:3001

Rodar node-api na porta 3001
```bash
PORT=3001 node api.js
```

Para configurar os acessos
<i>Acessando>ADMIN GUI>Gateway Services>New Gateway Service</i>

General information will help identify and manage this Gateway Service.
Name: upstream-exemplo
Service Endpoint: (select) Protocol, Host, Port and Path
Host: upstream-exemplo (IMPORTANTE: vai buscar mesmo nome do criado no Upstreams)

Criar roteamento
<i>Acessando>ADMIN GUI>Routes>New Route</i>

General information will help you identify and manage this route
Name:load-balancing
Service: (Select) upstream-exemplo-....
Route Configuration
HTTP/HTTPSRouting Rules
Paths: (/load-balancing)

Acessando a rota:  

A cada requisição é distribuido:

http://localhost:8000/load-balancing
<img src="assets/example-loadbalancing.png">

##### Criar Telemetria

Criar telemetria
<i>Acessando>ADMIN GUI>Plugins>New Plugin>opentelemetry</i>



### Conclusão
Implementar um API Gateway com Kong não apenas melhora a segurança e a gestão das suas APIs, mas também facilita a adição de novas funcionalidades como autenticação, monitoramento e controle de tráfego. Esta abordagem torna a arquitetura de microsserviços mais robusta e escalável, reduzindo a complexidade e garantindo um melhor desempenho e segurança.

### Proximo desafio
- Integrar kong com  keycloak 

### Referências
[Kong API Gateway Documentation](https://docs.konghq.com/)
[Node.js Express Documentation](https://expressjs.com/)
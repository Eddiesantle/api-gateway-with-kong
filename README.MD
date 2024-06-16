# API Gateway e Kong na prática.


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
- http://localhost:8000/ - gateway de entrada das apis
- http://localhost:8001/ - ADMIN API
- http://localhost:8002/ - ADMIN GUI

Criar projeto api basica com node
```bash
npm init -y
```
Instalar express
```bash
npm install express
```
Executar API
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
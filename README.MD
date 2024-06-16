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


# Serveless

## History

- Data centers
- IAAS
- PAAS
- Containers
- Serverless


## AWS Lambda

O AWS lambda é um serviço de computação no qual você pode fazer upload de seu código e criar uma função lambda. O AWS Lambda se encarrega de provisionar e gerenciar os servidores que você usa para executar o código. Você não precisa se preocupar com sistemas operacionais, patches, dimensionamento, etc. Você pode usar o Lambda das seguintes maneiras:
- Como um serviço de computação orientado a eventos, no qual o AWS Lambda executa seu código em resposta a eventos. Esses eventos podem ser alterações nos dados em um bucket do Amazon S3 ou em uma tabela do Amazon DynamoDB.
- Como um serviço de computação para executar seu código em resposta a solicitações HTTP usando o gateway de API da Amazon ou chamadas de API feitas usando SDKs da AWS

### Linguagens suportadas

- Node.js
- Java
- Python
- C#
- Go
- PowerShell
- Ruby

### Preço

- Número de requisições: os primeiros 1 milhão de solicitações são gratuitas, após US $ 0,20 por 1 milhão de solicitações a partir de então.
- Duração: calculado a partir do momento em que seu código começa a ser executado até retornar ou terminar, arredondado para os 100ms mais próximos. O preço depende da quantidade de memória que você aloca para sua função. Você cobra $ 0,00001667 por cada segundo de GB usado.

### Vantagens

- Não existe preocupação com os servers
- Ela está sempre escalando (para mais requisições, não para poder computacional)
- São muito baratas!
- Foco na aplicação e não na infraestrutura

### Desvantagens
- Tempo para a inicialização
- Não é tão performático quanto a arquitetura tradicional
- Limitado as linguagens suportadas
- Não é adequado para todo tipo de problema. Ex: grande processamento de dados (O tempo limite de execução é 5 minutos)

### Controle de Versão

Ao usar o controle de versão no AWS Lambda, você pode publicar uma ou mais versões de suas funções do Lambda. Como resultado, você pode trabalhar com diferentes variações da sua função Lambda no fluxo de trabalho de desenvolvimento, como desenvolvimento, beta e produção.
Cada versão da função Lambda possui um ARN (Amazon Resource Name) exclusivo. Depois de publicar uma versão, ela é imutável (ou seja, não pode ser alterada).
O AWS Lambda mantém seu último código de execução na versão $ LATEST. Quando você atualiza seu código de função, o AWS Lambda substitui o código na versão $ LATEST da função Lambda.

#### Qualified/Unqualified ARN

Você pode consultar esta função usando seu Amazon Resource Name (ARN). Existem dois ARNs associados a esta versão inicial
 ARN Qualified a função ARN com o sufixo da versão 
arn: aws: lambda: aws-region: acct-id: function:helloworld:$LATEST
ARN Unqualified a função ARN sem o sufixo da versão 
arn: aws: lambda: aws-region: acct-id: function:helloworld

### Alias

Depois de criar inicialmente uma Função Lambda (a versão $LATEST), você pode publicar uma versão 1 dela. Criando um alias denominado PROD que aponta para a versão 1, agora você pode usar o alias PROD para invocar a versão 1 da função Lambda.
Agora você pode atualizar o código (a versão $LATEST) com todos os seus aprimoramentos e publicar outra versão estável e aprimorada (versão 2). Você pode promover a versão 2 para produção remapeando o alias do PROD para que aponte para a versão 2. Se você encontrar algo errado, poderá reverter facilmente a versão de produção para a versão 1 remapeando o alias do PROD para que aponte para a versão 1.

- Pode ter várias versões das funções lambda
- A versão mais recente usará $latest
- Versão Qualified usará $latest, não Unqualified não terá
- As versões são imutáveis (não podem ser alteradas)
- Pode dividir o tráfego usando alias para diferentes versões
- Não é possível dividir o tráfego diretamente do $ latest, deve-se criar um alias para $latest

#### Exam tips

- As funções Lambda são independentes, 1 evento = 1 função
- As funções Lambda podem acionar outras funções lambda, 1 evento pode acionar X outras funções.
- AWS X-Ray permite depurar o que está acontecendo em um lambda
- O Lambda pode fazer coisas globalmente, você pode usá-lo para fazer backup de buckets S3 em outros buckets, etc.

## AWS X-Ray

O AWS X-Ray é um serviço que coleta dados sobre a solicitação que seu aplicativo manda, fornecendo ferramentas usadas para exibir, filtrar e obter informações sobre esses dados para identificar problemas e oportunidades de otimização. Para qualquer solicitação rastreada para seu aplicativo, você pode ver informações detalhadas não apenas sobre a solicitação e resposta, mas também sobre as chamadas que seu aplicativo faz para downstream de recursos, microsserviços, bancos de dados e APIs da Web HTTP da AWS.

### X-Ray Architecture

Você deve ter o X-Ray SDK instalado em seu aplicativo
Assim ele irá enviar bits de json para o X-Ray Deamon (instalado dentro do SO)
O X-Ray Deamon envia esse json para a X-Ray API
A X-Ray API armazena todo esse json 
E constrói visualização dessas comunicações  no X-Ray Console

### X-Ray SDK

- Interceptores a serem adicionados ao seu código para rastrear a solicitação HTTP recebida
- Client handlers para instrumentar do AWS SDK que seu aplicativo usa para chamar outros serviços da AWS
- Um cliente HTTP a ser usado para instrumentar chamadas para outros serviços da Web HTTP internos e externos

### X-Ray Integration

- Elastic Load Balancing
- AWS Lambda
- Amazon API Gateway
- Amazon Elastic Compute Cloud
- Aws elastic Beanstalk

### X-Ray Languages

- Java
- Go
- Node.js
- Python
- Ruby
- .NET

## Step Functions

Step Functions permitem visualizar e testar seus aplicativos Serverless. As Step Functions fornecem um console gráfico para organizar e visualizar os componentes do seu aplicativo como uma série de etapas (Grafos). Isso simplifica a criação e a execução de aplicativos de várias etapas. As Step Functions disparam e rastreia automaticamente cada etapa e tentam novamente quando há erros, para que seu aplicativo seja executado na ordem e conforme o esperado. As funções de etapa registram o estado de cada etapa, portanto, quando as coisas dão errado, você pode diagnosticar e depurar problemas rapidamente.

- Ótima maneira de visualizar seu aplicativo Serverless
- Step Functions disparam e rastreia automaticamente cada etapa
- Step Functions registra o estado de cada etapa, facilitando você encontrar onde é que o erro aconteceu

## API Gateway

É um serviço totalmente gerenciado que facilita para os desenvolvedores publicar, manter, monitorar e proteger APIs em qualquer escala. Com alguns cliques no AWS Management Console, você pode criar uma API que funciona como uma "porta de entrada" para aplicativos acessarem dados, lógica comercial ou funcionalidade de seus serviços de back-end, como aplicativos em execução no Amazon Elastic Compute Cloud ( Amazon EC2), código em execução na AWS ou em qualquer aplicativo Web

### Tipos de APIS

- REST APIs (Representation State Transfer)
- SOAP APIs (Simple Object Access Protocol)

### O que API Gateway pode fazer?

- Expor HTTPS endpoints para definir uma API RESTful
- Conexão serveless-ly a serviços como Lambda e Dynamo DB
- Envie endpoints da API para um destino diferente
- Execute de forma eficiente com baixo custo
- Escale sem esforço
- Rastrear e controlar o uso por chave de API
- Throttle requests para evitar ataques
- Conecte-se ao CloudWatch para registrar todas as solicitações de monitoramento
- Mantenha várias versões da sua API

### Configuração do API Gateway

Definir uma API (container)
Definir recursos e  nested recursos (caminhos de URL)
Para cada recurso:
Selecionar métodos HTTP suportados (verbos)
Definir segurança
Escolha o destino (como EC2, Lambda, DynamoDB, etc.)
Definir transformações de solicitação e resposta
Implantar API em um Stage:
Usa o domínio API Gateway, por padrão
Pode usar domínio personalizado
Agora oferece suporte ao AWS Certificate Manager: certificados SSL / TLS gratuitos.
Advanced API Gateway

#### Importação de API (Swagger 2.0)

Você pode usar o recurso de importação de API para importar uma API de um arquivo externo de definição (swagger file). Atualmente, o recurso Import API suporta arquivos de definição do Swagger v2.0.
Com a API de importação, é possível:
Criar uma nova API enviando uma solicitação POST que inclua uma definição Swagger na configuração da carga e do ponto final
Atualizar uma API existente usando a solicitação PUT que contém uma definição Swagger na carga útil. Você pode atualizar uma API substituindo-a por uma nova definição ou mesclando uma definição com uma API existente. Você especifica as opções usando um parâmetro de consulta de modo no URL da solicitação.

#### API Throttling

- Por padrão (sem cobrança extra), o API Gateway limita a taxa de solicitações em steady-state a 10.000 solicitações por segundo (rps)
- O máximo de solicitações simultâneas é de 5.000 em todas as APIs de uma mesma conta da AWS
- Se você exceder 10.000 solicitações por segundo ou 5.000 solicitações simultâneas, receberá uma resposta de erro HTTP 429 Too Many Request

#### SOAP Webservice Passthrough

Você pode configurar o API Gateway como uma passagem de serviço da web SOAP

#### API Gateway Caching

Você pode ativar o cache da API no Amazon API Gateway para armazenar em cache a resposta dos seus endpoints. Com o armazenamento em cache, você pode reduzir o número de chamadas feitas para o terminal e também melhorar a latência das solicitações para sua API. Quando você ativa o cache para um estágio, o API Gateway armazena em cache as respostas por um período de tempo de vida (TTL), especificado em segundos. O API Gateway responde à solicitação consultando a resposta no cache, em vez de fazer uma solicitação ao seu endpoint.

#### Origin Policy

Na computação, a same-origin policy é um conceito importante no modelo de segurança de aplicativos da web. De acordo com a política, um navegador da Web permite que scripts contidos em uma primeira página da Web acessem dados em uma segunda página da Web, mas apenas se as duas páginas têm a mesma origem.
Isso é feito para evitar ataques XSS (Cross-Site Scripting), segurança aplicada por navegadores da Web e ignorada por ferramentas como PostMan e curl.
Compartilhamento de Recursos de Origem Cruzada (CORS) é uma maneira de o servidor do outro lado (não o código do cliente no navegador) ser um mecanismo que permite que recursos restritos (por exemplo, fontes) em uma página da Web sejam solicitados a outro domínio fora do domínio com o qual o primeiro recurso foi veiculado.

- O navegador faz uma chamada HTTP OPTINS para um URL (OPTIONS é um método HTTP como GET, PUT e POST)
- O servidor retorna uma resposta que diz: "esses outros domínios foram aprovados para GET this URL"
- Caso de o Erro: "A política de origem não pode ser lida no recurso remoto?" você precisa ativar o CORS no API Gateway

# Exam Tips

- Lembre o que o API Gateway é de alto nível
- O API Gateway possui recursos de cache para aumentar o desempenho
- API Gateway é de baixo custo e escala automaticamente
- Você pode controlar o API Gateway para evitar ataques
- Você pode registrar resultados no CloudWatch
- Se você estiver usando Javascript / AJAX que usa vários domínios com o API Gateway, verifique se ativou o CORS no API Gateway
- CORS é imposto pelo cliente

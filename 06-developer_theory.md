# CI/CD

## AWS CodeCommit

- Baeado no Git
- É um repositório centralizado para todo o seu código, binarios, imagnes e libraries.
- Rastreia e gerencia as mudanças no código
- Mantém um hitorioco de versões
- Gerencia atualizações de várias origens e habilita colaboração
- Pode ser criados notificações em SNS, CloudWatch para eventos no seu repositório

## CodeDeploy

É um serviçõ de deploy automatico para permite você fazer deploy do código da sua aplicação direto na instancia EC2, sistema on-premise a Lambda Functions.

Isso permite que você lançe nova features rapidamente, evita downtime durante o deploy da aplicação e evita o risco associado com processos manuais. 

Automaticamente escala com a sua infraestrutura e integra com varias ferramentas de CI/CD (como Jenkins, Github, Atlassian, AWS Code Pipeline, bem como ferramentas de gerenciamento de configurações como Ansibgle, Puppet and Chef.

Dois metodos de deploy permitidos:
- In-Place
- Blue/Green

### In Place Deployment

(img)

- Se para a aplicação em cada instancia e a ultima versão é insalada
- The instancia fica fora de serviço durante esse tempo e sua capacidade de serviço é reduzida
- Se a instancia está atras de um load balancer, você pode configurar o load balancer para parar de mandar requisições para a instancia que estão com a versão sendo atualizada.
- In place é conhecido também como `Rolling Update`
- Só pode ser usado em EC2 e op-premise, não é suportado para Lambda
- Se você precisar desfazer suas mudanças, a ultima versão da sua aplicação tem que ser deployed de novo.

### Blue/Green Deployment

(img)

- Novas instancias são provisionadas e as ultimas versões são instaladas nessa novas instancias. Blue representa o deploy ativo e o Green sao as ultimas versões.
- As novas instancias são registradas com o Elastic Load Balancer, o trafico é encaminhado para as novas instancias e as instancias originais são terminadas eventualmente.
- As vantagens do Blue/Green deployment são que as novas instancias podem ser criadas antes do tempo e o código é liberado em produção simplesmente trocando o trafico para as novas instancias.
- Trocar de volta para  o ambiente original é rápido e mais seguro, apenas trocar o trafico para as instancias originais (no caso delas ainda não terem sido terminadas)

### Termologia do CodeDeploy

- Deployment Group - um grupo de instancias EC2 ou Lambda que receberam a nova versão do software a ser deployed
- Deployment - o processo e os componentes usados para aplicar a nova versão em produção
- Deployment Configuratuin - um grupo de regras de deploy e condições de sucesso e falha usados durante o deploy
- AppSpec File - define as ações de deployment você quer que o AWS CodeDeploy execute
- Revision - tudo que é necessario para realizar o deploy de uma nova versão: AppSpec file, application files, executaveis, config files
- Application - identificadoes unico da aplicação que você quer fazer o deploy. Para garantir que 

## AppSpec File

The AppSpec File é usado para definir os parametros que serão usaos no CodeDeply. A estrutura do arquivo depende se será utilizado para o EC2 ou Lambda.

### AppSpec para o Lambda

Ele deve ser escrito em YAML or JSON e conter os seguintes campos:
- version: reservado para uso futuro, por enquanto aceita apenas "0.0"
- Resources: o nome e propriedades do Lambda a ser feito o deploy
- hooks: especifica o lambda a rodar certos pontos antes do deploy para validar esse deploy antes de permitir o trafico

```yaml
version: 0.0
resources:
  - myLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      Name: "myLambdaFunction"
      Alias: "myLambdaFunctionsAlias"
      CurrentVersion: "1"
      TargetVersion: "2"
hooks:
  - BeforeAllowTraffic: "LambdaFunciontToValidateBeforeTrafficShift"
  - AfterAllowTraffic: "LambdaFunctionToValidadeAfterTrafficShift" 
```

#### Hooks

- BeforeAllowTraffic: 
- AfterAllowTraffic: 

### AppSpec para EC2 e On premises

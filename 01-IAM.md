# Guia para iniciantes do IAM

IAM permite gerenciar o uso e o nível de acesso ao console da AWS, oferecendo:
- Controle centralizado sobre a conta AWS.
- Acesso compartilhado à sua conta AWS.
- Permissão granulares.
- Identity Federation (Active Directory, Facebook, LinkedIn, etc).
- Multifactor Authentication.
- Acessos temporários para usuários ou serviços conforme necessário.
- Permite você configurar sua própria policy de rotação de senhas.
-  Integrar com diferentes serviços AWS.
- Suporte a PCI DSS compliance.

Com IAM podemos criar grupos de permissões, ex:

Seu time de marketing precisa de acesso ao s3 bucket para ler e gravar imagens, faz sentido então criar um grupo chamado, por exemplo, mkt_bucket_upload e criar uma policy com as permissões de upload de arquivos para o bucket. Desta maneira basta vincular a policy ao grupo e adicionar os usuários de marketing ao grupo mkt_bucket_upload.
Exemplo real: Nosso grupo Developers.

Também temos Papéis (Roles) são permissões para os serviços da AWS, como EC2, Lambda...

Também temos Policies, as policies podem ser anexadas a um grupo ou a uma Role de usuário, ali será definido as permissões e atribuídas ao grupo ou a Role.

## IAM Lab

- Conhecendo um pouco do console da AWS
- Criando um usuário
- Verificando o Conteudo das policies
- Adicionando uma policy ao usuário
- Criando um grupo
- Adicionado o usuário ao seu grupo
- Removendo o grupo
- Criando uma role

# IAM Summary

- IAM consistem nos seguintes itens:
  - Users
  - Groups
  - Roles
  - Policy Documents
- Exemplo da estrutura base: 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

- IAM é universal. Não se aplica a regiões no momento.
- Conta Root é simplesmente a conta gerado quando você cria a sua conta AWS pela primeira vez. Tem acesso administrativo completo.
- Novos usuários não tem permissão quando são criados.
- Novos usuários recebem um Access Key ID e Secret Access Key quando são criados.
- Você pode usar as chaves para acessar a AWS via linha de comando.
- Uma vez que gerou o usuário você pode ver o Accesss Key ID mais não mais o Secret Key, por isso salve sua chave.
- Se necessário você pode regerar a Secred Key, removendo acesso da antiga.
- Use o Multifactor Authentication (MFA) em sua conta root.
- Crie e Customize suas policies.

## Identity Access Management Quiz

1. Qual das opções a seguir NÃO é um recurso do IAM?

- [ ] Controle centralizado da sua conta da AWS
- [ ] Integra-se à sua conta do Active Directory existente, permitindo single sign-on
- [ ] Controle de acesso refinado aos recursos da AWS
- [x] Permite configurar a autenticação biométrica, de modo que nenhuma senha seja necessária.

2. A AWS recomenda que as instâncias do EC2 tenham credenciais armazenadas nelas, para que elas possam acessar outros recursos (como buckets S3).

- [ ] Verdadeiro
- [x] Falso.

3. Qual entidade do IAM você pode usar para delegar o acesso aos seus recursos da AWS a usuários, grupos ou serviços?

- [ ] IAM User
- [ ] IAM Web Identity Federation
- [x] IAM Role.
- [ ] IAM Group.

4. O que é uma policy do IAM?

- [ ] Um arquivo CSV que contém a Accesss Key e Secret Accesss Key do usuário
- [x] Um documento JSON que define uma ou mais permissões.
- [ ] Um arquivo que contém a chave SSH privada de um usuário.
- [ ] A policy que determina como sua conta da AWS será paga.

5. Qual é a melhor maneira de permitir que sua instância do EC2 leia arquivos em um bucket S3?

- [ ] Crie um novo usuário do IAM e conceda acesso de leitura ao S3. Armazene as credenciais do usuário localmente na instância do EC2 e configure seu aplicativo para fornecer as credenciais a cada solicitação de API
- [ ] Configure uma política de bucket que conceda acesso de leitura com base no nome da instância do EC2
- [x] Crie uma role do IAM com acesso de leitura ao S3 e atribua a role à instância do EC2.
- [ ] Crie uma nova role do IAM e conceda acesso de leitura ao S3. Armazene as credenciais da role localmente na instância do EC2 e configure seu aplicativo para fornecer as credenciais a cada solicitação da API.

6. Qual afirmação melhor descreve o IAM?

- [x] O IAM permite gerenciar usuários, grupos e roles e o nível correspondente de acesso à plataforma da AWS.
- [ ] O IAM permite gerenciar apenas as senhas dos usuários. A equipe da AWS deve criar novos usuários para sua organização. Isso é feito levantando um ticket.
- [ ] O IAM permite gerenciar permissões apenas para recursos da AWS.
- [ ] O IAM significa Gerenciamento Improvisado de Aplicativos e permite implantar e gerenciar aplicativos na nuvem da AWS.

# RDS

RDS facilita os processos de configuração, operação e escala de bancos de dados relacionais na nuvem.

RDS suporta os seguintes bancos: SQL Server, Oracle, MySQL Server, PostgreSQL, Aurora, MariaDB

## OLTP (Online Transaction Processing) vs OLAP (Online Analytical Processing)

Os sistemas OLAP permitem que os usuários analisem informações de banco de dados de vários sistemas de banco de dados ao mesmo tempo. OLTP administra transações do dia a dia de uma empresa. **RDS = OLTP. RedShift = OLAP**.

**Exam: architectural changes you can make in order to make your databases run faster :: Elasticache (in-memory caching).**

**Exam: when we have an EC2 instance in one security group and an RDS in another security group, what do we have to do?** 
Basicamente, abrir o RDS, clicar em Instâncias, clicar no security group, selecionar o security group utilizado pela instância, clicar na aba Inbound, botão “Edit” e criar um novo registro selecionando o banco de dados na coluna “Type” e o security group do EC2 na coluna Source. Assim, o EC2 passa a ter acesso à instância do RDS.

## RDS Backups

- **Automated backups**: permitem a recuperação do banco em qualquer ponto dentro de um período de retenção, que pode variar entre 1 e 35 dias (padrão 7 dias). 
  - Ao ativar backups automatizados para a instância de banco de dados, o Amazon RDS automaticamente realiza um snapshot diário completo dos dados (durante a janela de backup preferencial) e captura os logs de transação (à medida que as instâncias de banco de dados são atualizadas). 
  - Ao iniciar a recuperação point-in-time, os logs de transação serão aplicados ao backup diário mais apropriado para restaurar sua instância de banco de dados para o momento específico solicitado. 
  - Você pode iniciar a restauração point-in-time e especificar qualquer segundo durante seu período de retenção, até o último momento restaurável.
- **Database snapshot**: snapshots são criados manualmente pelo usuário e são salvos mesmo que a instância base seja removida. 

Quando um backup é restaurado, uma nova instância é criada no RDS, com um novo endpoint.

## Encriptação

Suportada por MySQL, Oracle, SQL Server, PostgreSQL, MariaDB e Aurora. Uma vez que a instância está encriptada, os dados dentro estão encriptados, assim como seus backups automatizados, snapshots e read replicas.

To encrypt an existing database, you need first to create a snapshot of this database, make a copy of the snapshot and encrypt the copy. 

## Multi-AZ

O RDS mantém cópias exatas das bases de produção em outras Availability zones, visando apenas disaster recovery. Cada ação tomada na instância principal são replicadas às suas cópias de maneira síncrona. Podem ser ativadas em caso de manutenções no banco de dados, falha nas instâncias dos bancos ou ainda na availability zone.

Caso ocorra algum problema com a instância principal, o RDS aponta o DNS diretamente para uma das réplicas. Portanto, é recomendado utilizar o DNS do RDS ao invés do IP da máquina principal.

Tráfego entre AZs é gratuito. Existem múltiplos AZs dentro da mesma região (no mínimo 3). 

Disponível para: MySQL Server, SQL Server, Oracle, PostgreSQL and MariaDB. (??)

## Read replicas

Permite manter cópias somente leitura das bases de produção, utilizando replicação assíncrona das operações realizadas na instância primária. É recomendado para sistemas que dependem de muita leitura de banco de dados, sendo utilizado apenas para escala.

Disponível para PostgreSQL, MySQL Server, MariaDb e Aurora.
Engine do Oracle e SQL Server não suportam read replicas.

Para utilizar, os backups automatizados precisam estar ativados. Pode-se ter até 5 cópias de somente leitura para um banco de dados, assim como pode-se criar read replicas a partir de outras read replicas, o que pode impactar na latência do serviço.

Cada read replica tem seu próprio endereço DNS, e podem haver read replicas com Multi-AZ ativado. Além disso, é possível criar read replicas de bancos com Multi-AZ.

Read replicas também podem ser “promovidas” para serem seus  bancos de dados principais. Podem haver read replicas em regiões diferentes (no MySQL e MariaDB). Uma read replica pode ser encriptada mesmo que o banco primário não seja.

**Disaster recovery = Multi-AZ**
**Performance improvement = Read replicas**

## Elasticache

É um serviço que facilita o processo de deploy, operação e escala de caches em memória na nuvem.

Melhora a performance de aplicações web permitindo a recuperação de informações a partir de caches em memória, muito mais rápidos do que depender de bancos de dados baseados em disco, por exemplo.

Pode ser utilizado para melhorar latência e throughput tanto em aplicações que dependem muito de leitura em bancos de dados (redes sociais, Q&A, etc) quanto em aplicações com cálculos muito complexos (sistemas de recomendação).

### Tipos de Elasticache

#### Memcached

- Tratado como um EC2 (escala horizontal, node replacement)
- Casos de uso:
  - Cache de objetos para aliviar o banco, o mais simples possível
  - Suporte à escala horizontal e alta velocidade de processamento (multithread)

#### Redis

- Tratado como banco de dados
- Casos de uso
  - Tipos de dados mais avançados
  - Ordenar ou ranquear datasets em memória (leaderboards, por exemplo)
  - Persistência
  - Execução em múltiplas Availability zones (Multi-AZ)

#### Exam tips

Na prova geralmente é dado um cenário com alta carga no banco de dados, perguntando qual serviço pode ser utilizado para aliviar esse estresse.

Elasticache -> muitas leituras no banco. Elasticache is a good choice when your database is particular read-heavy and not prone to frequent changing. 
RedShift -> análises realizadas no banco. Redshift is a good answer if the reason your database is feeling stress is because management keep running OLAP transactions on it

# Resumo

- RDS (Relational Data Services): OLTP
  - Suporta SQL Server, MySQL, PostgreSQL, Oracle, Aurora, MariaDB
- DynamoDB: no sql
- RedShift: OLAP
- Elasticache = Memcached ou Redis
- Disaster recovery = Multi-AZ
- Performance improvement = Read replicas
- Read replicas
  - Utilizadas para escalar, não pra disaster recovery
  - Backups automatizados precisam estar habilitados na instância do RDS
  - Pode ter até 5 read replicas de uma instância
  - Pode-se criar read replicas a partir de outras read replicas
  - Cada read replica tem seu próprio DNS
  - Read replicas podem ter o Multi-AZ ativados, assim como read replicas podem ser criadas de bases com Multi-AZ ativos
  - Read replicas podem ser “promovidas” para instâncias principais
  - Read replicas podem estar em outras regiões (MySQL and MariaDB)
- Elasticache
Na prova geralmente é dado um cenário com alta carga no banco de dados, perguntando qual serviço pode ser utilizado para aliviar esse estresse.
- Elasticache -> muitas leituras no banco
- RedShift -> análises realizadas no banco
- Memcached:
  - Salvar objetos simples, da maneira mais simples possível
  - Escala horizontal
- Redis:
  - Objetos um pouco mais complexos
  - Operações sobre os dados (ordenação, por exemplo)
  - Maior persistência (Redis é tratado como DB)
  - Multi-AZ
  - Pub/sub é necessário


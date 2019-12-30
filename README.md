# CloudWatch

- Monitoramento  
  - Possibilita, por exemplo, enviar um alerta quando exceder X dólares  
  
# IAM (Identity and Access Management)

- Controle de acesso, credenciais, etc.
- MFA (autenticação multifator)
  - Funciona com apps como, por exemplo, Duo Mobile (Dispositivo MFA virtual)
- Action (operação que pode performar) / Effect (allow ou deny) / Resource
- Já possui várias politicas padrões que pode utilizar, mas é possível criar politicas customizadas
- É recomendado criar grupos e nunca aplicar a política direto a uma pessoa usuária diretamente (sempre com o mínimo de permissões necessárias)
- É possível customizar as políticas de senha (pelo menos um número, caracteres especiais, etc)
- Usuárias criadas por aqui devem fazer o login através de outro link (ex: https://513034491943.signin.aws.amazon.com/console)

# EC2 (Elastic Compute Cloud)


## VPC (Virtual Private Cloud)
- IPs privados
- Routing
- Security groups
- Ferramenta importante para segurança
- Access Control List (filtro por ips, por exemplo)
- Pode possuir um grupo de Subnets

## Load Balancer
- Ouve uma porta (ex: um para 80 e outro pra 443)
- É criado dentro de uma VPC
- É habilitado para N availability zones
- Deve ser associado à um security group (quais ips podem acessar uma determinada porta)
- É criado um Target group (podendo ser do tipo Instance, IP ou Lambda Function)
  - Neste passo, cuidado ao preencher a porta. Deve ser a porta que a aplicação está escutando internamente
- Possui uma URL para healthcheck
- Permite fidelizar a sessão (através de um cookie) chamado de "stickiness". Fica associado ao target group.

## AMI (Amazon Machine Image)
-  É possível criar a partir de uma instância criada (EC2 -> Instances -> Actions -> Image -> Create Image)

## Auto scaling group
- Você escolhe qual imagem que será usada (para usar a sua já previamente criada, selecione "My AMIs")
- Escolhe que tipo de instância EC2 será criada (t2.micro, t2.medium, etc indicando poder computacional da máquina)

## Security group
- É possível configurar para que aceite conexões apenas através de um load baleancer previamente criado (Security group -> Inbound -> Edit -> No campo Source, selecione o security group com load balancer no lugar do IP).

## Elastic IP addresses
- É possível criar um ip estático publico e associa-lo com recurso que desejar. É possível associa-lo com outro recurso depois.

## S3 (Simple Storage Service)
- Armezana objetos (não importando o tipo)
- Permite gerenciar quem pode consulta-los
- Objetos são separados em "buckets"
  - Um bucket serve para armazenar um grupo de arquivos
  - Serve tbm para criar uma URL de acesso
  - O nome do balde precisa ser único para todo S3 (não apenas na sua conta)
- Um objeto pode ter até 5 terabytes (mas tem um limite de upload de 5Gb, para mais que isso precisa usar uma ferramenta de multipart
- Pode escolher qual região do mundo serão armazenados
- Além do arquivo, armazena também meta-dados (file type, data de modificação, etc)
- Um objeto é idenficado pela sua chave, formada pelo path e nome do arquivo, excluindo o nome do bucket (ex: images/image.png)
- É possível utilizar o IAM para dar permissões de acesso
- Existe a opção de "cross region replication" para diminuir a latência e aumentar a redundância
  - Permite a redundância para apenas uma outra região, para ter no mundo todo, use o CloudFront
- AWS Policy Generator (ferramenta para facilitar a criação de politicas de acesso)
- Copiando arquivos para um bucket via linha de comando:
  - `aws s3 cp <pasta>-s3 s3://<bucket-name>/<pasta> --recursive`
- Permite acessar os arquivos diretamente por uma URL (podendo inclusive deixar o acesso público)
- É possível habilitar CORs (por bucket)
- Existe SDKs para fazer upload diretamente para o S3

## DataBases

### RDS (Relational Database Service)
- Várias engines disponíveis: Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database e SQL Server
- Permite replicar em zonas diferentes
  - Failover automatico
- Permite criar replica da base para apenas leitura
  - Consistência eventual
  - Neste caso, não é usada como failover

### DynamoDB
- Não relacional (NoSQL)
- Schemaless
- É formado por um conjunto de tables
- Cada tabela tem um conjunto de itens.
- Cada item tem uma primary key (obrigatóriamente)
- A chave primária pode ser uma string, número ou binário
- A tabela tem um "provisioned throughput capacity"
  - Quantidade de operações de leitura/escrita por segundo (baseado em uma unidade de 4kb)
  - Se passar o limite estabelecido, a amazon pode fazer throttle ou até mesmo recusar a requisição
  
## Cloud Formation
 - Permite provisionar recursos utilizando templates (em formato json)
 - É possível, por exemplo, recriar a infrastrutura facilmente (ex: replicar todo ambiente de produção em um ambiente de dev)
 - Você pode versionar os templates no git e fazer rollback se precisar
 - CloudFormation Stack
  - Grupo de recursos AWS
  - É possivel atualizar ou deletar uma stack
  - No caso de atualizar, tentará fazer isso sem reiniciar sua instancia, se possível
 - CloudFormation Designer 
  - Ferramante "drag and drop" para criar templates
  - Permite criar do zero ou importar um template previamente criado
 - CloudFormer
  - Cria um CloudFormation template a partir da uma infraestrutura previamente criada

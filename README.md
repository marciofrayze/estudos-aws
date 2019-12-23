# CloudWatch

- Monitoramento  
  - Possibilita, por exemplo, enviar um alerta quando exceder X dolares  
  
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


# VPC (Virtual Private Cloud)
- IPs privados
- Routing
- Security groups
- Ferramenta importante para segurança
- Access Control List (filtro por ips, por exemplo)
- Pode possuir um grupo de Subnets


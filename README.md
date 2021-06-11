# SEI - Sistema Eletrônico de Informações usando docker

Fork do projeto [diogosm/sei-in-docker](http://github.com/diogosm/sei-in-docker)

## Començando:
```
git clone https://github.com/fabriciogmatos/sei.git 
```
Entre dentro da pasta do projeto e edite o arquivo: docker-compose.yml
- [ ] Edite os ARGS no servido "db" (URL, SQL, SiglaOrgaoSistema e OrgamName);
- [ ] Edite os ARGS no servido "app" (URL, FONTES, SiglaOrgaoSistema, OrgamName e db_host);

Copie o código do SEI para a pasta ./sei/codigo
Copie os dois arquiovs sqls do SEI e SIP para a pasta ./mysql/sql com os nomes :sei.sql e sip.sql

Inicie os containers:
```
docker-compose up -d
```

Acesse a aplicação
http://localhost:12200 - usuário teste e senha teste;


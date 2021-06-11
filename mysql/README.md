# Como construir a imagem do mysql?

É assumido que nesta pasta vai estar os arquivos *.sql do banco de dados do SEI e SIP conforme baixado do software público br. 

Nesse exemplo a pasta para colocar esses arquivos .sql é: `./mysql/sqls`. 

## Os Arquivos SQLS

Os arquivos *.sql do SEI e SIP deverão ter os seguintes nomes: `sei.sql` e `sip.sql`.

Algumas configurações (URLs do sei/sip, Sigla da Organização e Nome ),  serão alterados conforme os valores definidos nos `ARGS` de do service `db` do arquivo `docker-compose.yml`.

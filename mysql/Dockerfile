FROM mariadb:5.5

ARG URL
ARG SQL
ARG SiglaOrgaoSistema
ARG OrgamName

RUN apt-get update  >> /dev/null && apt-get -y install bash vim  >> /dev/null

ENV MYSQL_ALLOW_EMPTY_PASSWORD="yes"

# copia sqls
COPY $SQL/sip.sql /tmp/sip.sql
COPY $SQL/sei.sql /tmp/sei.sql
#COPY my.cnf /etc/my.cnf
COPY install.sh /docker-entrypoint-initdb.d/install.sh

RUN sed -i "s/update orgao set sigla='ABC', descricao='ORGAO ABC' where id_orgao=0;/update orgao set sigla='$SiglaOrgaoSistema', descricao='$OrgamName' where id_orgao=0;/" /docker-entrypoint-initdb.d/install.sh

RUN sed -i "s/update sistema set pagina_inicial='http:\/\/localhost\/sip' where sigla='SIP';/update sistema set pagina_inicial='http:\/\/$URL\/sip' where sigla='SIP';/" /docker-entrypoint-initdb.d/install.sh

RUN sed -i "s/update sistema set pagina_inicial='http:\/\/localhost\/sei\/inicializar.php', web_service='http:\/\/localhost\/sei\/controlador_ws.php?servico=sip' where sigla='SEI';/update sistema set pagina_inicial='http:\/\/$URL\/sei\/inicializar.php', web_service='http:\/\/$URL\/sei\/controlador_ws.php?servico=sip' where sigla='SEI';/" /docker-entrypoint-initdb.d/install.sh

RUN chmod a+x /docker-entrypoint-initdb.d/install.sh

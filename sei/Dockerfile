# @TODO:
#	Usar imagem apache+php5.6
FROM ubuntu:latest

ARG DEBIAN_FRONTEND=noninteractive

# inclusão de args
ARG URL
ARG FONTES
ARG SiglaOrgaoSistema
ARG db_host
ARG db_user
ARG db_pass
ARG db_port
ARG db_sip
ARG db_sei
ARG CacheSip
ARG JODConverter

RUN apt-get update >> /dev/null && apt-get -y install software-properties-common  >> /dev/null

## Instalacao de componentes da linguagem em pt_br
RUN apt-get -y install language-pack-pt language-pack-pt-base >> /dev/null

RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
RUN LC_ALL=C.UTF-8 add-apt-repository -y ppa:openjdk-r/ppa
RUN apt-get update && apt-get -y install \
	apache2 php5.6-common php5.6-cli \
	php-pear php5.6-bcmath php5.6-gd \
	php5.6-gmp php5.6-imap php5.6-intl \
	php5.6-ldap php5.6-mbstring php5.6-pspell \
	php5.6-snmp php5.6-xml php5.6-xmlrpc \
	php5.6-soap php5.6-fpm libapache2-mod-php5.6 \
	php5.6-mysql php5.6-json php5.6-mcrypt \
	php5.6-memcache php5.6-memcached php5.6 \
	libxext-dev libfontconfig1 libxrender1 \
	bash sed vim  >> /dev/null

RUN apt-get -y install openjdk-8-jre >> /dev/null

#RUN a2enmod php
RUN a2enmod rewrite

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

RUN chown -R www-data:www-data /var/www/
RUN chown -R www-data:adm /var/log/apache2
RUN chmod -R a+w /var/log/apache2

# Instalo o SEI
RUN mkdir -p /opt
RUN mkdir -p /opt/dados
ADD $FONTES/ /opt
RUN chown -R www-data:www-data /opt/*
## atribui permissao de execucao para pdf
RUN chmod a+x /opt/sei/web/../bin/w*
## atribui permissao de escrita no temp
RUN chmod a+w -R /opt/sei/temp/

RUN cp /opt/sei/config/ConfiguracaoSEI.php /opt/sei/config/ConfiguracaoSEI.php.ori

RUN sed -i "s/\[Servidor PHP\]/$URL/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/\[servidor BD\]/$db_host/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'Porta' => '',/'Porta' => '$db_port',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'Banco' => '',/'Banco' => '$db_sei',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'Usuario' => '',/'Usuario' => '$db_user',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'Senha' => '',/'Senha' => '$db_pass',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'UsuarioScript' => '',/'UsuarioScript' => '$db_user',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'SenhaScript' => '',/'SenhaScript' => '$db_pass',/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/'Tipo' => '')/'Tipo' => 'MySql')/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/\[Servidor Memcache\]/$CacheSip/" /opt/sei/config/ConfiguracaoSEI.php
RUN sed -i "s/\[Servidor JODConverter\]/$JODConverter/" /opt/sei/config/ConfiguracaoSEI.php

RUN cp /opt/sip/config/ConfiguracaoSip.php /opt/sip/config/ConfiguracaoSip.php.ori

RUN sed -i "s/\[Servidor PHP\]/$URL/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/\[Servidor BD\]/$db_host/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Porta' => '',/'Porta' => '$db_port',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Banco' => '',/'Banco' => '$db_sip',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Usuario' => '',/'Usuario' => '$db_user',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Senha' => '',/'Senha' => '$db_pass',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'UsuarioScript' => '',/'UsuarioScript' => '$db_user',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'SenhaScript' => '',/'SenhaScript' => '$db_pass',/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Tipo' => '')/'Tipo' => 'MySql')/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/\[Servidor Memcache\]/$CacheSip/" /opt/sip/config/ConfiguracaoSip.php

RUN sed -i "s/'Replicacao' => array(''),/'Replicacao' => array('*'),/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Pesquisa' => array(''),/'Pesquisa' => array('*'),/" /opt/sip/config/ConfiguracaoSip.php
RUN sed -i "s/'Autenticacao' => array('')),/'Autenticacao' => array('*')),/" /opt/sip/config/ConfiguracaoSip.php



# Adiciono includes
COPY sei.conf /etc/apache2
## Copio o arquivo php.ini para dentro do apache
COPY php.ini /etc/php/5.6/apache2/
RUN echo "IncludeOptional sei.conf" >> /etc/apache2/apache2.conf 
RUN echo "include_path = \".:/opt/infra/infra_php/:/opt/infra/:/opt/infra:/usr/lib:/usr/share/php\"" >> /etc/php/5.6/apache2/php.ini
RUN echo "short_open_tag = On" >> /etc/php/5.6/apache2/php.ini

## add ativacao da linguagem
ENV LANG='pt_BR.ISO-8859-1' LANGUAGE='pt_BR:pt:en' LC_ALL='pt_BR.ISO-8859-1'
RUN echo "pt_BR pt_BR.ISO-8859-1" >> /etc/locale.alias >> /dev/null
RUN echo "LANG=\"pt_BR\"" >> /etc/environment >> /dev/null
RUN echo "LANGUAGE=\"pt_BR:pt:en\"" >> /etc/environment  >> /dev/null
RUN localedef pt_BR -i pt_BR -f ISO-8859-1 >> /dev/null
RUN localedef pt_BR.ISO-8859-1 -i pt_BR -f ISO-8859-1 >> /dev/null
RUN localedef pt_BR.ISO8859-1 -i pt_BR -f ISO-8859-1 >> /dev/null

## suporte ao zip
RUN apt-get -y install zlib1g-dev php5.6-zip >> /dev/null

# Starto apache
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

EXPOSE 80


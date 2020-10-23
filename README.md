Projeto Tópicos Laravel com Zabbix

Este pacote fornece uma biblioteca de API do Zabbix para o Laravel Framework. Ele usa a classe PhpZabbixApi gerada pelo pacote http://github.com/confirm/PhpZabbixApi.

    NOTA: Esta versão foi testada apenas com Zabbix Server 3.0. *. Não tenho certeza se funciona com outras versões também.

Instalação

Para começar, você deve adicionar a dependência do Composer becker / laravel-zabbix-api ao seu projeto:

compositor requer becker / laravel-zabbix-api

O Dockerfile e os arquivos de configuração associados para um contêiner Laravel ou Lumen Docker.

Pacotes

Os seguintes pacotes estão instalados:

    Nginx
    PHP (com GD, Mcrypt, Sqlite, MySQL e Opcache)
    Redis (configurado como um cache LRU)
    NPM

Os processos são gerenciados pelo Supervisor.

Um cron job foi configurado para chamar o agendador do Laravel uma vez por minuto.

Antes de usar o Redis com Laravel, você precisará instalar o pacote predis / predis via Composer. Consulte a documentação do Laravel Redis.
Adicionando arquivos do seu site

ADICIONE / COPIE seu projeto Laravel / Lumen para o diretório / share em seu Dockerfile, ou se você não estiver estendendo este contêiner, você pode montar seu diretório local em / share como um volume.
Executar comandos do Artisan quando o contêiner é executado

Qualquer programa do Supervisor: x arquivos de configuração que você ADICIONAR / COPIAR no diretório / etc / supervisord / serão executados automaticamente quando o contêiner for iniciado. Certifique-se de que o nome do arquivo termine com .conf.
Uso para projetos não Laravel ou Lumen

Se você quiser usar este contêiner para um projeto PHP genérico, você pode montar seu projeto no diretório / share / public.

Você também deve remover o cron job do Laravel que é executado a cada minuto, adicionando o seguinte comando ao seu Dockerfile:

RUN rm /etc/cron.d/laravel

Acesso ao terminal

SSH não está configurado neste contêiner, presume-se que você obterá acesso por meio do seguinte comando:

docker exec -it YOUR_CONTAINER_NAME bash

Depois de entrar, você precisará definir o ambiente TERM, caso contrário, alguns comandos, como clear ou nano, irão falhar.

export TERM = xterm

Opcache

O PHP Opcache foi habilitado e configurado para revalidar a cada 60 segundos. Para o desenvolvimento local, você pode desejar desativar o Opcache:

docker exec -it YOUR_CONTAINER_NAME bash -c "sed -i 's / opcache.enable = 1 / opcache.enable = 0 / g' /etc/php/7.0/fpm/php.ini && sed -i 's / opcache. revalidate_freq = 60 / opcache.revalidate_freq = 0 / g '/etc/php/7.0/fpm/php.ini && supervisorctl reload "

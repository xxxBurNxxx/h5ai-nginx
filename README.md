
h5ai-nginx 
-------------

Olá, neste artigo irei descrever como configurar um indexador de arquivos moderno para servidores usando o h5ai de autoria do [larsjung](https://larsjung.de/h5ai/) e o nginx.

Segue abaixo alguns pontos importantes:
*	Não é difícil e nem complicado montar este serviço, porem tem certos pontos que atenção tem que ser dobrada parra não errar.
*	Não existe uma única maneira de fazer tudo o que está descrito aqui e provavelmente se você achou este artigo, você já deve ter visto algumas formas espalhadas pela internet.
*	Ao iniciar um tópico deste artigo, siga todos os passos até o final pois se faltar alguma etapa provavelmente lá na frente alguma coisa não vai funcionar.
*	Tudo que estiver marcado com *** tem que ter uma certa atenção.

Softwares necessários:
-------------
*	Distribuição Linux - Debian, Ubuntu, Raspibian e outras.
*	h5ai - https://larsjung.de/h5ai/
*	nginx
*	PHP

# Sumario

1. [Instalando e configurando o nginx](#1)
2. [Instalando PHP](#2)
3. [Baixando o  h5ai](#3)
4. [Testando o serviço](#4)


-----------------------------------------------------------------------------------------------------------------------------------

## 1. Instalando e configurando o nginx <a name="1"></a>


A instalação do nginx é bem simples, basta você executar o comando abaixo no terminal da sua distribuição Linux:

 	sudo apt-get install nginx -y

Execute o comando abaixo para verificar se o nginx esta instalado, o terminal deve te retornar a versão instalada.
	
		sudo nginx -v

Agora vamos alterar o arquivo de configuração do nginx para o nosso site, para isso iremos editar o arquivo de configuração do nginx com algum editor, no caso estarei utilizando o Nano.

	sudo nano /etc/nginx/sites-available/default

Com o arquivo aberto iremos alterar apenas algumas linhas.
Na linha "root ..........." iremos adicionar um # no inicio da linha para comentar ela e abaixo iremos colocar o diretório da pasta onde o site ira ficar, exemplo:

	#root /var/www/html;
	root /home/SEU_USUARIO/Documents/site-exemplo;

Mais abaixo iremos comentar a linha "index index.html ..........." e acrescentar a linha com os paramentos que precisamos:

	#index index.html index.htm index.nginx-debian.html;
	index index.html index.php /_h5ai/public/index.php;

*** E por ultimo iremos retirar os comentários de algumas linhas para ativar PHP.
Atenção a configuração deve ficar igual abaixo, caso tenha algum problema na hora de abrir o site pode ser este bloco que esteja com a versão do PHP diferente, mais abaixo você ira descobrir qual a versão foi instalada e caso necessário depois você pode ajustar.

        # pass PHP scripts to FastCGI server
        #
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
                fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
                #fastcgi_pass 127.0.0.1:9000;
        }
        
Feito todas as alterações no arquivo, você estiver usando o Nano pode pressionar **CTRL + o** para salvar, **Enter** para gravar no mesmo arquivo e **CTRL + x** para sair. Caso queira abra novamente este arquivo e verifique se salvou corretamente.

***Irei deixar no diretório desse arquivo um arquivo completo de exemplo.

## 2. Instalando PHP <a name="1"></a>

A instalação do PHP é bem simples, basta executar o comando abaixo:

	sudo apt-get install php-fpm -y

E depois executar para verificar a versão, conforme mencionei anterior mente.

	sudo php -v

## 3. Baixando o  h5ai <a name="1"></a>

Para configurar o h5ai, você pode baixar os arquivos oficiais ou baixar os que eu deixei no diretório desse artigo, extrair os arquivos e deixar a pasta **_h5ai** na raiz da pasta do seu site.

Basicamente deve ficar com uma estrutura de pasta conforme abaixo:

	site-exemplo
		     ┖-- _h5ai
		     ┖-- Documentos
		     ┖-- Fotos

## 4. Testando o serviço <a name="1"></a>

Para testar se tudo funcionou conforme primeiro precisamos descobrir qual o ip do computador e reiniciar o serviço do nginx para que ele pegue as configurações realizadas.

Para descobrir o ip do computador digite:
	
	ip address show
	      ou
	ifconfig

Depois digite os comandos abaixo para verificar o status no nginx, caso não retorne nenhum possivelmente esta tudo certo com as confiurações.

	sudo nginx -t

Digite o comando abaixo para reiniciar o serviço do nginx:

	sudo systemctl restart nginx.service
	
E digite novamente o comando abaixo para verificar se tem algum erro:

	sudo nginx -t

Feito isso e caso não tenha aparecido nenhuma mensagem de erro, basta você abrir o seu navegador e digitar o ip do seu computador ou digitar localhost.

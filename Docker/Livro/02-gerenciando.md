

## Subindo a primeira imagem

Para subir a primeira imagem é muito simples, apenas com os comando abaixo:

````
docker run -it ubuntu /bin/bash
````

Pronto o prompt de comando do ubuntu já estara pronto:

````
root@25be7ebe7e0f:/#
````

## Interagindo

Visualizando os seus containers

````
$ docker psc  
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
25be7ebe7e0f        ubuntu              "/bin/bash"         14 minutes ago      Up 14 minutes                           cranky_mahavira
````

### Parando

Dando o comando exit no container

````
exit
ou
docker stop 25
````

O Container estára desativado, podendo ser visualizado apenas com o comando

````
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
25be7ebe7e0f        ubuntu              "/bin/bash"         16 minutes ago      Exited (0) 53 seconds ago                       cranky_mahavira
````

### Re-conectando

Para inicialo novamente, apenas digite, só precisa da inicial do id

````
docker start 25
````

Pronto, e para entrar somente digite

````
docker attach 25
# ls
````

### Deletando

Para parar e remover o container é simples

````
container # exit
docker ps -a 
docker rm 25
````

## Subindo container em background

Para carregar o container em segundo plano atribuindo um nome linux1 ao container.

````
$ docker run -it -d --rm --name linux1 ubuntu:latest /bin/bash
````

Explicando parametros

- rm - Quando parado o container será deletado
- it - Executando o comando ex: /bin/bash
- d - Execute em segundo plano
- name - Atribua um nome para o container

Para conectar no container

````
$ docker atttach linux1
````

Pronto, com isso conectaremos no nosso novo container

## Carregando container com filesystem local

Subirá um container apenas para executar um comando e armazenar no diretório atual ( Variavel PWD).

````
$ docker run --rm -v ${PWD}:/myvol ubuntu /bin/bash -c "ls -lha > /myvol/lista.txt"
````

Ele irá executar o comando e depois morrer, notasse que no diretorio local foi gerado um arquivo lista.txt.

````
$ ls
$ cat lista.txt
total 56K
drwxr-xr-x   1 root root 4.0K Nov  9 00:45 .
drwxr-xr-x   1 root root 4.0K Nov  9 00:45 ..
-rwxr-xr-x   1 root root    0 Nov  9 00:45 .dockerenv
lrwxrwxrwx   1 root root    7 Oct  8 01:31 bin -> usr/bin
drwxr-xr-x   2 root root 4.0K Apr 15  2020 boot
drwxr-xr-x   5 root root  340 Nov  9 00:45 dev
drwxr-xr-x   1 root root 4.0K Nov  9 00:45 etc
drwxr-xr-x   2 root root 4.0K Apr 15  2020 home
lrwxrwxrwx   1 root root    7 Oct  8 01:31 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Oct  8 01:31 lib32 -> usr/lib32
lrwxrwxrwx   1 root root    9 Oct  8 01:31 lib64 -> usr/lib64
lrwxrwxrwx   1 root root   10 Oct  8 01:31 libx32 -> usr/libx32
drwxr-xr-x   2 root root 4.0K Oct  8 01:31 media
drwxr-xr-x   2 root root 4.0K Oct  8 01:31 mnt
drwxr-xr-x   4 root root  128 Nov  9 00:45 myvol
drwxr-xr-x   2 root root 4.0K Oct  8 01:31 opt
dr-xr-xr-x 129 root root    0 Nov  9 00:45 proc
drwx------   2 root root 4.0K Oct  8 01:34 root
drwxr-xr-x   1 root root 4.0K Oct 23 17:32 run
lrwxrwxrwx   1 root root    8 Oct  8 01:31 sbin -> usr/sbin
drwxr-xr-x   2 root root 4.0K Oct  8 01:31 srv
dr-xr-xr-x  13 root root    0 Nov  9 00:45 sys
drwxrwxrwt   2 root root 4.0K Oct  8 01:34 tmp
drwxr-xr-x   1 root root 4.0K Oct  8 01:31 usr
drwxr-xr-x   1 root root 4.0K Oct  8 01:34 var
````

## Gerenciando

Para entrar no container com ele em execução

````
$ docker exec -it b1 bash
````

## Visualizando logs

````
$ docker logs b1
````

## Montando volume e conectando diretamento

Para carregar um container já entrando no diretorio digite apenas o comando abaixo descrito

```
> docker run -it --rm -v ${PWD}:/myfiles -w /myfiles --name meu-primeiro-script php:7.2-cli /bin/bash 
```

Ele irá carregar o container, mapeando para dentro do container o diretorio local onde vc atualmente se encontra para o diretorio dentro do container "**/myfiles**" e o parametro -w irá para dentro do diretório.

Neste exemplo usando um container em php para carregamento de uma pagina

```
 /tmp/renata ----------------------------------------------------------------- INT | 18:56:29 
> echo '<? echo "test text \n";' > index.php       
 /tmp/renata ----------------------------------------------------------------------- 18:57:15 
> vim inde
 /tmp/renata ----------------------------------------------------------------- INT | 18:57:17 
> docker run -it --rm -v ${PWD}:/myfiles -w /myfiles --name meu-primeiro-script php:7.2-cli php index.php
test text 
```

## Container segundo plano

Simples, com parametro -d voce pode subir o container sem qualquer interação

```
 /tmp/renata ----------------------------------------------------------------------- 18:59:12 
> docker run -d httpd
Unable to find image 'httpd:latest' locally
latest: Pulling from library/httpd
bb79b6b2107f: Already exists 
26694ef5449a: Pull complete 
7b85101950dd: Pull complete 
da919f2696f2: Pull complete 
3ae86ea9f1b9: Pull complete 
Digest: sha256:b82fb56847fcbcca9f8f162a3232acb4a302af96b1b2af1c4c3ac45ef0c9b968
Status: Downloaded newer image for httpd:latest
9c317eeb78f53684b4e6404b132211c06b1559506c455fdbdf02ffef15dc902c
 /tmp/renata ----------------------------------------------------------------- 10s | 18:59:27 
> docker ps          
CONTAINER ID        IMAGE               COMMAND              CREATED             STATUS              PORTS               NAMES
9c317eeb78f5        httpd               "httpd-foreground"   12 seconds ago      Up 9 seconds        80/tcp              gracious_lichterman
```





Continue... [Dockerfile](03-entendendo-dockerfile.md)


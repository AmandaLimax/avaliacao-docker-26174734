# Respostas · Avaliação Prática de Docker · ViaSerra Transportes (Turma C)

Nome: Amanda Gabrielli Ferreira de Lima
Matrícula: 26174734
Usuário do GitHub: AmandaLimax
Usuário do Docker Hub: limaamanda

Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?
Resposta: eu utilizei a imagem base `nginx:1.27-alpine`. O tamanho final da imagem ficou em aproximadamente 15.5 MB

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.
   Resposta: O Nginx procura os arquivos em `/usr/share/nginx/html`.
Para conferir o conteúdo dentro do container, usei o comando:
`docker exec -it <nome_do_container> ls -la /usr/share/nginx/html`


## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
Resposta: Nome da imagem: `limaamanda/viaserra-portal:1.0-26174734`
Link público: https://hub.docker.com/r/limaamanda/viaserra-portal


4. Se você mudar o HTML, quais comandos precisa rodar para que a versão nova chegue ao Docker Hub?
Resposta: Eu precisaria reconstruir a imagem com o build e depois fazer o push para o registro:
1. `docker build -t limaamanda/viaserra-portal:1.0-26174734 ./portal`
2. `docker push limaamanda/viaserra-portal:1.0-26174734`


## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| #   | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
| --- | --------- | ------------------- | ------------------------ | ------------- |
| 1   |           |          Escrevi abaixo           |                          |               |
| 2   |           |                     |                          |               |
| 3   |           |                     |                          |               |

| 1 | WORKDIR | Estava definido como `/usr/share/nginx` | O Nginx não encontrava a pasta padrão do site (`/html`) para servir os arquivos | Removi a instrução WORKDIR para manter a estrutura original do Nginx |
| 2 | COPY | Faltava a instrução `COPY` para carregar o site | A página de manutenção ficava em branco ou exibia a tela padrão do Nginx sem o conteúdo | Adicionei a instrução `COPY site/ /usr/share/nginx/html/` |
| 3 | EXPOSE | A porta HTTP não estava declarada no Dockerfile | O container subia sem documentar explicitamente a porta de escuta da aplicação | Adicionei a instrução `EXPOSE 80` ao final do Dockerfile |

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?
Resposta: O formato `-p HOST:CONTAINER`, o primeiro número é a porta da máquina local (Host) e o segundo é a porta interna do container.
- Em `-p 7042:80`, acessamos o site na nossa máquina pela porta `7042`, redirecionando para a porta `80` do container.
- Em `-p 80:7042`, acessamos pela porta `80` da máquina apontando para a porta `7042` do container (o que daria erro se o Nginx estivesse escutando na 80).
O número da direita (`80` no primeiro caso) é a porta do container.


## Parte 4 · Primeiro docker-compose

7. Escreva os dois comandos `docker run` que fariam o mesmo que o seu `docker-compose.yml`.
Resposta: Portal:
`docker run -d --name portal -p 8034:80 --restart unless-stopped limaamanda/viaserra-portal:1.0-26174734`
         Manutenção:
`docker run -d --name manutencao -p 7034:80 --restart unless-stopped manutencao:26174734`


1. Qual comando derruba os dois containers de uma vez?
Resposta: O comando seria `docker compose down`

## Verificador

9. Código de conclusão impresso pelo verificador:

VIASERRA-26174734-6375AA4A

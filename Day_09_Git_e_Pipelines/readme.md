# Resumo do conteúdo Day 9 - Git e Pipelines

## **Git e GitHub**:
- O Git é um repositório de código distribuído;
- Possui um esquema de repositório interno, ele cria o repositório na máquina local;
- Na nossa máquina temos que ter instalado o binário do `git` que possui alguns comandos:
    - `git add`: Pega o arquivo(s) modificado(s) e adiciona na stage;
    - **Stage**: Etapa anterior ao `git commit`, onde as alterações nos códigos ficam para serem commitados.
    - `git commit`: Pego o que está no **Stage**, e salvo as mudanças em uma versão no repositório local.
        - `git commit -m "Mensagem sobre o commit"`: Com o `-m ""` passo uma mensagem para esse commit
    - `git push`: Ele pega o repositório e sincroniza com o repositório remoto (GitHub, GitLab, etc.)
    - `git pull`: Pego as alterações realizadas no repositório remoto e trago para o meu repositório local.

- Aprendendo mais comandos com o git:
    - `git init`: Vai iniciar no diretório em que esse comando foi passado um repositório git local;
    - Criado o repositório no GitHub/GitLab, vamos fazer o link entre o nosso repositório git local e o nosso GitHub remoto:
        - `git remote add origin git@github...`: Ele consigura no nosso repositório local qual o nosso repositório remoto.
        - `git remote -v`: Ele apresenta se a configuração foi realizada, apresentando o repositório remoto que o código subirá.
        - `git branch`: Para verificar em qual branch estamos.
        - `git push -u origin main`: O `push` vai fazer o envio do que está commitado para a branch main;

- Clonando:
    - Podemos fazer `git clone` para clonar o repositório;
        - 

- GitHub é uma das plataformas possíveis para repositório remoto;

Temos que fazer uma configuração de segurança para garantir que quem está tentando acessar o repositório remoto possui as permissões para acessar esses códigos. A melhor forma é utilizando a chave-pública SSH.

Criptografia assimétrica: Você gera duas chaves, a chave privada e a chave pública.
A chave pública é utilizada publicamente, qualquer pessoa possui a chave pública

- Chave Pública
- Chave Privada

SSH é um protocolo de comunicação que permite conectar máquinas; Por exemplo, minha máquina local à um servidor remoto. 

O GitHub usa o protocolo SSH utilizando o esquema de chave pública/privada para acessar ao repositório. Na máquina local geramos as chaves e manualmente colocamos a chave pública no GitHub.

Comando para gerar as chaves:
- `ssh-keygen`
    - Pegamos a chave pública gerada e salvando no `Add new SSH Key` nas configurações do GitHub.

Outras configurações que temos que realizar:
- `git config --global user.name "Meu Nome"`
- `git config --global user.email "meu.email@gmail.com"`

## **Branch:**
A ideia da branch é criar a possibilidade de ramificações do código, podendo criar uma realidade paralela "o multiverso do Git" onde trabalhamos em outra branch paralela a nosso branch principal (`main`), e depois podemos fazer um `merge` e mesclar com a branch `main`.

- `git checkout -b feat/nova-branch`: Criando com o `checkout -b` a nova branch.
    - Podemos fazer nossas alterações, e fazer o push para a branch que acabamos de criar.
    - No GitHub, podemos abrir um PR (Pull Request), que abrirá a possibilidade de faze o merge com a branch main.
        - Esse processo de PR é revisado por alguém que aprova ou não. Sendo aprovado, podemos fazer o merge.

## **Pipelines:**
Dentro de um processo de pipeline, tudo que entrar vai sofrer as mesmas modificações/transformações. Não necessariamente será para colocar um código em PRD, pode ser utilizado para realizações de testes por exemplo. É a automatização de um processo e garantia de qualidade (o que entrou na esteira está correto?).

Todos os pipelines vão utilizar o termo "job". Job é um conjunto de passos (agrupamento de etapas) que são enviados para o Servidor responsável pelo pipeline que por padrão rodam em paralelo, mas também podem funcionar como um DAG (Directly Acyclic Graph) que é acionada por um trigger (que pode ser p.e. quando houver um push para a main).

## **Github actions:**
Doc do GitHub Actions: 

- Dentro do GitHub temos o conceito de Workflows (cada workflow é um pipeline), que são tratados dentro de um diretório chamado `.github/workflows` no repositório. 

- Outro conceito do GitHub Actions são os Actions, algo como uma série de steps pré-definidas, com códigos automáticos que podemos utilizar.

`.github/workflows/testando-terraform.yml`
```yml
# Documentação
name: testando-modul-terraform
run-name: ${{ github.actor }} inicitou o modulo terraform

# on: é o trigger
on: 
    # executar a partir de um pull_request
    pull_request:
    

# Tudo que está aqui dentro faz parte do job
# Por default roda paralelo, se tiver o 'needs' ele cria dependências;
jobs:
    # Nome do job 'tfsec'
    tfsec:
        # definição do runner
        # rodar dentro do ubuntu mais atual possível
        runs-on: ubuntu-latest
        # São os passos do job
        steps:
            # uses são os actions em si
            - name: baixando codigo
              uses: actions/checkout@v4
            # para executar um comando
            # comando está sendo passado na pasta raiz do repositório
            - name: instalando tfsec
              run: curl -s https://do-tfsec
            - name: executando tfsec
              run: tfsec .
```

Poderíamos utilizar também o Actions do `tfsec`:
```yml
...
        steps:
            - name: tfsec
              uses: aquasecurity/tfsec-pr-commenter-actions@v1.2.0
              # variável para passar ao uses acima
              with:
                # cuidado ao passar o github_token, só passe se confiar muito.
                github_token: ${{ github.token }}
```








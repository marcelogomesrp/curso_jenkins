# Aula 02 - Integração do Jenkins com GitHub
[Videoaula](https://youtu.be/Q3OUjKnjR28)

## Configurar o pipeline do Jenkins utilizando um repositório publico

Em Pipeline, preencha da seguinte forma.

- Definition
    - Selecione "Pipeline script from SCM"
- SCM
    Selecione "Git"
- Repository URL
    - Colar a url (HTTPS) do seu repositório, a que se parece com o exemplo a seguir https://github.com/USUARIO/REPOSITÓRIO.git
- Credentials
    - Mantenha " - none - ".
- Brach Specifier (blanck for 'any')
    - Informe o nome */ seguido do nome da sua branch no github, exemplo "*/main"
- Repository browser
    - Mantenha (Auto)
- Script Path
    - Colocar o caminho para o seu arquivo com o pipeline, considerando que ele esta na raíz diretório do seu repositório.



## Gerar a chave
```
ssh-keygen -t ed25519 -C "seuemail@gmail.com"
```

Em ls ~/.ssh/ ele vai gerar dois arquivos:
- id_ed25519.pub
- id_ed25519

O arquivo id_ed25519.pub é sua chave publica e o id_ed25519 é a sua chave privada.


## Configura a chave no GitHub

Logado com a sua conta no github, vá em "Settings" -> "SSH and GPG keys" [link direto](https://github.com/settings/keys)

Clique no botão "New SSH key", adicione um "Title" (um nome para identificar esta chave depois) e em Key o conteúdo de sua chave publica "id_ed25519.pub" e clique em "Add SSH key"


## Configurar a chave no Jenkins

Vá em "Manage Jenkins" -> "Manage Credentials" -> "Global credentials (unrestricted)" -> "Add Credential" [link direto](http://localhost:8080/credentials/store/system/domain/_/newCredentials)

- Kind
    - Selecione "SSH Username with private key"
- Scope
    - Mantenha "Global(Jenkins,nodes, items,all child items, etc)
- ID
    - Mantenha em branco
- Description
    - Preecher o campo "Description" com um nome para identificar esta chave depois.
- Username
    - Mantenha em branco
- Private Key
    - Selecione "Enter directly" e clique em "Add" e copie e cole o conteúdo da sua chave privada (id_ed25519)
- Passphrase
    - Mantenha em branco
Clique em "Ok"

## Configurar o pipeline do Jenkins utilizando um repositório privado

Em Pipeline, preencha da seguinte forma.

- Definition
    - Selecione "Pipeline script from SCM"
- SCM
    Selecione "Git"
- Repository URL
    - Colar a url (SSH) do seu repositório, a que se parece com o exemplo a seguir git@github.com:usuário/repositório.git
- Credentials
    - Selecione o Description da sua chave.
- Brach Specifier (blanck for 'any')
    - Informe o nome */ seguido do nome da sua branch no github, exemplo "*/main"
- Repository browser
    - Mantenha (Auto)
- Script Path
    - Colocar o caminho para o seu arquivo com o pipeline, considerando que ele esta na raíz diretório do seu repositório.


## Configurar o pipeline do Jenkins utilizando usuário e senha

Por usuário e senha não funciona mais. 
```
Failed to connect to repository : Command "git ls-remote -h -- https://github.com/marcelogomesrp/annotSV.git HEAD" returned status code 128:
stdout:
stderr: remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: Authentication failed for 'https://github.com/marcelogomesrp/annotSV.git/'
```


## Configurar o pipeline do Jenkins utilizando token

### No GitHub
Va em: "Settings" -> "Developer settings" -> "Personal access tokens" -> "Generate new token" 

- Note
    - Colocar algum nome para facilitar sua identificação
- Expiration
    - Escolha o tempo, conforme sua conveniência
Clique em "Generate token"

Copia o token.

### No Jenkins

#### Gerar a credencial
Vá em "Manage Jenkins" -> "Manage Credentials" -> "Global credentials (unrestricted)" -> "Add Credential" [link direto](http://localhost:8080/credentials/store/system/domain/_/newCredentials)

- Kind
    - Selecione "Username with password"
- Scope
    - Mantenha "Global(Jenkins,nodes, items,all child items, etc)
- Username
    - Mantenha em branco
- Password
    - Cole o seu token
- Id 
    - Mantenha em branco
- Description
    - Preecher o campo "Description" com um nome para identificar esta chave depois.

Clique em "Ok"

#### No Pipeline

Em Pipeline, preencha da seguinte forma.

- Definition
    - Selecione "Pipeline script from SCM"
- SCM
    Selecione "Git"
- Repository URL
    - Colar a url (HTTPS) do seu repositório, a que se parece com o exemplo a seguir https://github.com/USUARIO/REPOSITÓRIO.git
- Credentials
    - Seleciona a sua credencia do token.
- Brach Specifier (blanck for 'any')
    - Informe o nome */ seguido do nome da sua branch no github, exemplo "*/main"
- Repository browser
    - Mantenha (Auto)
- Script Path
    - Colocar o caminho para o seu arquivo com o pipeline, considerando que ele esta na raíz diretório do seu repositório.
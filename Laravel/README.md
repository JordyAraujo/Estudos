# Iniciando um projeto Laravel

A maneira mais rápida de iniciar um projeto Laravel é utilizando o Composer. Assim, não precisamos instalar a biblioteca Laravel na máquina, pois o próprio Composer baixa e instala todas as dependências ao criar o projeto. Certifique-se de ter o PHP e o Composer instalados e funcionando no seu computador. Utilize os seguintes comandos para checar:

## Composer

```shell
user@PC:~$ composer -V
```

Caso esteja instalado corretamente, retornará algo parecido com o seguinte:

```shell
Composer version 2.1.6 2021-08-19 17:11:08
```

[<img src="https://img.shields.io/badge/Instalador Composer-Windows-blue?style=social&logo=composer" height="50"/>](https://getcomposer.org/Composer-Setup.exe)

## PHP

```shell
user@PC:~$ php -v
```

Em caso positivo, a resposta será parecida com isto:

```shell
PHP 8.0.9 (cli) (built: Jul 29 2021 14:12:19) ( ZTS Visual C++ 2019 x64 )
Copyright (c) The PHP Group
Zend Engine v4.0.9, Copyright (c) Zend Technologies
```

### [<img src="https://img.shields.io/badge/Instalador XAMPP-PHP%208-blue?style=social&logo=php" height="50"/>](https://www.apachefriends.org/xampp-files/8.0.11/xampp-windows-x64-8.0.11-2-VS16-installer.exe)

> O XAMPP é um ambiente de desenvolvimento PHP. Seu instalador configura uma distribuição Apache com PHP e MySQL já configurados para fácil uso.

Com tudo instalado e configurado, podemos iniciar um projeto Laravel. O comando abaixo utiliza o Composer para baixar as dependências necessárias e iniciar um projeto Laravel configurado e pronto para rodar.

```shell
user@PC:~$ composer create-project laravel/laravel app-exemplo
```

Neste comando, chamamos a função *create-project* do Composer, dizendo que vamos utilizar a biblioteca do Laravel para criar o app *app-exemplo*. Com isso, uma pasta chamada *app-exemplo* será criada no diretório atual. Para testar o funcionamento da aplicação recém criada, dê um *cd* para dentro do diretório e execute o seguinte comando:

```shell
user@PC:~$ php artisan serve
```

Este comando irá executar a aplicação e colocá-la para rodar na porta 8000 do seu sistema. Portanto, acesse *localhost:8000* para ver o resultado.

Todas as configurações do Laravel estão localizadas na pasta *./config/*, mas o Laravel basicamente não precisa de nenhuma configuração adicional para rodar como é gerado. Você pode acessar, por exemplo, configurações de local e fuso horário no arquivo *./config/app.php*.

Para acessar variáveis de configuração, utilize
```php
config('arquivo.variavel', 'default')
```
Onde *arquivo* é o nome do arquivo, na pasta config, onde está *variavel*, que representa o valor que estamos querendo buscar. Caso esta variável não esteja definida, o valor de *default* será retornado.

Ainda sobre configurações, na pasta raiz do projeto foi criado um arquivo *.env* que define as variáveis de ambiente utilizadas pela aplicação, como configurações de banco e email. Este arquivo deve ser adicionado ao seu *.gitignore* pois **não deve ser commitado**, pois guarda informações sensíveis do sistema. Uma boa prática é manter um arquivo *.env-example* como base, que é cópia do *.env*, mas sem as informações sensíveis.

Acessar variáveis de ambiente é bem parecido com as de configuração:
```php
env('VARIAVEL', 'default')
```
Onde *VARIAVEL* representa o valor que estamos querendo buscar. E *default* é o valor padrão, caso ela não esteja setada.

# Clonando e executando um projeto Laravel

Ao clonar um projeto Laravel, devemos nos atentar a alguns detalhes para que possamos rodar o projeto, uma vez que algumas configurações são sensíveis e não devem estar no controle de versão (git) e outras devem ser feitas de acordo com o ambiente de execução.

## Instalação de dependências

Quando iniciamos o nosso projeto usando o Composer, eu disse que ele ficou encarregado de baixar e configurar as dependências e bibliotecas utilizadas no nosso projeto. Tudo o que foi baixado fica numa pasta chamada *vendor*, localizada na raiz do projeto e não deve ser commitada, pois pode ser facilmente criada e sua presença só resulta no consumo desnecessário de espaço no nosso repositório. Basicamente estaremos subindo uma pasta pesada e cheia de arquivos que podemos recuperar a qualquer momento.

Pois bem, quando clonamos o repositório, essa pasta não existirá, então devemos criá-la com um simples comando:

```shell
user@PC:~$ composer upgrade
```

Este comando irá procurar pela pasta *vendor*. Caso ela não exista, será criada, instalando todas as dependências necessárias pro projeto (estas ficam salvas no arquivo *composer.lock* na raiz).

## .env

Em primeiro lugar, já sabemos que o arquivo *.env* não deve ser commitado, o que implica que ele não virá com o projeto quando clonarmos o repositório. Porém, como foi explicado, é uma boa prática manter um arquivo de exemplo. Portanto, essa é a hora de renomear esse arquivo para *.env* e preencher as informações necessárias que estarão faltando.

## Migrations

Migrations são arquivos utilizados para fazer controle de versão da base de dados. Para que não precisemos criar um banco na mão sempre que baixarmos o projeto, ou para que não ocorram conflitos ao alterar manualmente suas tabelas.

Portanto, quando clonamos um projeto que possui banco de dados, consideramos que suas migrations foram escritas corretamente, precisamos rodar suas migrations para criar a estrutura (o banco precisa estar criado localmente na sua máquina e setado no arquivo *.env*).

Para isso, basta rodar o seguinte comando:

```shell
user@PC:~$ php artisan migrate
```

## Arquivos públicos

Os arquivos salvos localmente em um sistema Laravel, como o upload de fotos de perfil, por exemplo, ficam na pasta *storage*. Porém, por questões de segurança, o Laravel apenas expõe publicamente os arquivos salvos na pasta *public*.

Portanto, para que possamos acessar os arquivos salvos no sistema pelos usuários, precisamos criar um link simbólico dentro da pasta *public*, que aponte para a pasta em *storage*.

Para facilitar nosso trabalho, o Laravel já possui um comando que faz esse trabalho pra nós: 

```shell
user@PC:~$ php artisan storage:link
```

Este comando cria um link da pasta *./storage/app/public* para a pasta *./public/storage*, tornando estes arquivos visíveis.

## Chave do projeto

Todo projeto Laravel possui uma variável em seu *.env* que define uma chave privada para a aplicação. Esta chave será usada, entre outras coisas, para gerar criptografia única no sistema.

Ao criarmos o projeto com o Composer, ele já cria a chave e salva no *.env*. Porém, caso a chave não esteja salva no *.env* de exemplo no seu repositório, será necessário criá-la. 

Mais uma vez, o Laravel já possui um comando que irá nos auxiliar com esta tarefa. Ele cria a chave e seta a mesma no arquivo *.env* (que já deve existir neste momento):

```shell
user@PC:~$ php artisan key:generate
```

# Rotas


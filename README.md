# Laravel
Estudando laravel

## 1. Banco de Dados
Configuração do banco de dados e feita no arquivo .env
Onde é preenchido dos os dados do banco.
Ex:
DB_CONNECTION=mysql
DB_DATABASE= nome do banco
DB_USERNAME= usuário
DB_PASSWORD= senha  de acesso (se tiver) no mysql

-> Caso queira usar o sqlite precisamos:
Apaguar todas deixando apenas DB_CONNECTION e citar o banco sqlite
Ex:
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite    -> Adicionar o caminho onde será criado o aquivo database.sqlite

-> A criação do arquivo database.sqlite será feita dentro da pasta Database (esse arquivo será onde irá armazernar os dados)

-> Feito isso, já temos o bastante para começar a criar a tabela. Para isso, vamos utilizar o artisan:
Ex:
  php artisan make:model Models\\Customer -m




### 1.1. Como comunica
por default(padrão) o sinprocesso/laravel já vem conectado ao banco....
### 1.2. Como criar migração
migrate: Com este recurso é possível criar as tabelas, alterar a estrutura, definir a ordem de criação de cada tabela e fazer relacionamentos entre elas.
-> Os arquivos de migrations (estrutura das tabelas) ficam em database/migrations/
-> Por padrão o Laravel já traz duas migrations, você pode abrir os arquivos
Ex:
- (número gerado)create_users_table.php  -> tem a estrutura padrão da tabela users (utilizada para fazer login no sistema).

- (número gerado)create_password_resets_table.php   -> contém a estrutura da tabela password_resets, utiliza para resetar a senha do usuário.
* Obs: A ordem de criação tabelas é definida pelo nome do arquivo, ou seja, neste caso a tabela users é criada antes da tabela password_resets. O sistema de Migration do Laravel mantém a data de criação no mome do arquivo para ordenar de forma correta, sendo assim não gera erros no momento de criar uma tabela quem contém relacionamento com outra tabela.


1. descobrir se há comando laravel para criar migrations

### Criar novas Migrations:
As Migrations foram criadas no Laravel para o uso do controle de versão e o compartilhamento das alterações de um BD entre os desenvolvedores de uma equipe. Com uma Migration, é possível construir ou modificar as tabelas do seu banco sem a necessidade de realizar isso manualmente no phpMyAdmin.


-> O comando: (php artisan migrate) vai criar a tabela users exatamente com a estrutura definida
Ex:
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
}

- Caso precise criar um novo arquivo de migration para alterar a estrutura da tabela users, adicionando novas opções rode este comando:
-> php artisan make:migration add_name_field_table_name --table=users
- Após rodar este comando vai gerar um novo arquivo, mais ou menos com este mesmo nome (apenas timestamps diferente).
Agora que já criamos o arquivo de migration que vai atualizar a estrutura da tabela users, podemos rodar o comando para atualizar a tabela: php artisan migrate


### Criação de um do model e migrate:
Ex:
php artisan make:model NomeDaModel -m
Uma model então é criada. Além disso, é criado uma migration com o mesmo nome da model no caminho (\database\migrations).
### Criar somente um model:
php artisan make:model NomeDaModel

### Criar somente a Migration execute o comando:
php artisan make:migration NomeDaMigracao


##### Pode criar as migrations (tabelas) de duas formas:
I -> php artisan make:migration create_products_table
Assim gera um arquivo de migration apenas, com um nome semelhante a isso:
database/migrations/ano_mes_dia_timestamps_create_products_table.php

É possível criar também a partir da criação de um Model:
II -> php artisan make:model Product -m
A opção menos -m indica que é para criar o Model e também o arquivo de migration correspondente, neste caso vai criar um arquivo de migration semelhante ao último exemplo.
Com isso gera uma tabela simples onde podemos complementa-las com os campos que precisaremos definindo todos os campos:
Ex:
public function up()
{
    Schema::create('media', function (Blueprint $table) {
        $table->bigIncrements('id');
        $table->morphs('model');
        $table->string('collection_name');
        $table->string('name');
        $table->json('responsive_images');
        $table->unsignedInteger('order_column')->nullable();
        $table->nullableTimestamps();
    });

Depois de definirmos os campos na tabela podemos rodar a migrations:
  php artisan migrate

### Método UP
  - Responsável pela implementação das atualizações do banco, criar uma tabela, atualizar uma coluna, etc.

### Método Down
  - O inverso da função UP. Reverte o banco de dados ao estado anterior a esta atualização

### Models:
As Models são classes responsáveis pela administração dos dados. Esla são inseridas no diretório \app

### Obs:
- Após criar suas migrations com os dados de interesse e fazer sua conexão como banco, utilize o comando "php artisan migrate" no terminal para construir as tabelas.
### Caso houver erro na execução da migrate deverá fazer esse passo:
- Caso durante a criação das tabelas apareça o seguinte erro: "SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes"

- Será preciso acessar o arquivo: AppServiceProvider no caminho App/Providers:
\Illuminate\Support\Facades\Schema::defaultStringLength(191);

Vai ficar assim:

    public function boot()
    {
        \Illuminate\Support\Facades\Schema::defaultStringLength(191);
    }

Logo depois, é necessário apagar todas as tabelas da base de dados e executar novamente o comando "php artisan migrate" de acordo com o passo 3 para construir novamente as tabelas.

Logo depois, é necessário apagar todas as tabelas da base de dados e executar novamento o comando php artisan migrate





1. caso não exista comando, copiar e colar as existentes mudando o nome do arquivo atualizando o timestamp e mudando o conteúdo do arquivo para criar a estrutura no banco de dados
1. como é o padrão de nomenclatura de campos e tabelas usados no laravel
1. como escrever os tipos e tamanhos dos campos no laravel migration

## 2. CRUD
### 2.1. Modelo
Models são estruturas de banco de dados (tabelas)
1. como criar os atributos/campos do modelo para o banco de dados e prover os get/set para o controller
### 2.2. Controlador (controller)
Ao invés de definir todos a lógica das suas requisições em um único arquivo routes.php, você pode desejar organizar este comportamente usando classes Controllers ou Controlador. Controladores podem agrupar solicitações HTPP relacionadas a manipulação lógica de uma classe. Controladores são normalmente localizados no diretório app/Http/Controllers.
### Gerar um Controller
-> php artisan make:controller PostsController --resource

Esse comando irá gerar um arquivo chamado PostsController.php que terá por padrão todas as funções de CRUD e essa será sua única responsabilidade. Nesse exemplo o controller será responsável por criar, atualizar, excluir e listar as publicações.

### Single Action Controller
Criar um controller de uma única ação
  php artisan make:controller FavoritePost --invokable
Este só será responsável por uma e somente uma funcionalidade do sistema.
Será gerado um controller que utiliza o método mágico invoke do php que será executado imediatamente quando for chamado. Para adicionar uma rota ao controller é simples:
Ex:
Route::post(‘favorite/{post}’, ‘FavoritePost’);



1. como criar rota para a página
1. como criar os metodo do crud no controller (index, create, update, delete, new, edit....)
### 2.3. Interface Gráfica (view)
1. como é feito o html no laravel (blade)
1. como dizer qual arquivo html/blade pertence a uma action (metodo) do controller

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

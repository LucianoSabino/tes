# Iniciando

Primeiros passos

### Inicializando

`yarn init -y`

-   Fazer a instalação do Typescript na pasta do projeto, como uma dependência de desenvolvimento. Como o código final é convertido para javascript antes de ser disponibilizado online, só precisaremos do Typescript em ambiente de desenvolvimento. `yarn add typescript ts-node-dev @types/node tsconfig-paths -D`

-   Criar o arquivo "tsconfig.json" que conterá as configurações do Typescript, com o comando: `npx tsc --init --rootDir src --outDir build --esModuleInterop --resolveJsonModule --lib es6 --module commonjs --allowJs true --noImplicitAny true`
    Em resumo, os parâmetros passados são:

    _rootDir_: É aqui que o TypeScript procura nosso código.

    _outDir_: Onde o TypeScript coloca nosso código compilado.

    _esModuleInterop_: Se estiver usando commonjs como sistema de módulo (recomendado para aplicativos Node), então esse parâmetro deve ser definido como true.

    _resolveJsonModule_: Se usarmos JSON neste projeto, esta opção permite que o TypeScript o use.

    _lib_: Esta opção adiciona tipos de ambiente ao nosso projeto, permitindo-nos contar com recursos de diferentes versões do Ecmascript, bibliotecas de teste e até mesmo a API DOM do navegador. Usaremos recursos es6 da linguagem.

    _module_: commonjs é o sistema de módulo Node padrão.

    _allowJs_: Se você estiver convertendo um projeto JavaScript antigo em TypeScript, esta opção permitirá que você inclua arquivos .js no projeto.

    _noImplicitAny_: Em arquivos TypeScript, não permita que um tipo seja especificado inexplicitamente. Cada tipo precisa ter um tipo específico ou ser declarado explicitamente any.

-   Arquivo .gitignore
    .idea/
    .vscode/
    node_modules/
    build/
    temp/
    .env
    coverage
    ormconfig.json
    dist

    uploads/\*
    !uploads/.gitkeep

-   Executar o servidor em desenvolvimento
    Usaremos a biblioteca ts-node-dev para execução da aplicação em desenvolvimento.
    Incluir o script para rodar o ts-node-dev no arquivo package.json.

    _"scripts": { "dev": "ts-node-dev --inspect --transpile-only --ignore-watch node_modules src/server.ts" }_
    Executar o servidor:
    Fazer um escripit basico com o console.log
    `yarn dev`

### EditorConfig

-   O Editor Config é uma ferramenta que auxilia na padronização da configuração para vários desenvolvedores trabalhando em um mesmo projeto, mas em diferentes editores de código ou IDE's.

-   Instalar no VSCode a extensão EditorConfig for VS Code. Depois de instalada, ao clicar com o botão direito sobre o explorador de arquivos do projeto vamos selecionar a opção Generate .editorconfig. E a execução dessa opção deve gerar um arquivo .editorconfig com o seguinte conteúdo:

## Feramentas de endentação

### ESLint

ESLint é um linter JavaScript que permite que você aplique um conjunto de padrões de estilo, formatação e codificação para sua base de código. Ele examina seu código e avisa quando você não está seguindo o padrão que definiu.

-   Olhar a documentação0
    \_ Executar esse comando ``npm init @eslint/config@latest`

### Prettier

Prettier é um formatador de código opinativo e, em conjunto com o ESLint, forma uma parceria perfeita para nós, desenvolvedores.

\_ Ver a documentação

Colocar no arquivo _.prettierrc_
{
"semi": true,
"trailingComma": "all",
"singleQuote": true,
"printWidth": 80,
"arrowParens": "avoid",
"editor.formatOnPaste": true,
"editor.formatOnType": true,
"formattingToggle.affects": ["formatOnSave"]
}

### Type ORM Prisma

Agora, inicialize o TypeScript:

_npx tsc --init_

Em seguida, instale o Prisma CLI como uma dependência de desenvolvimento no projeto:

_npm install prisma --save-dev_

Por fim, configure o Prisma ORM com o initcomando do Prisma CLI:

_npx prisma init --datasource-provider postgresql_

-   No arquivo .env fica a configuração de conexão ao banco de dados.
    `DATABASE_URL="postgresql://projeto:test@localhost:5436/mydb?schema=public"`

        postgresql:// : Indica que estamos nos conectando a um banco de dados PostgreSQL. É como um protocolo que o seu aplicativo usa para "falar" com o banco.
        projeto: É o nome do usuário utilizado para se autenticar no banco de dados. Pense nele como seu login.

        test: É a senha associada ao usuário "projeto". É a chave que permite o acesso ao banco. Importante: Evite usar senhas simples em ambientes de produção. Utilize senhas fortes e considere usar ferramentas de gerenciamento de senhas.

        localhost: Indica que o banco de dados está rodando na mesma máquina que você está utilizando. Se o banco estivesse em outro servidor, você usaria o endereço IP ou nome de domínio desse servidor.

        5436: É a porta em que o servidor PostgreSQL está ouvindo por conexões. A porta padrão para PostgreSQL é 5432, mas você pode configurá-la para usar outra porta.

        mydb:É o nome do banco de dados específico que você deseja acessar. Pense nele como um contêiner onde você armazenará seus dados.

        ?schema=public:
        schema: Especifica o esquema padrão que será utilizado para criar e consultar objetos (tabelas, views, etc.). Um esquema é como um namespace que organiza os objetos do banco de dados.

        public: É o nome de um esquema padrão que já existe na maioria das instalações do PostgreSQL. Ele é usado para armazenar objetos que não pertencem a nenhum esquema específico.`

-   Agora so colocar a relação da sua tabela no prisma/schema.prisma exemplo:

    // Aqui é especificação do banco, so copia e cola
    generator client {
    provider = "prisma-client-js"
    }

    datasource db {
    provider = "postgresql"
    url = env("DATABASE_URL")
    }

    //

    Aqui começa a implementar

    model User {
    id Int @id @default(autoincrement())
    name String
    email String @unique
    posts Post[]
    }

    model Post {
    id Int @id @default(autoincrement())
    title String
    content String
    author User @relation(fields: [authorId], references: [id])
    authorId Int
    }

-   Depois executar esse comando `npm i @prisma/clieqnt`
-   Depois so rodar a migração `npx prisma migrate dev`

-   Adicione tbm no seu package.json `"dev": "ts-node-dev --env-file .env -r tsconfig-paths/register --inspect --transpile-only --ignore-watch node_modules src/shared/http/server.ts"`

### Docker com DBever

-   Crier um arquivo com o nome ``docker-compose.yml` esse aquivo vai ser responsavel para conequição, vai ser colocado isso
    version: '3.7'
    services:
    db:
    image: postgres
    restart: always
    environment:
    POSTGRES_PASSWORD: test
    POSTGRES_USER: projeto
    ports: - 5436:5432

Depois so executar o comando `docker-compose up`

-   Agora so ir no DBever e criar o banco, trocando a porta (no ex é 5436) colocando o usuario e a senha.

-   [Qualquer duvida ver o video](https://youtu.be/EwUJcdB-vKs?si=m_JyxZRilMUjYENU)

### Estrutura do Projeto

-Estrutura de pastas:
_config_ - configurações de bibliotecas externas, como por exemplo, autenticação, upload, email, etc.
_modules_ - abrangem as áreas de conhecimento da aplicação, diretamente relacionados com as regras de negócios. A princípio criaremos os seguintes módulos na aplicação: customers, products, orders e users.
_shared_ - módulos de uso geral compartilhados com mais de um módulo da aplicação, como por exemplo, o arquivo server.ts, o arquivo principal de rotas, conexão com banco de dados, etc.
_services_- estarão dentro de cada módulo da aplicação e serão responsáveis por todas as regras que a aplicação precisa atender, como por exemplo:

-A senha deve ser armazenada com criptografia;
-Não pode haver mais de um produto com o mesmo nome;
-Não pode haver um mesmo email sendo usado por mais de um usuário;
-Criando a estrutura de pastas:

        mkdir -p src/config
        mkdir -p src/modules
        mkdir -p src/shared/http
        mv src/server.ts src/shared/http/server.ts

Ajustar o arquivo package.json:

**{"scripts": "dev": "ts-node-dev --inspect --transpile-only --ignore-watch node_modules src/shared/http/server.ts"}**

-   Configurando as importações
    Podemos usar um recurso que facilitará o processo de importação de arquivos em nosso projeto.
    Iniciamos configurando o objeto paths do tsconfig.json, que permite criar uma base para cada path a ser buscado no projeto, funcionando de forma similar a um atalho:

    "baseUrl": "./"
    "paths": {
    "@config/_": ["src/config/_"],
    "@modules/_": ["src/modules/_"],
    "@shared/_": ["src/shared/_"]
    }
    instalar a biblioteca que irá indicar ao nosso script do ts-node-dev. O nome dessa biblioteca é tsconfig-paths, e para instalar execute o seguinte comando no terminal (na pasta do projeto):
    `yarn add -D tsconfig-paths`
    Depois de instalar o tsconfig-paths, ajustar o script dev no arquivo package.json, incluindo a opção _-r tsconfig-paths/register_. Deverá ficar assim:

    **"dev": "ts-node-dev -r tsconfig-paths/register --inspect --transpile-only --ignore-watch node_modules src/shared/http/server.ts"**
    Observação: o comando acima deve ser incluído como uma linha única do script dev. Agora, para importar qualquer arquivo no projeto, inicie o caminho com um dos paths configurados, usando o CTRL+SPACE para usar o autocomplete.

## Desenvolvimento

-   Instalação de blibioteca _yarn add express cors express-async-errors_ e _yarn add -D @types/express @types/cors_
-   So programar

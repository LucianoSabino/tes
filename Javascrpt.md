### Inicializando

`yarn init -y`

<h3 align="center">Docker</h3>
- Crier um arquivo com o nome ``docker-compose.yml` esse aquivo vai ser responsavel para conexão
    Coloque no arquivo: 
        version: '3'

        services:
        db-postgres-evento:
            image: postgres
            ports:
            - "5438:5432"
            container_name: "db-postgres-evento"
            restart: always
            volumes:
            - ./banco-de-dados-evento:/var/lib/postgresql/data
            environment:
            POSTGRES_USER: pguser
            POSTGRES_PASSWORD: pguser

        volumes:
            banco-de-dados-evento:

- Depois execute esse comando `docker-compose up -d`

<h3 align="center">Prisma</h3>

- Execute esse comando para instalar `yarn add prisma @prisma/client`
- Iniciando o prisma `yarn prisma init `

  - Depois faça as configurações do banco de dados mo arquivo .env
  - Depois va no arquinvo prisma/schema.prisma, nele que vai ser criado o banco de dados e a tabela como por exemplo

        // Crianado a tabela
        model User {

        // Criando as colunas
        id Int @id @default(autoincrement())
        name String
        email String @unique
        password String
        phone String?
        creatdAt DateTime @default(now())
        updatadAt DateTime @updatedAt
        }

    - Depois rode esse comando para criar migration `yarn prisma migrate dev --name init`

Agora sua conexão com o banco de dados e sua tabela foi feita.

<h3 align="center">Fazendo a api</h3>

Execute esse comando, para instalar as blibiotecas `yarn add express cors dotenv`

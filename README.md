# Aula de Back-End / DER

## Tópicos
- **DER**
    - Entidades
    - Atributos
    - Relacionamentos
    - Cardinalidade
    - Exemplo de DER

- **Back-End**
    - Projeto com CRUD completo
    - Montando o schema.prisma
    - Comandos do prisma

## DER
- ### Entidades
    Representam coisas ou objetos do mundo real, como um "cliente" ou um "produto". No padrão tradicional, são desenhadas como ***retângulos***
- ### Atributos
    São as características ou propriedades de uma entidade, como o "nome" ou o "cpf" do cliente. São representados por ***elipses***
- ### Relacionamentos
     Mostram como as entidades interagem entre si, como um cliente que "realiza" um pedido. São desenhados como ***losangos***
- ### Cardinalidade
    Indica o limite numérico do relacionamento, como uma relação de um-para-muitos (um cliente pode fazer vários pedidos, mas um pedido pertence a um único cliente)

|**Exemplo**|
|:-:|
|![Img do DER](./assets/)|
## Back-End

- ### Projeto
    Para começar nossa aula é preciso clonar este repositório e abri-lo com o code
    ```
    git clone https://github.com/nickfelisbelo/aula_backend
    cd aula_backend
    code .
    ```

- ### Montando schema.prisma
    Primeiro precisamos analisar o DER feito, depois precisamos analisar e decidir qual o tipo de cada atributo (Int, Decimal, String, DateTime, etc)

    Após analisar o DER e ver os atributos devemos fazer um sistema de models parecido com:
    ```
    model Entidade {
        atributoChavePrimaria   type @id
        atributo                type
        atributo                type

        atributoChaveSecundaria type

        identificador EntidadeRelacionada @relation(fields: [atributoChaveSecundaria], references: [atributoChavePrimaria]) 
    }
    ```
    Ao analisar o DER teremos este ``schema.prisma`` 
    ```
    generator client {
      provider = "prisma-client-js"
    }

    datasource db {
      provider = "mysql"
    }

    model Usuario {
        id       Int    @id @default(autoincrement())
        nome     String
        telefone String

        emprestimos Emprestimo[]
    }

    model Emprestimo {
        id        Int    @id @default(autoincrement())
        livro     String
        data_emprestimo DateTime
        data_devolucao DateTime
        usuarioId Int

        usuario Usuario @relation(fields: [usuarioId], references: [id])
    }
 
    ```
- ### Comandos do prisma
    Para iniciar o nosso projeto é preciso:
    - Iniciar o seu banco de dados(Caso seja no XAMPP)
    
    Após iniciar o banco de dados, rode os seguintes comandos:
    ```
    npx prisma migrate dev
    npx prisma generate
    ```
    Caso ocorra um erro por conta da versão do prisma, rode o seguintes comandos:
    ```
    npx prisma reset
    ```
    Depois apague o banco de dados (DROP DATABASE xxxxx - No Xampp) e delete a pasta ``migrations``, ou utilize um novo banco (mudar o nome do que está no `.env`). Por fim, rode os seguintes comandos no terminal:
    ```
    npm uninstall prisma @prisma/client
    npm install prisma@7 @prisma/client@7
    npx prisma migrate dev
    npx prisma generate
    ```
    Por fim, vc tem que iniciar seu servidor, então rode o comando:
    ```
    npm run dev
    ```

- ### Caso deseje criar os controllers e routes
    Aqui você pode criar normalmente ou usar a extensão do Reenye, caso deseje utilizar a dependência do Reenye, siga o passo a passo:
    - Baixe a extensão:
    ```
    npx -g backend-aula
    ```
    - Após baixar utilize o comando:
    ```
    backend-aula nomedoseuprojeto
    ```
    - Após criar o seu projeto com a extensão, crie o seu banco de dados(``schema.prisma``)
    - Em seguida, utilize o comando para cada model criado(escreva em letra miúscula, ex: model Entidade => backend-aula -r entidade)
    ```
    backend-aula -r nomedaentidade
    ```
    - Caso deseje testar no Insomnia, rode o comando:
    ```
    backend-aula -insomnia
    ```
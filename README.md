## Flask CRUD API (Aplicação CRUD + Docker):

A pasta "flask_crud_api" contém o código de uma API CRUD utilizando o framework Flask que salva e manipula dados em um banco de dados PostgreSQL local. O programa "flask_app.py" implementa operações de criação, leitura, atualização e exclusão de pacientes com sintomas de gripe. 

O código está disponível para análise, mas para executar o container recomendo puxar a imagem do [repositório no Docker Hub](https://hub.docker.com/r/ericmidt/flask_crud_api).

Para executar o container, siga estas etapas:
1. Certifique-se de ter o Docker instalado.

2. Execute o aplicativo Docker Desktop.

3. No diretório de sua escolha, puxe a imagem do container:
    ```bash
    docker pull ericmidt/flask_crud_api
    ```
    Caso haja algum problema ao executar o comando, entre na sua conta Docker Hub no terminal e tente novamente.

4. Execute o seguinte comando (não se esqueça de substituir as credenciais
com as suas próprias para acessar seu banco local PostgreSQL):
    ```bash
    docker run -e DB_NAME="mydb" -e DB_USER="myuser" -e DB_PASSWORD="mypassword" -e DB_HOST="host.docker.internal" -e DB_PORT="your_db_port" -p 5000:5000 ericmidt/flask_crud_api:latest
    ```
    Exemplo:
    ```bash
    docker run -e DB_NAME="postgres" -e DB_USER="postgres" -e DB_PASSWORD="12345" -e DB_HOST="host.docker.internal" -e DB_PORT="5432" -p 5000:5000 ericmidt/flask_crud_api:latest
    ```
    Geralmente a porta padrão do PostgreSQL é "5432".

    Você deve ver a seguinte mensagem no seu terminal:
    ```bash
    * Serving Flask app 'flask_app'
    * Debug mode: off
    WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
    * Running on all addresses (0.0.0.0)
    * Running on http://127.0.0.1:5000
    * Running on http://172.17.0.2:5000
    Press CTRL+C to quit
    ```
5. A API estará acessível em http://localhost:5000.

### Endpoints
#### Listar Pacientes
Retorna uma lista de todos os pacientes.

- URL: /pacientes
- Método: GET
- Resposta:
    - 200 OK - Lista de pacientes.

#### Criar Paciente
Cria uma nova entrada de paciente.

- URL: /paciente
- Método: POST
- Corpo da Requisição:
    ```json
    {
    "timestamp": "AAAA-MM-DD HH:MM:SS",
    "sexo": "Gênero",
    "idade": 99,
    "sintomas": "Sintomas",
    "dataInicioSintomas": "AAAA-MM-DD HH:MM:SS",
    "municipio": "Cidade",
    "estado": "Estado",
    "tomouVacinaCovid": true
    }
    ```
- Resposta:
    - 200 OK - Paciente criado com sucesso.

#### Atualizar Paciente
Atualiza uma entrada de paciente existente.

- URL: /paciente/{id}
- Método: PUT
- Parâmetros da URL:
    - id - ID do Paciente
- Corpo da Requisição:
    ```json
    {
    "timestamp": "AAAA-MM-DD HH:MM:SS",
    "sexo": "Gênero",
    "idade": 0,
    "sintomas": "Sintomas",
    "dataInicioSintomas": "AAAA-MM-DD HH:MM:SS",
    "municipio": "Cidade",
    "estado": "Estado",
    "tomouVacinaCovid": true
    }
    ```
- Resposta:
    - 200 OK - Paciente atualizado com sucesso.
    - 404 Not Found - Paciente com o ID especificado não encontrado.

#### Remover Paciente
Remove uma entrada de paciente existente.
- URL: /paciente/{id}
- Método: DELETE
- Parâmetros da URL:
    - id - ID do Paciente
- Resposta:
    - 200 OK - Paciente removido com sucesso.
    - 404 Not Found - Paciente com o ID especificado não encontrado.



### Testes (requisições de exemplo)
#### Criar Paciente
Para criar três pacientes, execute os comandos abaixo no seu terminal:

```bash
curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-18T12:00:00Z\", \"sexo\": \"Feminino\", \"idade\": 25, \"sintomas\": \"Dor de cabeça, Cansaço\", \"dataInicioSintomas\": \"2023-08-18T00:00:00Z\", \"municipio\": \"Curitiba\", \"estado\": \"PR\", \"tomouVacinaCovid\": true}" http://localhost:5000/paciente

curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-19T12:00:00Z\", \"sexo\": \"Masculino\", \"idade\": 74, \"sintomas\": \"Tosse, Coriza\", \"dataInicioSintomas\": \"2023-08-18T00:00:00Z\", \"municipio\": \"Curitiba\", \"estado\": \"PR\", \"tomouVacinaCovid\": false}" http://localhost:5000/paciente

curl -X POST -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-20T12:00:00Z\", \"sexo\": \"Feminino\", \"idade\": 36, \"sintomas\": \"Dor de cabeça, Febre\", \"dataInicioSintomas\": \"2023-08-18T00:00:00Z\", \"municipio\": \"Curitiba\", \"estado\": \"PR\", \"tomouVacinaCovid\": true}" http://localhost:5000/paciente
```
Abra o programa pgAdmin4, e clique com o botão direito na seção "Tables" e clique em "Refresh".
Você deve ver uma nova tabela chamada "dados_gripe". Clique com botão direito na seção
"Tables", clique em "Query Tool". Em seguida você pode realizar uma query para
verificar os dados que foram salvos, como por exemplo:

```sql
SELECT * FROM notificacoes_sindrome_gripal_parana
```
#### Listar Pacientes
Para retornar os pacientes inseridos na tabela, execute o comando:
```bash
curl http://localhost:5000/pacientes
```

#### Atualizar Paciente
Para atualizar o paciente com id 1, execute o comando:
```bash
curl -X PUT -H "Content-Type: application/json" -d "{\"timestamp\": \"2023-08-21T12:00:00Z\", \"sexo\": \"Masculino\", \"idade\": 30, \"sintomas\": \"Febre, Tosse\", \"dataInicioSintomas\": \"2023-08-19T00:00:00Z\", \"municipio\": \"São Paulo\", \"estado\": \"SP\", \"tomouVacinaCovid\": true}" http://localhost:5000/paciente/1
```

#### Excluir Paciente
Para excluir o paciente com id 1, execute o comando:
```bash
curl -X DELETE http://localhost:5000/paciente/1
```
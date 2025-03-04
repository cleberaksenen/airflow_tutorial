# Apache Airflow tutorial

![image](https://github.com/user-attachments/assets/dda1ff5c-0672-4bda-88b2-cb857bd79a51)

## Conceito
Apache Airflow é uma plataforma (conjunto de processos, software) para criar, programar e monitorar fluxos de trabalho (workflows) de maneira programática

O foco do Airflow não é o que está dentro da tarefa! Mas sim na orquestração de execução dessas atividades (monitorar se o A executou ou não, para que possa executar o B).

Quando os fluxos de trabalho são definidos como código (Python), eles se tornam dinâmicos! Ou seja, mais fáceis de manter, verificáveis, testáveis e colaborativos (SE X, ENTAO Y).

O desenvolvedor Airflow executa suas tarefas e desenvolve os DAGs em uma série de Workers, enquanto segue as dependências entre elas.

-> Possui muitos recursos utilitários de linha de comando para manipular e fazer manutenções nos DAGs.

-> Possui também interface web com o usuário (porta 8080).

## O que não é
- Não é uma solução de streaming ou roteamento de dados, não estando no espaço do Spark Streaming ou Storm.
- Não é uma ferramenta para ETL de dados.
- Não é uma ferramenta de armazenamento.
- Não se transfere dados de uma tarefa para outra, no máximo parâmetros.
- Espera-se que os fluxos de trabalho sejam estáticos ou mudem lentamente.

## Como e quando usar

![Diagrama em branco (2)](https://github.com/user-attachments/assets/a0eab936-24b0-475c-952d-39116941a100)

Se parar para pensar, um agendador de tarefas simples poderia fazer isso. Então porque usar o Apache Airflow?

- Se você tiver dezenas de Workflows para gerenciar;
- Se você precisar conectar com várias fontes de dados (SQL, Oracle);
- Garante monitoramento e tolerância a falhas.

## Arquitetura

![image](https://github.com/user-attachments/assets/25b0b3d7-c2ac-4826-afe7-f737d4e45bbe)

- Airflow.cfg – define a interface geral (coração da aplicação);
- Interface do usuário pode ser via CLI ou Web;
- Schedule – responsável pelo fluxo de tarefas e execução do workflow;
- Executor pode ser local ou sequencial, impactando na forma de execução das tarefas;
- Worker – responsável por executar as tarefas para depois devolver os resultados;
- Todos precisam acessar os metadados (backend);

### DAG (Directed Acyclic Graph, ou Grafo Acíclico Direcionado)
- Representa a estrutura de um fluxo de trabalho, sendo a coleção de todas as tarefas a serem executadas, organizada de modo que reflita seus relacionamentos e dependências;
- Uma DAG define a ordem de execução das tarefas (tasks) e suas dependências, garantindo que o fluxo siga um caminho lógico e sem ciclos.
- É possível construir lógica nas tarefas. Ex. Tarefa B só será executada depois da A, tarefa C só será executada depois de 10 min de finalização da tarefa A.
- Não me importa o que cada tarefa irá fazer e sim que aconteçam no momento certo e na ordem correta.
- Podemos ter SubDAGs, permitindo flexibilidade e organização, sem repetição.

### Default Parameters
- Podemos passar um dicionário com parâmetros para um DAG (tarefas).
- Isso evita a repetição de passagem de vários parâmetros.
  
### Context manager
- Forma de tornar o DAG dinâmico (se – A; se não – B).

### Operators
- São os responsáveis pela determinação do que realmente é feito (tarefas).

BashOperator – executa um comando bash;

PythonOperator – chama uma função Python arbitrária;

EmailOperator – envia um e-mail;

SimpleHttpOperator – envia uma solicitação HTTP;

SQLOperator – executa um comando SQL;

Sensor – espera um certo tempo, arquivo, banco de dados...

### Hooks
- Interfaces para plataformas externas e bancos de dados.

### Pools
- Limitadores de recurso, como paralelismo e uso de CPU.

### Connections
- Conexão com fontes de dados externa.
- Cada conexão terá um ID próprio, podendo ser reutilizada em outros DAGs.

### Xcom (comunicação cruzada)
- Recurso que permite que tarefas se comuniquem (A e B se comunicam).
- A comunicação é de metadados, parâmetros e variáveis.
- É definido com um conjunto chave/valor.

### Variáveis
- Forma de comunicar parâmetros, guardar configurações.

### Branching (ramificação)
- Uma bifurcação que pode ser a partir de uma condição ou não.
- Para controle, pode-se voltar para a tarefa anterior.

### Trigger Rules
- Ações disparadas a partir de condições.
- O parâmetro trigger_rule define a regra pela qual a tarefa gerada é acionada.

### Last Execution Only
- Usado em cenários com menos complexidade, sem dependência, sub gráfico ou branch. Não precisamos criar um DAG.
- Posso executar uma tarefa sem necessidade de pedir para os dados e parâmetros serem guardados (no backfills).

### Zombie Mode
- Quando tarefas estão estagnadas, o Airflow tenta encerrá-la, caso falha, ou reiniciá-la, caso necessário. Uma “limpeza” do fluxo.

### Cluster Policies
- Cenários e contextos definidos em relação ao uso de recursos e acesso a tarefas. Semelhante a uma permissão de rede.

Ex. Apenas o grupo X terá acesso ao recurso Y.

### Jinja Templating
- Ferramenta para implementar macros, fazer loops.
- Usado para sofisticar e aproveitar tarefas e DAGs.

### Packaged DAGs
- Combinação de DAGs diferentes - principais e específicos.
- Usado em grandes ambientes.

### Backfill
- Usado quando há necessidade em saber a execução do DAG em um período passado, para refletir sobre os ajustes e mudanças realizadas.
- Especifica-se START_DATE e END_DATE como parâmetros.

***

## Instalação
### Passo 01: Acessar como usuário root
```
sudo bash
```
### Passo 02: Criar uma variável de ambiente (AIRFLOW_HOME)
```
export AIRFLOW_HOME=~/airflow
```

É recomendado adicionar ao PATH:
```
nano ~/.bashrc
```
-> Na última linha adicionar "export AIRFLOW_HOME=~/airflow"
```
source ~/.bashrc
```

### Passo 03: Instalação via pip
```
pip install apache-airflow 
```
### Passo 04: Inicializando o MetaStore no Apache AIRFLOW
Inicialmente com SQLite para configurar a ferramenta, mas em seguida é melhor migrar para um outro banco de dados (MySQL, PostgreSQL).
```
airflow db init
```

### Passo 05: Principais altrações
Acesse a pasta airflow na home do usuário:
```
cd ~/airflow/
```
-> O arquivo airflow.cfg é o arquivo de configurações.
-> Os DAGs ficam dentro dessa pasta "/root/airflow/dags" (Essa pasta precisa ser criada)

Migrando de SQLite para PostgreSQL ou MySQL:
Será necessário editar no arquivo de configuração a string de conexão: 
"sql_alchemy_conn = sqllite:////root/airflow/airflow.db"

#### PostgreSQL:
sql_alchemy_conn = postgresql+psycopg2://airflow:airflow@localhost/airflow_db

#### MySQL:
sql_alchemy_conn = mysql+mysqldb://airflow_user:airflow_pass@localhost:3306/airflow_db

Será necessário mudar o executor:
executor = SequentialExecutor

executor = LocalExecutor

Instalando o PostgreSQL:
```
sudo apt-get install postgresql postgresql-contrib
```
```
nano /etc/postgresql/12/main/pg_hba.conf
```
Descomentar/ativar essa opção:
![image](https://github.com/user-attachments/assets/0eb44bad-88e3-4c1d-ba14-c0d8384ee97e)

Por fim, dar o restart no PostgreSQL:
```
service postgresql restart
```
Conectando ao PostgreSQL:
```
sudo -u postgres psql
```
Criando o usuário PostgreSQL para o Airflow:
```
CREATE USER [nome do usuário] PASSWORD [senha];
```
–> tem que ser o mesmo usuário do Airflow
```
CREATE DATABASE [nome do banco de dados];
```
-> tem que ser o mesmo nome do banco de dados do arquivo de configuração
```
GRANT ALL PRIVILEGES ON DATABASE [nome do banco de dados] TO [nome do usuário]
```
Tendo feito a migração do banco de dados SQLite para PostgreSQL ou MySQL:
```
airflow db init 
```

***

## Configurações
### Criação de usuário:
```
airflow users create \
    --username admin \
    --firstname SeuNome \
    --lastname SeuSobrenome \
    --role Admin \
    --email seu@email.com \
    --password senha_segura
```

## Iniciando a interface web (localhost:8080)
```
airflow webserver --port 8080
```

## Subir o agendador (scheduler):
```
airflow scheduler
```









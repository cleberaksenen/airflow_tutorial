# Apache Airflow tutoriall

![image](https://github.com/user-attachments/assets/dda1ff5c-0672-4bda-88b2-cb857bd79a51)

## Conceito
Apache Airflow é uma plataforma (conjunto de processos, software) para criar, programar e monitorar fluxos de trabalho (workflows) de maneira programática

![image](https://github.com/user-attachments/assets/022b2c7a-9429-411b-8847-f3de301e818f)

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

![image](https://github.com/user-attachments/assets/a0f02c48-2a85-429a-989a-9c2d62f73c22)

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








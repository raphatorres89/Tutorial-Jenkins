# Jenkins Tutorial

## Setup

Para rodar o Jenkins, será necessário:
1. Java 8
2. Docker

### Download e Execução
1. http://mirrors.jenkins.io/war-stable/latest/jenkins.war
2. Abrir o terminal no diretório do arquivo baixado
3. Executar `java -jar jenkins.war --httpPort=8080`
4. Navegar até `http://localhost:8080`
5. Seguir as instruções para completar a instalação

Quando finalizar a instalação, já será possível colocar o Jenkins para funcionar ;)

## Criando um Pipeline

Um Pipeline é um conjunto de plugins que dá suporte a uma implementação e faz a entrega contínua no Jenkins.

Uma entrega contínua é uma expressão para um processo de entrega de software com controle de versão para usuários e clientes.

O Pipeline do Jenkins é feito escrevendo em um arquivo de texto chamado `Jenkinsfile`, que é apontado para um repositório de controle de versão (git).

### Exemplo

1. Dentro do seu projeto, crie um arquivo chamado `Jenkinsfile` (igual ao deste repositório)

2. Clique em **New Item** no menu do Jenkins.

![](https://jenkins.io/doc/book/resources/pipeline/classic-ui-left-column.png)

3. Dê um nome para seu novo item e selecione **Multibranch Pipeline**

4. Clique em **Add Source**, escolha o tipo do repositório e preencha os detalhes

5. Clique em **Save** e veja seu novo Pipelne rodar.

POde ser necessário modificar um dos exemplos do `Jenkinsfile` para rodar com seu projeto. Tente modificar o comando `sh` para rodar o mesmo comando utilizado par rodar o programa localmente em sua máquina.

Depois de configurar seu Pipeline, o Jenkins irá detectar automaticamente cada nova Branch ou Pull Request do seu repositório e começará a rodar os Pipelines para eles.

## Rodando múltiplos passos (steps)

Pipelines são formados por várias etapas que lhe permitem buildar, testar e deployar suas aplicações. Os Pipelines proporcionam ordenar diversos passos de forma simples para ajudar a automatizar os processos.

Imagine a "etapa" como um comando único que gera uma única ação. Quando a etapa têm sucesso, ela move para a próxima. Quando a etapa falha, o Pipeline irá falhar.

Quando todas as etapas passarem, o Pipeline considerará a execução feita com sucesso.

```
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
    }
}
```

### Timeouts, retries e mais

Existem etapas poderosas que englobam outras etapas que podem facilmente resolver problemas como repetir (`retry`) etapas até ter sucesso ou sair de uma etapa se ela demorar muito tempo (`timeout`).

```
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                retry(3) {
                    sh './flakey-deploy.sh'
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                }
            }
        }
    }
}
```

O estágio de "Deploy" repete o script `flakey-deploy.sh` 3 vezes, e então espera 3 minutos para executar o script `health-check.sh`. Se o health check não completar em 3 minutos, o Pipeline será marcado como falha no estágio de "Deploy".

Etapas "Wrapper" como `timeout` e `retry` podem conter outras etapas, incluindo `timeout` ou `retry`.

Podemos compor estas etapas juntos. Por exemplo, se quisermos repetir nosso deploy 5 vezes, mas nunca demorar mais de 3 minutos no total antes de falhar o cenário:

```
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        sh './flakey-deploy.sh'
                    }
                }
            }
        }
    }
}
```

### Finalizando

Quando o Pipeline termina de executar, pode ser necessário executar etapas de limpeza ou executar algumas ações com base no resultado do Pipeline. Essas ações podem ser executadas na seção de `post`.

```
pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'echo "Fail!"; exit 1'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
```








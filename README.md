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



---
Owner: Blossom Dude
Last edited time: 2024-01-11T14:27
Created time: 2023-08-07T20:11
---
- [[#Jenkinsfile|Jenkinsfile]]
- [[#Agent|Agent]]
- [[#Триггеры|Триггеры]]

`/lib/systemd/system/jenkins.service` - файл конфигурации

Jenkins CLI

```Bash
java -jar jenkins-cli.jar -auth admin:*token* -s *URL* help
# Подключение к Jenkins CLI через jar-ник, токен и запуск команды help
```


### Jenkinsfile

Структура такова:  
pipeline → stages → stage → steps  

  

- ==Пример декларативного Pipeline + post action==
    
    ```Bash
    pipeline {
        agent {
            label "node01"
        }
        
        tools {
            go 'go-1.20'
        }
        
        environment {
            GO111MODULE='on'
        }
        
        stages {
            stage('test'){
                steps{
                    git 'https://github.com/rotoro-cloud/go-webapp-sample.git'
                    sh 'go test ./...'
                }
            }
            
            stage('build docker image'){
                steps{
                    script{
                        app = docker.build("rotoro-cloud/go-webapp-sample")
                    }
                }
            }
    
    		post {            # В секции post есть пять блоков. 
            always {      # Это по сути результаты завершения работы конвейера.
                echo 'Pipeline is done.'
            }
            success {
                echo 'Well, its successful!'
            }
            failure {
                echo 'Well, it failed!'
            }
            unstable {
                echo 'But its unstable'
            }
            changed {
                echo 'And this run has changed status after last build'
            }
            aborted {
                echo 'The build was aborted.'
            }
        }
    }
    ```
    
- ==Пример параллельных стейдж для быстроты сборки  
    Параллельные сборки программ и запуск на соответсвующих агентах.  
    ==
    
    ```Bash
    pipeline {
        agent {
            label 'ubuntu-agent01'
        }
        tools {
            go 'go-1.20'
        }
        stages {
            stage('test') {
                steps {
                    git 'https://github.com/rotoro-cloud/go-webapp-sample.git'
                    sh 'go test ./...'
                }
            }
            stage('build') {
                parallel {
                    stage ('linux') {
                        steps {
                            sh 'GOOS=linux GOARCH=amd64 go build -o go-webapp-sample-amd64-linux .'
                            sh 'cp -f ./go-webapp-sample-amd64-linux /tmp/'
                        }
                    }
                    stage ('macos') {
                        steps {
                            sh 'GOOS=darwin GOARCH=amd64  go build -o go-webapp-sample-amd64-darwin .'
                        }
                    }
                    stage ('windows') {
                        steps {
                            sh 'GOOS=windows GOARCH=amd64 go build -o go-webapp-sample-amd64.exe .'
                        }
                    }
                }
            }
            stage('run') {
                parallel {
                    stage ('linux') {
                        agent {
                            label 'ubuntu-agent01'
                        }
                        steps {
                            sh 'export JENKINS_NODE_COOKIE=dontKillMe && /tmp/go-webapp-sample-amd64-linux &'
                        }
                    }
                    stage ('macos') {
                        agent {
                            label 'macos'
                        }
                        steps {
                            sh './go-webapp-sample-amd64-darwin &'
                        }
                    }
                    stage ('windows') {
                        agent {
                            label 'win-10-x64'
                        }
                        steps {
                            bat 'START ./go-webapp-sample-amd64.exe /min'
                        }
                    }
                }
            }
        }
    }
    ```
    
- ==Вложенные Stages (====sequential stages====)==
    
    ```Bash
    pipeline {
        agent any
        stages {
            stage('first') {
                stages {                    # вот тут определяются 
                    stage('second') {       # вложенные stages
                        steps {
                            echo "Second stage"
                        }
                    }
                }
            }
        }
    }
    ```
    
- ==Передача параметров в pipeline== 
    
    ```Bash
    pipeline {
        agent any
        parameters {
            string defaultValue: 'Phoenix', description: 'Input the project ID', name: 'project_id'
        }
        stages {
            stage('first') {
                steps {
                    echo "project is ${params.project_id}"
                }
            }
        }
    }
    ```
    
- ==Условия WHEN==
    
    ==  
      
    ==[Документация по when](https://www.jenkins.io/doc/book/pipeline/syntax/#when)
    
    ==When==
    
    ```Bash
    pipeline {
        agent any
        environment {
            access = 'granted'
        }
        stages {
            stage('first') {
                when {
                    environment name: 'access', value: 'granted'
                }
                steps {
                    echo "Just completed the first stage with ${env.access}"
                }
            }
        }
    }
    ```
    
    ---
    
    ==allOf== - если хотим чтобы все условия выполнялись
    
    ==anyOf== - одно из условий
    
    ==not== - сработает если ни одно условие не сработало
    
    ```Bash
    pipeline {
        agent any
        environment {
            access = ''
        }
        stages {
            stage('first') {
                when {
                    not {
                        allOf {
                            environment name: 'access', value: 'granted'
                            branch 'master'
                        }
                    }
                }
                steps {
                    echo "Just completed the first stage in ${BRANCH_NAME}"
                }
            }
        }
    }
    ```
    
- ==IF ELSE и try catch==
    
    ```Bash
    pipeline {
        agent any
        environment {
            TZ='Tokyo'
        }
        stages {
            stage('check') {
                steps {
                    script {
                        if(env.TZ == 'Moscow') {
                            echo 'Your timezone is UTC+3:00'
                        }
                        else {
                            try {
                                sh "prepare_new_tz.sh"
                            }
                            catch(e) {
                                echo "Error with the new TZ creation"
                            }
                        }
                    }
                }
            }
        }
    }
    ```

- Options

[https://www.jenkins.io/doc/book/pipeline/syntax/#available-options](https://www.jenkins.io/doc/book/pipeline/syntax/#available-options)

### Agent

  

Условия к агенту

- SSHD
    - `PasswordAuthentication yes` в ssh-конфиге.
- Java
    - `sudo apt install default-jdk` - устанавливаем jdk
- Создание User
    
    - `sudo adduser jenkins` - создаем пользователя jenkins
    - `sudo usermod -aG sudo jenkins` - выдаем права админа
    
> [!important]  
> Рекомендуется ставить одну и ту же версию JDK на все инстансы - агенты и контроллер. Так как возможны ошибки.  
    
```Bash
    
    agent none         # Без агента. Нужно определить в Stage
    
    --------------------------------
    
    agent any           #Любой доступный агент
    
    --------------------------------
    
    agent {             # Таким способом можно определять в Stage.
    	label "agent1"    # Используются определенный агент(ы)
    }                   # с такой меткой.
    
    --------------------------------
    
    agent {             # агент в контейнере,
            docker {    # создаваемый в pipeline
                image 'golang:1.19-alpine' 
                args '-u root:root'
            }
```



### Триггеры
Без плагинов доступно четыре типа:

- `builds remotely` - позволяющий создать эндпоинт, по вызову которого будет запущен job. URL примерно такого вида **JENKINS_URL/job/fs/build?token=TOKEN_NAME**.
- `Build after other projects are built` - Этот механизм позволяет одной job запуститься в зависимости от того, как закончилась другая.
- `Build periodically` - Это про запуск заданий по времени. Можно сравнить с **cron**.
- `Poll SCM` - Происходит периодический опрос репозитория, и, если в нем произошли изменения, запускается сборка.

```Bash
    pipeline {
        stages{
            stage('Build Dev') {
                agent {
                    label {
                        label 'dev'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    sh 'git pull'
                }
            }
            
            stage ('Test Dev') {
                agent {
                    label {
                        label 'dev'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    sh "go test ./..."
                }
            }
            
            stage ('Deploy Dev') {
                agent {
                    label {
                        label 'dev'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    script {
                        withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                            sh 'go run main.go &'
                        }
                    }
                }
            }
    
    				stage('Build Prod') {
                agent {
                    label {
                        label 'prod'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    sh 'git pull'
                }
            }
            
            stage ('Test Prod') {
                agent {
                    label {
                        label 'prod'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    sh "go test ./..."
                }
            }
            
            stage ('Deploy Prod') {
                agent {
                    label {
                        label 'prod'
                        customWorkspace "/opt/go-app"
                    }
                }
                steps {
                    script {
                        withEnv ( ['JENKINS_NODE_COOKIE=do_not_kill'] ) {
                            sh 'go run main.go &'
                        }
                    }
                }
            }
        }
    }
```
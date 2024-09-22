---
Owner: Blossom Dude
Last edited time: 2024-01-18T17:38
Created time: 2024-01-12T12:54
---
- UCB_podman_pipeline_for_UCB
    
    ```Bash
    pipeline {
      agent any
      stages {
        stage('Checkout Scm Platform') {
          steps {
            git(credentialsId: '4898b7c5-e9ff-4bff-adee-7158a04195f2', url: 'http://172.29.73.100/unicreditbank/docshouse.git')
          }
        }
        stage('Maven Build Platform') {
          steps {
            sh 'mvn clean install -DskipTests'
          }
        }
    	  stage('Checkout Scm API') {
          steps {
            git(credentialsId: '4898b7c5-e9ff-4bff-adee-7158a04195f2', url: 'http://172.29.73.100/unicreditbank/ucb-ecm-api.git')
          }
        }
        stage('Maven Build API') {
          steps {
            sh 'mvn clean install -DskipTests'
          }
        }
    	  stage('Checkout Scm Webapp') {
          steps {
            git(credentialsId: '4898b7c5-e9ff-4bff-adee-7158a04195f2', url: 'http://172.29.73.100/unicreditbank/ucb-ecm-webapp.git')
          }
        }
        stage('Maven Build Webapp') {
          steps {
            sh 'mvn clean install -DskipTests'
          }
        }
      
        stage('Shell script Build image') {
          steps {
            sh '''echo "FROM registry.test.ucb:9080/template-ucb-webapp_for_ucb:0.6" > Dockerfile
    				echo "COPY ./target/ucb-ecm-webapp-exec.jar /app/ucb-ecm-webapp.jar" >> Dockerfile
    				docker build -t registry.test.ucb:9080/ucb-ecm-webapp:${NEW_TAG} .
    				docker push registry.test.ucb:9080/ucb-ecm-webapp:${NEW_TAG}
    				docker save -o /tmp/ucb-web-pp_${NEW_TAG}.tar registry.test.ucb:9080/ucb-ecm-webapp:${NEW_TAG}'''
          }
        }
    
        stage('Checkout Scm Front') {
          steps {
            git(branch: 'master', url: 'http://172.29.73.100/unicreditbank/ucb-front.git', credentialsId: '4898b7c5-e9ff-4bff-adee-7158a04195f2')
          }
        }
    
        stage('Shell script Build Front') {
          steps {
            sh '''npm config set registry http://registry.npmjs.org/
    				npm cache clean --force
    				npm install
    				npm run build -- --mode ucb-ucb-test'''
          }
        }
        stage('Shell script Build front image') {
          steps {
            sh '''echo "FROM registry.test.ucb:9080/template-ucb-front_for_ucb:0.6" > Dockerfile
    				echo "COPY ./dist/ /usr/local/apache2/htdocs/app/" >> Dockerfile
    				docker build -t registry.test.ucb:9080/front-ucb:${NEW_TAG} .
    				docker push registry.test.ucb:9080/front-ucb:${NEW_TAG}
    				docker save -o /tmp/front-ucb.tar registry.test.ucb:9080/front-ucb:${NEW_TAG}
    				cd /tmp
    				'''
          }
        }
    
    }
      tools {
        maven 'maven_3.8.2'
        jdk 'JDK11'
        nodejs 'node'
      }
      options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
      }
    }
    ```
    
    registry.test.ucb:9080/front-ucb:ucb-pipe-tls-25
    
    podman run -d -p 8443:8443 --name ucb-front-https -v /u01/ssl:/ssl -v /home/httpd-ssl.conf:/usr/local/apache2/conf/extra/httpd-ssl.conf registry.test.ucb:9080/front-ucb:ucb-pipe-tls-25
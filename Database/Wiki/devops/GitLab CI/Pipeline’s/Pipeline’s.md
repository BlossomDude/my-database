---
Owner: Blossom Dude
Last edited time: 2024-01-29T16:15
Created time: 2024-01-29T10:06
---
> [!important]  
> Полное рук-во по ci.yml файлу  

Пайпланы пишуться в `.gitlab-ci.yml` файле.  
  

- Простой пример пайплайна:
    
    ```Bash
    build-job:
      stage: build
      script:
        - echo "Hello, $GITLAB_USER_LOGIN!"
    
    test-job1:
      stage: test
      script:
        - echo "This job tests something"
    
    test-job2:
      stage: test
      script:
        - echo "This job tests something, but takes more time than test-job1."
        - echo "After the echo commands complete, it runs the sleep command for 20 seconds"
        - echo "which simulates a test that runs 20 seconds longer than test-job1"
        - sleep 20
    
    deploy-prod:
      stage: deploy
      script:
        - echo "This job deploys something from the $CI_COMMIT_BRANCH branch."
      environment: production
    ```
    
- If then в пайплайне.
    
    Using shell variable
    
    ```YAML
    deploy-dev:
    image: testimage
    environment: dev
    tags:
     - kubectl
    script:
     - if [ "$flag" == "true" ]; then MODULE="demo1"; else MODULE="demo2"; fi
     - kubectl apply -f ${MODULE} --record=true
    ```
    
    Using shell variable with yaml multiline block
    
    ```YAML
    deploy-dev:
    image: testimage
    environment: dev
    tags:
      - kubectl
    script:
      - >
        if [ "$flag" == "true" ]; then
          kubectl apply -f demo1 --record=true
        else
          kubectl apply -f demo2 --record=true
        fi
    ```
    
    Using gitlab rules
    
    ```YAML
    workflow:
      rules:
        - if: '$CI_PIPELINE_SOURCE == "schedule"'
          when: never
        - if: '$CI_PIPELINE_SOURCE == "push"'
          when: never
        - when: always
    ```
    
    Using gitlab templates and variables
    
    ```YAML
    demo1-deploy-dev:
      extends: .deploy-dev
      only:
        variables: [ $flag == "true" ]
      variables:
        MODULE: demo1
    
    demo2-deploy-dev:
      extends: .deploy-dev
      only:
        variables: [ $flag == "false" ]
      variables:
        MODULE: demo2
    
    .deploy-dev:
    image: testimage
    environment: dev
    tags:
      - kubectl
    script:
      - kubectl apply -f ${MODULE} --record=true
    ```
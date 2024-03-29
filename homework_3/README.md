# Homework3

## Задание
1. Добавить 2 окружения "preprod" и "production"
2. Добавить отдельные deploy job для каждой среды
3. Добавить переменную "$My Login" внутри .gitlab-ci.yml, которая будет меняться в зависимости от среды.
4. Добавить переменную "$"$My Password" не используя .gitlab-ci.yml, которая так же будет меняться в зависимости от среды.
5. (*) Добавить скрипт в .gitlab-ci.yml, который все запущенные pipeline по названии ветки(ref) их.

> Прислать ссылку на gitlab_ci.yaml либо скриншот gitlab_ci.yaml test stage и скриншот успешно отработанной job-ы test.

## .gitlab-ci.yml
```yaml
image: busybox:latest

stages:
  - build
  - test
  - deploy
  - stop
  - stop previous jobs

variables:
  IMAGE_TAG: $CI_COMMIT_BRANCH-$CI_COMMIT_SHORT_SHA
  CI_DEBUG_TRACE: "true"
  My_Login: $CI_COMMIT_REF_NAME
  My_Password: $${CI_COMMIT_REF_NAME}_password

include:
  local: smoke.gotlab-ci.yml

cache:
  key:
    files:
      - composer.lock
  paths:
    - vendor/

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build1:
  stage: build
  script:
    - echo "Do your build here"
    - echo one >> house.txt
    - mkdir -p vendor/
    - echo "build" > vendor/hello.txt

test1:
  stage: test
  script:
    - echo "Do a test here"
    - echo "For example run a test suite"
  when: always
  environment:
    name: test

deploy to preprod:
  stage: deploy
  variables:
    TARGET_ENV: preprod
  script:
    - echo "Do you deploy here to ${TARGET_ENV}"
    - echo ${DB_SERVER}
  only:
    - homework3
  environment:
    name: preprod
    on_stop: stop to preprod
    auto_stop_in: 1 day

deploy to production:
  stage: deploy
  variables:
    TARGET_ENV: production
  script:
    - echo "Do you deploy here to ${TARGET_ENV}"
    - echo ${DB_SERVER}
  only:
    - homework3
  when: manual
  environment:
    name: production

stop to preprod:
  stage: stop
  variables:
    TARGET_ENV: preprod
  script:
    - echo "STOP ${TARGET_ENV}"
  only:
    - homework3
  when: manual
  environment:
    name: preprod
    action: stop

stop to production:
  stage: stop
  variables:
    TARGET_ENV: production
  script:
    - echo "STOP ${TARGET_ENV}"
  only:
    - homework3
  when: manual
  environment:
    name: production
    action: stop


cancel:
  stage: stop previous jobs
  image: everpeace/curl-jq
  script:
    - |
      if [ "$CI_COMMIT_REF_NAME" == "main" ]
        then
          (
            echo "Cancel old pipelines from the same branch except last"
            OLD_PIPELINES=$( curl -s -H "PRIVATE-TOKEN: $RUNNER_TOKEN" "https://gitlab.com/api/v4/projects/${CI_PROJECT_ID}/pipelines?ref=${CI_COMMIT_REF_NAME}&status=running" \
                  | jq '.[] | .id' | tail -n +2 )
                  for pipeline in ${OLD_PIPELINES}; \
                      do echo "Killing ${pipeline}" && \
                        curl -s --request POST -H "PRIVATE-TOKEN: ${RUNNER_TOKEN}" "https://gitlab.com/api/v4/projects/${CI_PROJECT_ID}/pipelines/${pipeline}/cancel"; done
          ) || echo "Canceling old pipelines (${OLD_PIPELINES}) failed"
      fi


```
![Alt text](<foto/Снимок экрана_1.png>)

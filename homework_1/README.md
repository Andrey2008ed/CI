# Homework1

## Задание
1. Зарегистрироваться в gitlab
2. создать pipeline n runner
3. сохранить артефакт одной из стадий + исключить из папки с артефактами любой файл 
4. сделать любую gitlab pages

## 2. создать pipeline n runner

![Alt text](<fotos/Снимок экрана 2023-12-04 в 22.35.22.png>)

![Alt text](<fotos/Снимок экрана 2023-12-04 в 22.35.44.png>)
## Вопрос касательно runne, акой смысл его запускать локально ? когда можно это сделать в docker или в вебе? 

![Alt text](<fotos/Снимок экрана 2023-12-05 в 23.45.06.png>)

### Пример сразу с артефактами
```yaml
stages:
  - build
  - test
  - deploy
  - pages

image: alpine


build_job:
  stage: build
  script:
    - echo "Building the project..."
  artifacts:
    paths:
      - path/to/artifact
    exclude:
      - path/to/artifact/excluded_file.txt


test_job:
  stage: test
  script:
    - echo "Running tests..."

deploy_job:
  stage: deploy
  script:
    - echo "Deploying the project..."

pages_job:
  stage: deploy
  script:
    - echo "Deploying static websites..."
  artifacts:
    paths:
      - public/
  only:
    - main

create_file_job:
  stage: build
  script:
    - echo "Creating the file..."
  artifacts:
    paths:
      - path/to/file.txt

test_file_job:
  stage: test
  script:
    - echo "Testing the file..."

delete_file_job:
  stage: deploy
  script:
    - echo "Deleting the file..."

pages:
  stage: deploy
  script:
    - echo 'Publishing website to GitLab Pages'

  artifacts:
    paths:
      - public/
    exclude:
      - public/example.txt




```


## 3. сохранить артефакт одной из стадий + исключить из папки с артефактами любой файл 

```yaml
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Building the project..."
  artifacts:
    paths:
      - path/to/artifact
    exclude:
      - path/to/artifact/excluded_file.txt

test_job:
  stage: test
  script:
    - echo "Running tests..."

deploy_job:
  stage: deploy
  script:
    - echo "Deploying the project..."

pages_job:
  stage: deploy
  script:
    - echo "Deploying static websites..."
  artifacts:
    paths:
      - public/
  only:
    - master

create_file_job:
  stage: build
  script:
    - echo "Creating the file..."
  artifacts:
    paths:
      - path/to/file.txt

test_file_job:
  stage: test
  script:
    - echo "Testing the file..."

delete_file_job:
  stage: deploy
  script:
    - echo "Deleting the file..."

```

4. сделать любую gitlab pages

```yaml
stages:
  - build
  - test
  - deploy
  - pages

image: alpine


build_job:
  stage: build
  script:
    - echo "Building the project..."
  artifacts:
    paths:
      - path/to/artifact
    exclude:
      - path/to/artifact/excluded_file.txt

test_job:
  stage: test
  script:
    - echo "Running tests..."

deploy_job:
  stage: deploy
  script:
    - echo "Deploying the project..."

pages_job:
  stage: deploy
  script:
    - echo "Deploying static websites..."
  artifacts:
    paths:
      - public/
  only:
    - master

create_file_job:
  stage: build
  script:
    - echo "Creating the file..."
  artifacts:
    paths:
      - path/to/file.txt

test_file_job:
  stage: test
  script:
    - echo "Testing the file..."

delete_file_job:
  stage: deploy
  script:
    - echo "Deleting the file..."

pages:
  stage: deploy
  script:
    - echo 'Publishing website to GitLab Pages'
    # Add commands to build your website here
  artifacts:
    paths:
      - public/
    exclude:
      - public/example.txt

```



![Alt text](<fotos/Снимок экрана 2023-12-13 в 18.15.13.png>)

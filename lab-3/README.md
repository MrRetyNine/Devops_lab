# Devops_lab_3

## ТЗ
Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь.

## Выполнение работы

Чтобы выполнить лабораторную работу был создан отдельный репозиторий на GitHub https://github.com/MrRetyNine/Test.

В него мы добавили файлы из предыдущей лабораторной работы: хороший докер и файлы, нужные для него (app.py - код на питоне, requirements.txt - связан с управлением зависимостями Python при создании Docker-образа)


![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/64439a6b-fb28-48d9-9620-ae93222b8785)
![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/7908795a-035c-46d7-b852-189f2e1dc601)


```Dockerfile
FROM python:3.10.5

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt && apt-get update

COPY . .

ENTRYPOINT ["python", "./app.py"]
```

Была произведена регистрация в Docker Hub.

![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/3cbcff2e-ca5d-4363-a91d-c2e6e6673f64)

Дальше в репозитории на гитхабе были сделаны следующие шаги:

1. Выбрана вкладка  «Settings» (Настройки) в верхней части страницы;
2. В разделе «Secrets and variables» по очереди выбраны «Actions» и «Secrets»;
3. Созданы два секрета: Docker_username (логин от Docker Hub) и Docker_password (Получение через Docker Hub: после авторизации перейдите в раздел «Account Settings» и выберите «Security», нажмите на кнопку «New Access Token»);
4. Создан новый workflow файл docker-publisher.yml и написан код, который представляет собой GitHub Actions workflow, который автоматизирует процесс сборки и публикации Docker-образа в Docker Hub при выполнении push-событий в ветку "main" репозитория:

```YAML
name: Build and Publish Docker Image

on:
  push:
    branches:
      - 'main'
jobs:
 build-and-publish:
  runs-on: ubuntu-latest
  steps:
    - name: Checkout code
      uses: actions/checkout@v2
    - name: Login to Docker Hub
      uses: docker/login-action@v1
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    - name: Build and Push
      uses: docker/build-push-action@v2
      with:
        context: .
        push: true
        tags: ${{ secrets.DOCKER_USERNAME }}/my-image:latest

```
**Что делает код:**

1. **Событие триггера:** 
   - В этом коде мы определяем, что действие будет выполнено при push-событии в указанную ветку (в данном случае, `'main'`).
   - Это делается с помощью блока on:

   ```yaml
   on:
     push:
       branches:
         - 'main'
   ```

2. **Работа (job) build-and-publish:**
   - Этот блок определяет название работы, которая будет выполняться.
   - Мы указываем, что она будет выполняться на `ubuntu-latest` (последняя версия Ubuntu) в блоке `runs-on`.
   - Каждая работа состоит из шагов (steps), которые определяют действия, выполняемые в рамках данной работы.

   _Вот как это выглядит:_

   ```yaml
   jobs:
     build-and-publish:
       runs-on: ubuntu-latest
       steps:
         ...
   ```

3. **Шаги (steps):**
   - **Checkout code:**
     - В этом шаге используется действие `actions/checkout@v2`, которое клонирует репозиторий с кодом в рабочую директорию для последующей обработки.
   - Login to Docker Hub:
     - В этом шаге используется действие `docker/login-action@v1` для входа в Docker Hub.
     - Он использует секреты (secrets), такие как DOCKER_USERNAME и DOCKER_PASSWORD, которые являются конфиденциальными данными, не разглaшаемыми в репозитории.
   - **Build and Push:**
     - В этом шаге используется действие `docker/build-push-action@v2` для сборки Docker-образа из текущего контекста (context) и его последующей публикации в Docker Hub.
     - Тэг образа (tag) устанавливается как `${{ secrets.DOCKER_USERNAME }}/my-image:latest`.

Actions выполнил все шаги, образ успешно загрузился в репозиторий на Dockerhub. Ниже представлены доказательства:

<img width="733" alt="image" src="https://github.com/MrRetyNine/Devops_lab/assets/112976351/433d46b3-acf0-4b92-bbfb-523433bcae40">
<img width="730" alt="image" src="https://github.com/MrRetyNine/Devops_lab/assets/112976351/ea0c842b-0c00-40fd-b739-e7eeda27f232">

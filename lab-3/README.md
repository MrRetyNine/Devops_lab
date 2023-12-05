# Devops_lab_3

## ТЗ
Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь.

## Выполнение работы

Чтобы выполнить лабораторную работу был создан отдельный репозиторий на GitHub https://github.com/MrRetyNine/Test.

В него мы добавили файлы из предыдущей лабораторной работы.


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

Была произведена регистрация в Docker Hub, дальше в репозитории на гитхабе были сделаны следующие шаги:

1. Выбрана вкладка  «Settings» (Настройки) в верхней части страницы;
2. В разделе «Secrets and variables» по очереди выбраны «Actions» и «Secrets»;
3. Созданы два секрета: Docker_username (логин от Docker Hub) и Docker_password (Получение через Docker Hub: после авторизации перейдите в раздел «Account Settings» и выберите «Security», нажмите на кнопку «New Access Token»);
4. Создан новый workflow файл docker-publisher.yml:

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

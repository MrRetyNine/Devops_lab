# Devops_lab_2

## ТЗ
Написать два Dockerfile – плохой и хороший. Плохой должен запускаться и работать корректно, но в нём должно быть не менее 3 “bad practices”. В хорошем Dockerfile они должны быть исправлены. В Readme описать все плохие практики из кода Dockerfile и почему они плохие, как они были исправлены в хорошем  Dockerfile, а также две плохие практики по использованию этого контейнера

## Выполнение работы

Написана программа на Python, которая использует библиотеку randfacts для получения случайного факта и выводит его на экран, в requirements прописано условие наличия пакета randfacts версии 0.20.2

![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/64439a6b-fb28-48d9-9620-ae93222b8785)
![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/7908795a-035c-46d7-b852-189f2e1dc601)

### Bad Dockerfile
```
FROM python:latest

# set a directory for the app
WORKDIR /app

# copy all the files to the container
COPY requirements.txt .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt
RUN apt-get update

COPY . .


# run the command
CMD ["python", "./app.py"]
```
1) FROM python:latest: В данной строке мы определяем базовый образ, на основе которого строится наш контейнер. Latest версия представляет самую недавнюю версию, что в свою очередь может привести к непредсказуемому поведению при сборке в будущем. Лучше всего использовать конкретную версию образа во избежание неожиданных изменений.

2. WORKDIR /app: Эта команда устанавливает директорию, в которой будет происходить работа внутри контейнера.

3. COPY requirements.txt .: Копируем файл requirements.txt из текущего каталога в директорию /app внутри контейнера.

4. RUN pip install --no-cache-dir -r requirements.txt: Устанавливаем зависимости из файла requirements.txt.

5. RUN apt-get update: Команда apt-get update может привести к дополнительному времени сборки и ненужным зависимостям. 

6. COPY . .: Копируем все файлы из текущего каталога хоста внутрь контейнера в текущую директорию.

7. CMD ["python", "./app.py"]: Команда запускает приложение внутри контейнера.

Основные проблемы:
- Использование latest версии базового образа python. Лучше использовать конкретную версию, чтобы обеспечить стабильность и воспроизводимость сборки в будущем.
- Избыточные слои: две команды RUN подряд.
- CMD вместо ENTRYPOINT в случае запуска с другими аргументами.

![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/0715a5b1-c53e-425f-af99-6323e86a8ca5)

### Good Dockerfile
```
FROM python:3.10.5

# set a directory for the app
WORKDIR /app

# copy all the files to the container
COPY requirements.txt .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt && apt-get update

COPY . .


# run the command
ENTRYPOINT ["python", "./app.py"]
```

- ENTRYPOINT вместо CMD
- Объединение команд RUN
- Выбрана конкретная версия Python вместо latest
  
![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/4f1a44c3-a49b-45e3-a056-987e3b7fb025)

![image](https://github.com/MrRetyNine/Devops_lab/assets/112976351/9b08d86b-088c-4e10-9e2a-10b6bc64b540)

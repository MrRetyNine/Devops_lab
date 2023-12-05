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
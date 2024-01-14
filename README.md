https://habr.com/ru/articles/774032/ - Разработка веб-приложения на Python с использованием FastAPI и Docker

Склонируйте этот репозиторий:
git clone git@github.com:BJIaduk/habr_pyhon_web_docker.git

Установите питон:
sudo apt update
sudo apt install python3
Установите виртуальное окружения для питона:
sudo apt-get install -y python3-venv
Создайте и активируйте виртуальное окружение:
python3 -m venv venv
source venv/bin/activate
Установите pip-tools:
pip install pip-tools
Создайте файл requirements.in и запишите в него строки:
echo "fastapi" > requirements.in
echo "uvicorn" >> requirements.in
Далее создадим файл requirements.txt, в котором появится список версий нужных нам библиотек и всех зависимостей, необходимых для их работы:
pip-compile
И установим все эти зависимости и библиотеки в наше виртуальное окружение:
pip-sync
И можем запускать наше приложение на виртуалке или нашей машине:
uvicorn main:app

Если неободимо запустить приложение в докере:
Установим docker:
sudo apt install docker
Создаем Dockerfile:
  FROM python:yourversion - необходимо записать свою версию питона
  WORKDIR /app
  COPY ./main.py /app
  COPY ./requirements.txt /app
  RUN pip install --no-cache-dir --upgrade -r /app/requirements.txt
  CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "80"]
Создадим докер образ:
docker build -t fastapiapp .
И запустим контейнер, чтобы запустиось наше приложение:
docker run -d --name fastapiapp -p 8000:80 fastapiapp

Если необходимо остановить приложение - останавливаем контейнер:
docker stop fastapiapp
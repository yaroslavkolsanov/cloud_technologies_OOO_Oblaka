# Лабораторная работа 3

### Цель работы

Сделать, чтобы после пуша в ваш репозиторий автоматически собирался докер образ и результат его сборки сохранялся куда-нибудь.

### Ход работы

1. Для выполнения лабораторной работы был создан отдельный [репозиторий](https://github.com/yaroslavkolsanov/Lab3_DevOps). При пуше в этот репозиторий автоматически собирается докер образ и пушится на dockerhub.

![Alt text](./images/repository.png)

2. В директории .github/workflows был создан yml-файл docker-push.yml, благодаря которому при push в ветку main происходит login на dockerhub, build докер образа и его push на dockerhub. Для подключения к dockerhub были созданы секреты USER и PASSWORD, где USER - это логин от аккаунта на dockerhub, а PASSWORD - сгенерированный access token, получение которого показано ниже

![Alt text](./images/get-token.png)
![Alt text](./images/access-token.png)

Содержимое yml-файла

'''
name: Push to Docker hub

on: 
  push:
    branches:
      - 'main'

jobs:
  push_to_docker_hub:
    name: Push to Docker hub
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository 
        uses: actions/checkout@v4
      
      - name: Login to Docker hub 
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.USER }}
          password: ${{ secrets.PASSWORD }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: ${{ secrets.USER }}/lab3_test:latest
'''

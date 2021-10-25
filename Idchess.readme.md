Краткое описание процесса сборки и деплоя

Текущая реализация деплоя основана на github actions и использует self-hosted runner, запущенный на staging сервере. 
Реализация позволяет откатывать, на любое предыдущее состояние в рамках доступности docker образов в хранилище

Деплой состоит из 2х частей:
1) сборки докер докер образа
2) деплоя его на сервер

1. Сборка образа
- Образ собирается из [Doсkerfile](https://github.com/smmarty/idchess_front/blob/main/Dockerfile)
- К сборке добавляется тэги: номер деплоя(action) в github и тег latest
- Образ пушится в реестр гитхаба (пока частный) ghcr.io/ivanvsobolev (см.todo)

2. Деплой сервиса
- Деплой основан на docker-compose. Используется [docker-compose-idchess-front.yml](https://github.com/smmarty/idchess_front/blob/main/docker-compose-idchess-front.yml) 
- перед деплоем подставляется номер версии образа, совпадающий с номером сборки(action) в github
- запуск сервиса используя измененый docker-compose файл



ToDo:
- изменить использование частного гитхаб аккаунта для хранения докер образов (необходимы права владельца репы) [документация](https://docs.github.com/en/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#upgrading-a-workflow-that-accesses-ghcrio)
- добавить деплой в продакшен по доп. заданию

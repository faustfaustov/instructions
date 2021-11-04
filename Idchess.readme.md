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
- Деплой основан на запуске job nomad'а. Конфигурации job nomad'а хранятся в отдельном [репозитории](https://github.com/smmarty/idchess_devops)
- перед деплоем подставляется номер версии образа, совпадающий с номером сборки(action) в github
- запуск сервиса используя измененый hcl файл(конфигурация job)


Ручной запуск сборки и деплоя:

Любой существующий workflow run возможно перезапустить, войдя внутрь выбранного workflow и нажав кнопку **Re-Run all jobs** в правом верхнем углу

ToDo:
- изменить использование частного гитхаб аккаунта для хранения докер образов (необходимы права владельца репы) [документация](https://docs.github.com/en/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#upgrading-a-workflow-that-accesses-ghcrio)
- вынести переменные из actions в секреты github
- добавить деплой в продакшен по доп. заданию

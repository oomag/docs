# Регламент по разработке ПО

Данный регламент содержит общие рекомендации по разработке ПО в команде до
10-12 человек применительно к моделям agile/scrum.


## Общие положения

При разработке ПО, которое предполагает в дальнейшем долгий цикл поддержки,
необходимо придерживаться определенной модели разработки для того, чтобы легче
управлять процессом сборки и развертывания, упростить поиск определенных
изменений кодовой базы и систематизировать процесс выпуска и поддержки ПО.
Данный регламент содержит в себе рекомендации, применимые при типовой
разработке подобного ПО.


## Описание методик работы
### Инструменты

В разработке следует придерживаться максимально распространенных инструментов
для работы с кодовой базой - Git в качестве системы контроля версий, Bitbucket
или Github для хранения кодовой базы, Atlassian Jira или Github issues для
процессом разработки.

### Порядок разработки

Кодовая база должна храниться в одном репозитории. Если разработка предполагает
явное разделение ПО на модули и при этом есть явная зависимость одного модуля
от другого, то эти модули или должны управляться из одного репозитория с
применением Git submodules, или подобная зависимость должна быть устранена на
уровне архитектуры. Дальше по тексту подобные репозитории будут упоминаться как
upstream.

#### Методика слияний изменений

Для внесения изменений в кодовую базу, разработчик должен сделать форк upstream
репозитория (дальше по тексту такой форк будет называться downstream).
Недопустимо:

* создавать не согласованные заранее ветки в upstream репозитории
* делать push в upstream master ветку и согласованные ветки для разработки,
  такие как testing, staging etc.

После того, как разработчик сделал форк upstream репозитория, он должен
клонировать его на локальную машину и проводить необходимые ему изменения в
отдельной ветке. После коммита изменений локально, эта ветка необходимо
выложить как отдельную в downstream репозитории, после чего сделать Pull
Request в upstream репозиторий. После того, как pull request был сделан,
разработчик должен поменять статус тикета в системе управления процессом
разработки (Jira, GH Issues), если это не происходит автоматически. Для
слияния подобного пулл-реквеста с одной из основных веток должны произойти
**все** указанные ниже события:

* Пулл-реквест должен пройти все имеющиеся тесты - smoke, unit, regression,
  intergation etc.
* Пулл-реквест должен пройти ревью как минимум одного Subject Matter Expert с
  положительным результатом

Target branch для слияния пулл-реквеста может выбираться разработчиком
самостоятельно, но в общем случае рекомендуется воздерживаться от слияния
напрямую в master ветку без прохождения stage или testing веток. При слиянии с
testing веткой последняя сливается с master веткой по согласованию в команде
разработки или автоматически по каким-то временным отрезкам.

Данный процесс позволяет:

* Иметь всегда рабочие master и testing ветки за счет прохождения тестов и
  ревью до слияния
* Иметь консистентный набор веток в upstream репозитории
* Позволять разработчикам легко делать ручной squash длинных пулл-реквестов
  за счет возможности делать force push в downstream репозитории

#### Версионирование

Версионирование рекомендуется производить согласно стандарту semver. Краткая
выдержка того, как это должно происходить:
учитывая номер версии МАЖОРНАЯ.МИНОРНАЯ.ПАТЧ, следует увеличивать:

* МАЖОРНУЮ версию, когда сделаны обратно несовместимые изменения API.
* МИНОРНУЮ версию, когда вы добавляете новую функциональность, не нарушая
  обратной совместимости.
* ПАТЧ-версию, когда вы делаете обратно совместимые исправления.

Дополнительные обозначения для предрелизных и билд-метаданных возможны как
дополнения к МАЖОРНАЯ.МИНОРНАЯ.ПАТЧ формату. Более подробное описание можно
посмотреть на [сайте semver][1].

[1]: https://semver.org/lang/ru/

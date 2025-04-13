# Проектная работа спринта 8

BionicPRO — российская компания, которая производит и продаёт бионические протезы.

В компании пять отделов:
   - Отдел продаж.
   - Отдел производства и эксплуатации.
   - Отдел маркетинга.
   - Отдел машинного обучения (ML).
   - Отдел разработки.

В IT-системе BionicPRO четыре ключевых компонента:
   - Программа в чипе протеза.
   - Приложение для донастройки протеза.
   - Интернет-магазин.
   - CRM.

**Проблемы и бизнес-задачи компании:**

Ранее технология SSO компании реализовывалась через Oauth2.0 Code Grant в Keycloak. С тех пор компанию успели взломать. Хакеры воспользовались уязвимостью и скачали персональные данные пользователей по всем протезам. Ходит слух, что утечка дошла до конкурентов BionicPRO и они уже знают, сколько активных пользователей есть у компании. Когда информация о взломе достигла СМИ, клиенты BionicPRO подали в суд на компанию. Если не урегулировать конфликт в досудебном порядке, есть риск, что компании придётся заплатить большие штрафы. Директор BionicPRO в срочном порядке созвал собрание руководителей. В ходе обсуждения наивысший приоритет отдали задачам по усилению безопасности. Также нужно учесть, что пользователи узнали о сборе данных и теперь хотят получать больше информации о работе своих протезов. Они требуют, чтобы разработчики добавили в приложение возможность скачать отчёт. Чтобы урегулировать судебные иски, компания пошла на уступки и пообещала предоставить такую функциональность для пользователей Android-приложения.

Вот ещё несколько важных проблем, с которыми столкнулась компания:
   - Огромный объём данных. Изначально у компании была только одна база данных — PostgreSQL. Очень быстро она начала ухудшать показатели работы системы. Команда попробовала использовать индексы, но это не помогло: большое количество данных начало влиять на остальные таблицы базы.
   - Выход на новые рынки и соблюдение требований законодательства. Компания сейчас работает только в РФ, но планирует осваивать новые рынки и развивать свои продукты в других странах. BionicPRO работает с медицинскими данными, и во многих странах есть требования к хранению такой информации. Нужно учитывать, что правовое регулирование в разных странах может отличаться. Важно продумать, как будут храниться данные о пациентах, в том числе — авторизационные.

**Цели бизнеса:**
   - Злоумышленники не могут использовать уязвимость SSO в приложении.
   - Пользователи могут скачать данные о работе протеза в виде отчёта. Чтобы реализовать эту функциональность, нужно выгружать данные из CRM в ClickHouse. Для этого требуется написать отдельное приложение, которое сможет предоставлять отчёт из нескольких источников — CRM и DB.
   - Пользователь должен иметь доступ только к тем отчётам, которые содержат данные о его протезе или протезах. У него не должно быть доступа к информации о других пользователях.

**Необходимо принять архитектурные решения для решения проблем и достижения целей компании.**


## Запуск проекта

1. Выполнить в терминале в корневой папке проекта:

```shell
docker compose up -d
```

2. Дождаться поднятия контейнеров с сервисами.

3. Открыть в браузере http://localhost:3000

## Проверка

1. Залогиниться под пользователем с ролью prothetic_user (например, prothetic1). В payload запроса на получение access_token должен присутствовать code_verifier.

2. Нажать кнопку "Download report" и убедиться, что появилось сообщение о загрузке отчёта.

3. Завершить сессию текущего пользователя (можно через интерфейс Keycloak - http://localhost:8080/, меню Sessions).

4. Залогиниться под пользователем с ролью, отличной от prothetic_user (например, user1).

5. Нажать кнопку "Download report" и убедиться, что отчёт не загружается (Ошибка: 403).

## Остановка проекта

Выполнить в терминале:

```shell
docker compose down
```

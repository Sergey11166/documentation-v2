---
title: Авторизация запросов к Облаку Эвотор
keywords:
summary:
sidebar: evorest
permalink: doc_authorization.html
tags:
---

Все запросы к Облаку Эвотор осуществляются в рамках приложения. После установки приложения в [Личном кабинете](https://market.evotor.ru/#/store/auth/login), Облако присваивает ему *токен Облака Эвотор*, уникальный для пары приложение-пользователь.

Передавайте этот токен в заголовке `Authorization`, чтобы авторизовать [свои запросы к Облаку](./doc_example_calls.html#evotorApi).

## Как получить токен Облака Эвотор

Вы можете получить токен Облака Эвотор:

* [вручную](./doc_authorization.html#byhand);
* [автоматически](./doc_authorization.html#automated).

  Для автоматического получения токена Облака Эвотор вам потребуется поддержать собственный сервис. В документации такой сервис называется *сторонним*.

### Получение токена вручную {#byhand}

*Чтобы получить токен Облака Эвотор вручную:*

1. Войдите в [Личный кабинет](https://dev.evotor.ru/#/store/apps) на сайте разработчиков и выберите [созданное приложение](./doc_evotor_api_introduction.html).

2. Выберите необходимый черновик и перейдите на вкладку **Интеграция**.

3. В разделе **Настройки приложения**, включите опцию **Создать вкладку "Настройки" на странице приложения**.

   Опция добавляет вкладку **Настройки** для приложения установленного в [Личном кабинете пользователя Эвотор](https://market.evotor.ru/#/store/auth/login).

4. Нажмите кнопку **Текстовое поле**, в поле **Значение поля** укажите `${token}`.

5. Сохраните изменения, вернитесь к списку черновиков приложения и переведите необходимый черновик в тестирование.

6. Войдите в [Личный кабинет](https://market.evotor.ru/#/store/auth/login) в Магазине приложений.

7. На вкладке **Мои приложения**, выберите приложение, в котором были сделаны изменения.

8. Перейдите на вкладку **Настройки**.

   На вкладке отображается поле, которое содержит токен Облака Эвотор, необходимый для авторизации запросов к Облаку. Передавайте токен в заголовке `Authorization`.

   {% include image.html file="app_token.png" max-width="" border="" url="/images/app_token.png" caption="" alt="" %}

### Автоматическое получение токена {#automated}

 Автоматически вы можете получить токен Облака Эвотор одним из двух способов:

* На адрес [`partner.ru/api/v1/user/token`](https://api.evotor.ru/docs/#tag/Vebhuki-zaprosy%2Fpaths%2F~1partner.ru~1api~1v1~1user~1token%2Fpost);

* В GET-параметре `token` веб-адреса iframe-приложения.

*Чтобы получить токен Облака Эвотор на адрес `partner.ru/api/v1/user/token`:*

1. Поддержите в своём сервисе адрес `partner.ru/api/v1/user/token`.

   На указанный адрес Облако передаёт токен Облака Эвотор после его установки в [Личном кабинете](https://market.evotor.ru/#/store/auth/login) пользователя. Запросы облака необходимо авторизовать с помощью [*токена приложения стороннего сервиса*](https://developer.evotor.ru/docs/doc_evotor_api_authorization.html#serverToken).

   {% include note.html content="Токен приложения стороннего сервиса – токен, с помощью которого ваш сервис авторизует запросы от Облака Эвотор"%}

2. Войдите в [Личный кабинет](https://dev.evotor.ru/#/store/apps) на сайте разработчиков и выберите [созданное приложение](./doc_evotor_api_introduction.html).

3. Выберите необходимый черновик и перейдите на вкладку **Интеграция**.

4. Включите опцию **Токен приложения для доступа к REST API Эвотор**.

5. В поле **URL**, укажите адрес вида `partner.ru/api/v1/user/token`, на который Облако будет передавать токен Облака Эвотор.

6. Выберите тип авторизации, с помощью которого ваш сервис будет авторизовать [передачу токена Облака Эвотор](https://api.evotor.ru/docs/#tag/Vebhuki-zaprosy%2Fpaths%2F~1partner.ru~1api~1v1~1user~1token%2Fpost).

   * Если вы выбрали **С помощью токена**. В поле **Токен**, укажите токен приложения стороннего сервиса в формате `uuid4`.

      Облако передаёт токен в заголовке `Authorization`.

   * Если вы выбрали **Запрос логина и пароля ( Basic Auth )**. В полях **Логин** и **Пароль** укажите данные для доступа к стороннему сервису.

      Облако кодирует логин и пароль с помощью base64 и передаёт закодированные данные в заголовке `Authorization`.

   Подробное описание запроса на передачу токена Облака Эвотор вы найдёте в [Справочнике API](https://api.evotor.ru/docs/#tag/Vebhuki-zaprosy%2Fpaths%2F~1partner.ru~1api~1v1~1user~1token%2Fpost). Полученный токен передавайте в заголовке `Authorization`, в своих запросах к Облаку.

7. Сохраните изменения, вернитесь к списку черновиков приложения и переведите необходимый черновик в тестирование.

После того, как пользователь Магазина приложений установит ваше приложение, Облако будет передавать в ваш сервис токен Облака Эвотор.

*Чтобы получить токен Облака Эвотор в GET-параметре `token` веб-адреса iframe-приложения:*

1. Войдите в [Личный кабинет](https://dev.evotor.ru/#/store/apps) на сайте разработчиков и выберите [созданное приложение](./doc_evotor_api_introduction.html).

2. Выберите необходимый черновик и перейдите на вкладку **Интеграция**.

3. В разделе **Настройки приложения**, включите опцию **Создать вкладку "Настройки" на странице приложения**.

4. Нажмите на **iframe** и укажите необходимые поля.

5. Сохраните изменения, вернитесь к списку черновиков приложения и переведите необходимый черновик в тестирование.

Iframe будет добавлен на вкладку **Настройки**, на странице установленного приложения. Облако передаёт токен Облака Эвотор в GET-параметре `token` веб-адреса iframe:

```shell
https://partner.org/#/?uid=<example>&token=string
```

{% include image.html file="iframe_token.png" url="images/iframe_token.png" caption="Токен из iframe" %}

---
## Front matter
lang: ru-RU
title: Лабораторная работа № 1-D
subtitle: Защита корпоративного мессенджера
author:
  - Доберштейн А. С., Оразгелдиев Я. О., Барабанова К. А.
institute:
  - Российский университет дружбы народов, Москва, Россия

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \usetheme{metropolis}
 - \usepackage{fontspec}
 - \usepackage{polyglossia}
 - \setdefaultlanguage{russian}
 - \setmainfont{FreeSerif}
 - \setsansfont{FreeSerif}
 - \setmonofont{FreeSerif}
 - \usepackage{amsmath}
 - \usepackage{amssymb}
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Доберштейн А. С., Оразгелдиев Я. О., Барабанова К. А.
  * НФИбд-02-22
  * Российский университет дружбы народов

:::

::::::::::::::

## Цель работы

Основной целью работы является получение навыков обнаружения и устранение уязвимостей  WordPress-wpDiscuz, Proxylogon, Rocket.Chat и их последствий.

## Выполнение лабораторной работы

Для начала изучили вектор атаки, адреса злоумышленника и атакуемых серверов.

![Вектор атаки](image/1.png){#fig:001 width=70%}

## Уязвимость WordPress-wpDiscuz

Залогинились в ViPNet для обнаружения уязвимости в журнале событий.

![Вход в ViPNet](image/2.png){#fig:002 width=70%}

## Уязвимость WordPress-wpDiscuz

В "Событиях" обнаружили событие AM Exploit Wordpress с программным кодом, предназначенным для эксплуатации уязвимости

![Журнал событий](image/3.png){#fig:003 width=70%}

## Уязвимость WordPress-wpDiscuz

Изучили информацию по CVE-коду об обнаруженной уязвимости, изучили рекомендации по нейтрализации. 

![Обзор уязвимости](image/4.png){#fig:004 width=70%}

## Уязвимость WordPress-wpDiscuz

Для устранения уязвимости подключились к удаленному рабочему столу по адресу 10.140.2.180

![Подключение к удаленному рабочему столу](image/5.png){#fig:005 width=70%}

## Уязвимость WordPress-wpDiscuz

Вошли под указанной учетной записью. 

![Вход](image/6.png){#fig:006 width=70%}

## Уязвимость WordPress-wpDiscuz

В соответствии с вектором атаки в KeePass нашли CMS WordPress. 

![KeePass](image/7.png){#fig:007 width=70%}

## Уязвимость WordPress-wpDiscuz

Просмотрели сайт WordPress по указанному адресу. Здесь обнаружили последствие - Deface - изменение внешнего вида интерфейса. 

![Deface](image/8.png){#fig:008 width=70%}

## Уязвимость WordPress-wpDiscuz

В панели администрирования перешли во вкладку с плагинами и деактивировали плагин wpDiscuz 

![Плагин wpDiscuz](image/9.png){#fig:009 width=70%}

## Уязвимость WordPress-wpDiscuz

Для того, чтобы устранить последствие Deface, необходимо откатить сайт до предыдущей резервной копии. Для этого перешли в панель администрирования и во вкладке с плагинами нашли плагин UpdraftPlus - Backup/Restore, перешли в "Settings".

![Плагин UpdraftPlus - Backup/Restore](image/10.png){#fig:010 width=70%}

## Уязвимость WordPress-wpDiscuz

Выбрали последнюю резервную копию и нажали "Restore". 

![Restore](image/11.png){#fig:011 width=70%}

## Уязвимость WordPress-wpDiscuz

Поставили флажки у компонентов "Themes" и "Uploads". 

![Параметры восстановления](image/12.png){#fig:012 width=70%}

## Уязвимость WordPress-wpDiscuz

Во всплывшем окне с ошибкой нажали "Удалить старые директории"

![Удаление старых директорий](image/13.png){#fig:013 width=70%}

## Уязвимость WordPress-wpDiscuz

Когда директории удалились, нажали "Return to UpdraftPlus configuration"

![Восстановление версии](image/14.png){#fig:014 width=70%}

## Уязвимость WordPress-wpDiscuz

Обновили страницу сайта. Убедились, что последствие Deface успешно устранено.

![Устранено последствие](image/15.png){#fig:015 width=70%}

## Уязвимость WordPress-wpDiscuz

Перешли в Putty web-portal, чтобы проверить сокеты на наличие подозрительных процессов с помощью утилиты ss.

![Проверка процесов](image/16.png){#fig:016 width=70%}

## Уязвимость WordPress-wpDiscuz

Уничтожили вредоносные соединения с помощью команды kill {pid}. Убедились в их отсутствии.

![Уничтожение вредоносных процессов](image/17.png){#fig:017 width=70%}

## Уязвимость WordPress-wpDiscuz

Первая уязвимость с ее последствием успешно устранены

![Успех](image/18.png){#fig:018 width=70%}

## Уязвимость Proxylogon

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий.

![Журнал событий](image/19.png){#fig:019 width=70%}

## Уязвимость Proxylogon

Изучили информацию об обнаруженной уязвимости.

![Обзор уязвимости](image/20.png){#fig:020 width=70%}

## Уязвимость Proxylogon

В соответствии с вектором атаки в KeePass нашли MS Exchange.

![KeePass](image/21.png){#fig:021 width=70%}

## Уязвимость Proxylogon

Подключились к удаленному рабочему столу по адресу в соответствии с вектором атаки. Открыли Internet Information Services Manager. 

![inetmgr](image/22.png){#fig:022 width=70%}

## Уязвимость Proxylogon

Перешли в /MAIL/Sites/Default Web Site/ecp

![inetmgr](image/23.png){#fig:023 width=70%}

## Уязвимость Proxylogon

Перешли в IP Address and Domain Restrictions, в "Actions" выбрали "Edit Feature Settings", в открывшемся окне в параметре "Access for unspecified clients" выбрали "Deny".

![inetmgr](image/24.png){#fig:024 width=70%}

## Уязвимость Proxylogon

Далее открыли терминал, чтобы обнаружить вредоносные процессы с помощью утилиты netstat.

![Вредоносные процессы](image/25.png){#fig:025 width=70%}

## Уязвимость Proxylogon

![Вредоносные процессы](image/26.png){#fig:026 width=70%}

## Уязвимость Proxylogon

Остановили эти процессы и проверили их отсутствие.

![Вредоносные процессы](image/27.png){#fig:027 width=70%}

## Уязвимость Proxylogon

Далее в директории /C:/Program Files/Microsoft/Exchange Server/V15/FrontEnd/HttpProxy/owa/auth удалили файл AM_Backdoor.aspx

![Удаление файла .aspx](image/28.png){#fig:028 width=70%}

## Уязвимость Proxylogon

Уязвимость Proxylogon и ее последствие China Chopper успешно устранены.

![Успех](image/29.png){#fig:029 width=70%}

## Уязвимость Rocket.Chat

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. 

![Журнал событий](image/30.png){#fig:030 width=70%}

## Уязвимость Rocket.Chat

Изучили информацию об обнаруженной уязвимости.

![Обзор уязвимости](image/31.png){#fig:031 width=70%}

## Уязвимость Rocket.Chat

В соответствии с вектором атаки в KeePass нашли RocketChat.

![KeePass](image/32.png){#fig:032 width=70%}

## Уязвимость Rocket.Chat

Открыли веб-версию Rocket.Chat и нажали на сброс пароля для указанной учетной записи.

![Сброс пароля](image/33.png){#fig:033 width=70%}

## Уязвимость Rocket.Chat

На почту администратора Rocket.Chat было направлено email-письмо с инструкциями по сбросу пароля.

![Сообщение об отправке email](image/34.png){#fig:034 width=70%}

## Уязвимость Rocket.Chat

В консоли от администратор просмотрели это письмо. Скопировали ссылку со сгенерированным токеном для сброса пароля. 

![Инструкция по сбросу пароля](image/35.png){#fig:035 width=70%}

## Уязвимость Rocket.Chat

Перешли по скопированному адресу в браузере

![Сброс пароля](image/36.png){#fig:036 width=70%}

## Уязвимость Rocket.Chat

Задали новый пароль для пользователя

![Сброс пароля](image/37.png){#fig:037 width=70%}

## Уязвимость Rocket.Chat

Просмотрели /home/user/backup_codes для прохождения двухфакторной аутентификации при сбросе пароля. 

![Backup_codes](image/38.png){#fig:038 width=70%}

## Уязвимость Rocket.Chat

Ввели один из них в соответствующее поле ввода.

![Сброс пароля](image/39.png){#fig:039 width=70%}

## Уязвимость Rocket.Chat

Залогинились с измененным паролем. 

![Вход в учетную запись](image/40.png){#fig:040 width=70%}

## Уязвимость Rocket.Chat

В панели администриования перешли в "Права доступа", и для роли "User" поставили флажок "User must use Two factor Authentication".

![Права доступа](image/41.png){#fig:041 width=70%}

## Уязвимость Rocket.Chat

Перешли во вкладку "Учетные записи" и настроили подтверждение адреса электронной почты при регистрации для роли "User" и автоматическую настройку двухфакторной аутентификации по электронной почте для новых пользователей.

![Настройки двухфакторной аутентификации](image/42.png){#fig:042 width=70%}

## Уязвимость Rocket.Chat

![Настройки регистрации](image/43.png){#fig:043 width=70%}

## Уязвимость Rocket.Chat

В терминале администратора Rocket.Chat-server отредактировали файл mongod.conf, расскомментировав параметр "security:" и прописав отключение выполнения JavaScript на стороне сервера базы данных "javascriptEnabled: False".

![Редактирование mongod.conf](image/44.png){#fig:044 width=70%}

## Уязвимость Rocket.Chat

Перезапустили службу mongod.service

![Restart mongod.service](image/45.png){#fig:045 width=70%}

## Уязвимость Rocket.Chat

С помощью утилиты ss обнаружили и остановили вредоносные процессы.

![Уничтожение вредоносных соединений](image/46.png){#fig:046 width=70%}

## Уязвимость Rocket.Chat

Уязвимость RocketChat RCE и ее последствие meterpreter успешно устранены.

![Успех](image/47.png){#fig:047 width=70%}

## Завершение выполнения лабораторной работы

Заполнили карточки инцидентов для уязвимостей и их последствий.

![Карточки инцидентов](image/48.png){#fig:048 width=70%}

## Выводы

В результате выполнения лабораторной работы мы получили навыки обнаружения и устранение уязвимостей  WordPress-wpDiscuz, Proxylogon, Rocket.Chat и их последствий.

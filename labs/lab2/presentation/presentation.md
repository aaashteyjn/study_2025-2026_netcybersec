---
## Front matter
lang: ru-RU
title: Лабораторная работа № 2-D
subtitle: Защита интеграционной платформы
author:
  - Доберштейн А., Оразгелдиев Я., Лобанова П., Лушин А., Барабанова К.
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

  * Доберштейн А., Оразгелдиев Я., Лобанова П., Лушин А., Барабанова К.
  * НФИбд-02-22
  * Российский университет дружбы народов

:::

::::::::::::::

## Цель работы

Основной целью работы является получение навыков обнаружения и устранение уязвимостей Bitrix vote RCE, GitLab RCE, WSO2 API-Manager RCE и их последствий.

## Выполнение лабораторной работы

Для начала изучили вектор атаки, адреса злоумышленника и атакуемых серверов.

![Вектор атаки](image/1.png){#fig:001 width=70%}

## Уязвимость Bitrix vote RCE

Залогинились в ViPNet для обнаружения уязвимости в журнале событий.

![Вход в ViPNet](image/2.png){#fig:002 width=70%}

## Уязвимость Bitrix vote RCE

В "Событиях" обнаружили события: внедрение полезной нагрузки в HTTP-запрос, PHP-скрипт с кодом для удаленного выполнения команд, информирование о скачивании исполняемого файла с машины нарушителя. 

![Журнал событий](image/3.png){#fig:003 width=70%}

## Уязвимость Bitrix vote RCE

Изучили информацию по CVE-коду об обнаруженной уязвимости, изучили рекомендации по нейтрализации. 

![Обзор уязвимости](image/4.png){#fig:004 width=70%}

## Уязвимость Bitrix vote RCE

Для устранения уязвимости подключились к удаленному рабочему столу. 

![Подключение к удаленному рабочему столу](image/5.png){#fig:005 width=70%}

## Уязвимость Bitrix vote RCE

Вошли под указанной учетной записью. 

![Вход](image/6.png){#fig:006 width=70%}

## Уязвимость Bitrix vote RCE

В соответствии с вектором атаки в KeePass нашли CMS Bitrix. 

![KeePass](image/7.png){#fig:007 width=70%}

## Уязвимость Bitrix vote RCE

В лог-файле apache2 по пути /var/log/apache2/access.log обнаружили следующую информацию: два запроса к файлу /bitrix/tools/vote/uf.php с внедрением полезной нагрузки для последующей загрузки веб-backdoor и запрос к файлу веб-backdoor для создания WebShell сессии с машиной нарушителя.

![Лог-файл](image/8.png){#fig:008 width=70%}

## Уязвимость Bitrix vote RCE

Произвели поиск по названию полезной нагрузки с помощью команды find /var/www/html/ -iname «payload2.phar», нашли данный файл.

![Поиск файла](image/9.png){#fig:009 width=70%}

## Уязвимость Bitrix vote RCE

Просмотрели содержимое с помощью текстового редактора, отобразилась информация о скачивании веб-backdoor по пути /var/www/html/caidao.php.

![Информация о полезной нагрузке](image/10.png){#fig:010 width=70%}

## Уязвимость Bitrix vote RCE

Открыли сайт Bitrix. Не удалось получить доступ к интерфейсу администрирования из-за действующей полузной нагрузки. 

![Попытка входа в Bitrix](image/11.png){#fig:011 width=70%}

## Уязвимость Bitrix vote RCE

Для устранения вектора для локального повышения привелегий (LPE) удалили SUID-бит у файла /var/www/html/apache_restart с
помощью команды chmod –s /var/www/html/apache_restart. 

![Устранение LPE](image/12.png){#fig:012 width=70%}

## Уязвимость Bitrix vote RCE

Для закрытия уязвимости добавиkb изменения в файл /var/www/html/bitrix/tools/vote/uf.php, перед require_once и между знаков вопроса вставили код:

![Редактирование uf.php](image/13.png){#fig:013 width=70%}

## Уязвимость Bitrix vote RCE

Создали файл .htaccess в директории /var/www/html/bitrix/tools/vote, задающий правила работы веб-сервера для конкретного каталога и подкаталогов. Для закрытия уязвимости в данном файле можно прописали команду deny from all.

![Создание .htaccess](image/14.png){#fig:014 width=70%}

## Уязвимость Bitrix vote RCE

С помощью утилиты ss и команды kill закрыли meterpreter сессии.

![Закрытие meterpreter сессий](image/15.png){#fig:015 width=70%}

## Уязвимость Bitrix vote RCE

В директории веб-сервера обнаружили скрипт password_recovery.php.

![Директория веб-сервера](image/16.png){#fig:016 width=70%}

## Уязвимость Bitrix vote RCE

Прописали новый пароль.

![password_recovery.php](image/17.png){#fig:017 width=70%}

## Уязвимость Bitrix vote RCE

Подключились к веб-серверу, в ссылке указали название данного файла. 

![Deface веб-сервера](image/18.png){#fig:018 width=70%}

## Уязвимость Bitrix vote RCE

Авторизовались с правами администратора 

![Авторизация](image/19.png){#fig:019 width=70%}

## Уязвимость Bitrix vote RCE

Открылась панель администрирования. 

![Администрирование](image/20.png){#fig:020 width=70%}

## Уязвимость Bitrix vote RCE

Удалили файл password_recovery.php. 

![Удаление файла](image/21.png){#fig:021 width=70%}

## Уязвимость Bitrix vote RCE

Доступ к панели администрирования восстановлен. Удалили все файлы в директории взломанного веб-сервера. 

![Удаление файлов](image/22.png){#fig:022 width=70%}

## Уязвимость Bitrix vote RCE

Файл резервной копии разархивировали в директорию /var/www/html с помощью команды tar xvzf /var/bitrix_backups/Bitrix_full_backup.tar.gz -C /var/www/html. 

![Резервная копия](image/23.png){#fig:023 width=70%}

## Уязвимость Bitrix vote RCE

Далее повторили действия по устранению полезной нагрузки: 
Для устранения вектора для локального повышения привелегий (LPE) удалили SUID-бит у файла /var/www/html/apache_restart с
помощью команды chmod –s /var/www/html/apache_restart. 

![Устранение LPE](image/12.png){#fig:024 width=70%}

## Уязвимость Bitrix vote RCE

Для закрытия уязвимости добавиkb изменения в файл /var/www/html/bitrix/tools/vote/uf.php, перед require_once и между знаков вопроса вставили код:

![Редактирование uf.php](image/13.png){#fig:025 width=70%}

## Уязвимость Bitrix vote RCE

Создали файл .htaccess в директории /var/www/html/bitrix/tools/vote, задающий правила работы веб-сервера для конкретного каталога и подкаталогов. Для закрытия уязвимости в данном файле можно прописали команду deny from all

![Создание .htaccess](image/14.png){#fig:026 width=70%}

## Уязвимость Bitrix vote RCE

Удалили файл /var/www/html/apache_restart.

![Удаление файла](image/24.png){#fig:027 width=70%}

## Уязвимость Bitrix vote RCE

Уязвимость с ее последствием успешно устранены 

![Успех](image/25.png){#fig:028 width=70%}

## Уязвимость GitLab RCE

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. Изучили информацию об обнаруженной уязвимости.

![Обзор уязвимости](image/29.png){#fig:029 width=70%}

## Уязвимость GitLab RCE

В соответствии с вектором атаки в KeePass нашли GitLab. 

![KeePass](image/30.png){#fig:030 width=70%}

## Уязвимость GitLab RCE

Подключились к удаленному рабочему столу по адресу в соответствии с вектором атаки. Открыли веб-интерфейс GitLab и авторизовались под учетной записью администратора.  

![GitLab](image/31.png){#fig:031 width=70%}

## Уязвимость GitLab RCE

Перешли на страницу Admin Area.  

![Admin Area](image/32.png){#fig:032 width=70%}

## Уязвимость GitLab RCE

В левой панели инструментов перешли во вкладку Settings – General. 

![Settings -> General](image/33.png){#fig:033 width=70%}

## Уязвимость GitLab RCE

в настройках нашли пункт Sign-up restrictions и нажали кнопку Expand.  

![Sign-up restrictions](image/34.png){#fig:034 width=70%}

## Уязвимость GitLab RCE

Настроили конфигурацию, разрешающую регистрацию новых аккаунтов только с одобрения адмнистратора. 

![Настройка](image/35.png){#fig:035 width=70%}

## Уязвимость GitLab RCE

Сохранили конфигурацию.  

![Сохранение конфигурации](image/36.png){#fig:036 width=70%}

## Уязвимость GitLab RCE

В панели администратора перешли во вкладку Users 

![Users](image/37.png){#fig:037 width=70%}

## Уязвимость GitLab RCE

В строке с пользователем Script Kiddie нажали Delete user and contributions.  

![Удаление пользователя](image/38.png){#fig:038 width=70%}

## Уязвимость GitLab RCE

Подтвердили удаление.  

![Подтверждение удаления](image/39.png){#fig:039 width=70%}

## Уязвимость GitLab RCE

С помощью утилиты ss и команды kill закрыли meterpreter сессии. 

![Закрытие meterpreter сессий](image/40.png){#fig:040 width=70%}

## Уязвимость GitLab RCE

Уязвимость с ее последствием успешно устранены 

![Успех](image/41.png){#fig:041 width=70%}

## Уязвимость WSO2 API-Manager RCE

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. 

![Журнал событий](image/42.png){#fig:042 width=70%}

## Уязвимость WSO2 API-Manager RCE

Изучили информацию об обнаруженной уязвимости.

![Обзор уязвимости](image/43.png){#fig:043 width=70%}

## Уязвимость WSO2 API-Manager RCE

В соответствии с вектором атаки в KeePass нашли API-Manager. 

![KeePass](image/44.png){#fig:044 width=70%}

## Уязвимость WSO2 API-Manager RCE

Открыли файл конфигурации WSO2 API-Manager и добавили в конец запись resource.access_control.

![Файл конфигурации](image/45.png){#fig:045 width=70%}

## Уязвимость WSO2 API-Manager RCE

![Редактирование](image/46.png){#fig:046 width=70%}

## Уязвимость WSO2 API-Manager RCE

Удалили загруженный exploit.jsp файл по пути /opt/wso2am-4.0.0/repository/deployment/server/webapps/authentic
ationendpoint. 

![Удаление файла](image/47.png){#fig:047 width=70%}

## Уязвимость WSO2 API-Manager RCE

Удалили сгенерированный файл /tmpp/payload.elf. 

![Удаление файла](image/48.png){#fig:048 width=70%}

## Уязвимость WSO2 API-Manager RCE

С помощью утилиты ss и команды kill закрыли meterpreter сессии.

![Закрытие meterpreter сессий](image/49.png){#fig:049 width=70%}

## Уязвимость WSO2 API-Manager RCE

Зашли в веб-интерфейс WSO2 API-Manager по ссылке https://10.10.2.27:9443/carbon и авторизовались под учетной записью администратора. 

![Вход в веб-интерфейс](image/50.png){#fig:050 width=70%}

## Уязвимость WSO2 API-Manager RCE

Просмотрели список пользователей. 

![Пользователи](image/51.png){#fig:051 width=70%}

## Уязвимость WSO2 API-Manager RCE

Удалили пользователя hacker.

![Удаление пользователя](image/52.png){#fig:052 width=70%}

## Уязвимость WSO2 API-Manager RCE

Уязвимость с ее последствием успешно устранены. 

![Успех](image/53.png){#fig:053 width=70%}

## Выводы

В результате выполнения лабораторной работы мы получили навыки обнаружения и устранение уязвимостей Bitrix vote RCE, GitLab RCE, WSO2 API-Manager RCE и их последствий.

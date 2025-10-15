---
## Front matter
title: "Лабораторная работа № 2-D"
subtitle: "Защита интеграционной платформы"
author: "Доберштейн А., Оразгелдиев Я., Лобанова П., Лушин А., Барабанова К."

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: FreeSerif
romanfont: FreeSerif
sansfont: FreeSerif
monofont: FreeSerif
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Основной целью работы является получение навыков обнаружения и устранение уязвимостей Bitrix vote RCE, GitLab RCE, WSO2 API-Manager RCE и их последствий.

# Выполнение лабораторной работы

## Подготовка к выполнению лабораторной работы

Для начала изучили вектор атаки, адреса злоумышленника и атакуемых серверов.(рис. [-@fig:001]).

![Вектор атаки](image/1.png){#fig:001 width=70%}

## Уязвимость Bitrix vote RCE

Залогинились в ViPNet для обнаружения уязвимости в журнале событий.(рис. [-@fig:002]).

![Вход в ViPNet](image/2.png){#fig:002 width=70%}

В "Событиях" обнаружили события: внедрение полезной нагрузки в HTTP-запрос, PHP-скрипт с кодом для удаленного выполнения команд, информирование о скачивании исполняемого файла с машины нарушителя. (рис. [-@fig:003]).

![Журнал событий](image/3.png){#fig:003 width=70%}

Изучили информацию по CVE-коду об обнаруженной уязвимости, изучили рекомендации по нейтрализации. (рис. [-@fig:004]).

![Обзор уязвимости](image/4.png){#fig:004 width=70%}

Для устранения уязвимости подключились к удаленному рабочему столу. (рис. [-@fig:005]).

![Подключение к удаленному рабочему столу](image/5.png){#fig:005 width=70%}

Вошли под указанной учетной записью. (рис. [-@fig:006]).

![Вход](image/6.png){#fig:006 width=70%}

В соответствии с вектором атаки в KeePass нашли CMS Bitrix.(рис. [-@fig:007]). 

![KeePass](image/7.png){#fig:007 width=70%}

В лог-файле apache2 по пути /var/log/apache2/access.log обнаружили следующую информацию: два запроса к файлу /bitrix/tools/vote/uf.php с внедрением полезной нагрузки для последующей загрузки веб-backdoor и запрос к файлу веб-backdoor для создания WebShell сессии с машиной нарушителя.(рис. [-@fig:008]).

![Лог-файл](image/8.png){#fig:008 width=70%}

Произвели поиск по названию полезной нагрузки с помощью команды find /var/www/html/ -iname «payload2.phar», нашли данный файл.(рис. [-@fig:009]).

![Поиск файла](image/9.png){#fig:009 width=70%}

Просмотрели содержимое с помощью текстового редактора, отобразилась информация о скачивании веб-backdoor по пути /var/www/html/caidao.php.(рис. [-@fig:010]).

![Информация о полезной нагрузке](image/10.png){#fig:010 width=70%}

Открыли сайт Bitrix. Не удалось получить доступ к интерфейсу администрирования из-за действующей полузной нагрузки. (рис. [-@fig:011]).

![Попытка входа в Bitrix](image/11.png){#fig:011 width=70%}

Для устранения вектора для локального повышения привелегий (LPE) удалили SUID-бит у файла /var/www/html/apache_restart с
помощью команды chmod –s /var/www/html/apache_restart. (рис. [-@fig:012]).

![Устранение LPE](image/12.png){#fig:012 width=70%}

Для закрытия уязвимости добавиkb изменения в файл /var/www/html/bitrix/tools/vote/uf.php, перед require_once и между знаков вопроса вставили код:(рис. [-@fig:013]).

![Редактирование uf.php](image/13.png){#fig:013 width=70%}

Создали файл .htaccess в директории /var/www/html/bitrix/tools/vote, задающий правила работы веб-сервера для конкретного каталога и подкаталогов. Для закрытия уязвимости в данном файле можно прописали команду deny from all(рис. [-@fig:014]).

![Создание .htaccess](image/14.png){#fig:014 width=70%}

С помощью утилиты ss и команды kill закрыли meterpreter сессии. (рис. [-@fig:015]).

![Закрытие meterpreter сессий](image/15.png){#fig:015 width=70%}

В директории веб-сервера обнаружили скрипт password_recovery.php. (рис. [-@fig:016]).

![Директория веб-сервера](image/16.png){#fig:016 width=70%}

Прописали новый пароль. (рис. [-@fig:017]).

![password_recovery.php](image/17.png){#fig:017 width=70%}

Подключились к веб-серверу, в ссылке указали название данного файла. (рис. [-@fig:018]).

![Deface веб-сервера](image/18.png){#fig:018 width=70%}

Авторизовались с правами администратора (рис. [-@fig:019]).

![Авторизация](image/19.png){#fig:019 width=70%}

Открылась панель администрирования. (рис. [-@fig:020]).

![Администрирование](image/20.png){#fig:020 width=70%}

Удалили файл password_recovery.php. (рис. [-@fig:021]).

![Удаление файла](image/21.png){#fig:021 width=70%}

Доступ к панели администрирования восстановлен. Удалили все файлы в директории взломанного веб-сервера. (рис. [-@fig:022]).

![Удаление файлов](image/22.png){#fig:022 width=70%}

Файл резервной копии разархивировали в директорию /var/www/html с помощью команды tar xvzf /var/bitrix_backups/Bitrix_full_backup.tar.gz -C /var/www/html. (рис. [-@fig:023]).

![Резервная копия](image/23.png){#fig:023 width=70%}

Далее повторили действия по устранению полезной нагрузки: 
Для устранения вектора для локального повышения привелегий (LPE) удалили SUID-бит у файла /var/www/html/apache_restart с
помощью команды chmod –s /var/www/html/apache_restart. (рис. [-@fig:024]).

![Устранение LPE](image/12.png){#fig:024 width=70%}

Для закрытия уязвимости добавиkb изменения в файл /var/www/html/bitrix/tools/vote/uf.php, перед require_once и между знаков вопроса вставили код:(рис. [-@fig:025]).

![Редактирование uf.php](image/13.png){#fig:025 width=70%}

Создали файл .htaccess в директории /var/www/html/bitrix/tools/vote, задающий правила работы веб-сервера для конкретного каталога и подкаталогов. Для закрытия уязвимости в данном файле можно прописали команду deny from all(рис. [-@fig:026]).

![Создание .htaccess](image/14.png){#fig:026 width=70%}

Удалили файл /var/www/html/apache_restart.(рис. [-@fig:027]).

![Удаление файла](image/24.png){#fig:027 width=70%}

Уязвимость с ее последствием успешно устранены (рис. [-@fig:028]).

![Успех](image/25.png){#fig:028 width=70%}

## Уязвимость GitLab RCE

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. Изучили информацию об обнаруженной уязвимости.(рис. [-@fig:029]).

![Обзор уязвимости](image/29.png){#fig:029 width=70%}

В соответствии с вектором атаки в KeePass нашли GitLab.(рис. [-@fig:030]). 

![KeePass](image/30.png){#fig:030 width=70%}

Подключились к удаленному рабочему столу по адресу в соответствии с вектором атаки. Открыли веб-интерфейс GitLab и авторизовались под учетной записью администратора. (рис. [-@fig:031]). 

![GitLab](image/31.png){#fig:031 width=70%}

Перешли на страницу Admin Area. (рис. [-@fig:032]). 

![Admin Area](image/32.png){#fig:032 width=70%}

В левой панели инструментов перешли во вкладку Settings – General.(рис. [-@fig:033]). 

![Settings -> General](image/33.png){#fig:033 width=70%}

в настройках нашли пункт Sign-up restrictions и нажали кнопку Expand. (рис. [-@fig:034]). 

![Sign-up restrictions](image/34.png){#fig:034 width=70%}

Настроили конфигурацию, разрешающую регистрацию новых аккаунтов только с одобрения адмнистратора.(рис. [-@fig:035]). 

![Настройка](image/35.png){#fig:035 width=70%}

Сохранили конфигурацию. (рис. [-@fig:036]). 

![Сохранение конфигурации](image/36.png){#fig:036 width=70%}

В панели администратора перешли во вкладку Users(рис. [-@fig:037]). 

![Users](image/37.png){#fig:037 width=70%}

В строке с пользователем Script Kiddie нажали Delete user and contributions. (рис. [-@fig:038]). 

![Удаление пользователя](image/38.png){#fig:038 width=70%}

Подтвердили удаление. (рис. [-@fig:039]). 

![Подтверждение удаления](image/39.png){#fig:039 width=70%}

С помощью утилиты ss и команды kill закрыли meterpreter сессии. (рис. [-@fig:040]).

![Закрытие meterpreter сессий](image/40.png){#fig:040 width=70%}

Уязвимость с ее последствием успешно устранены (рис. [-@fig:041]).

![Успех](image/41.png){#fig:041 width=70%}

## Уязвимость WSO2 API-Manager RCE

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. (рис. [-@fig:042]).

![Журнал событий](image/42.png){#fig:042 width=70%}

Изучили информацию об обнаруженной уязвимости.(рис. [-@fig:043]).

![Обзор уязвимости](image/43.png){#fig:043 width=70%}

В соответствии с вектором атаки в KeePass нашли API-Manager.(рис. [-@fig:044]). 

![KeePass](image/44.png){#fig:044 width=70%}

Открыли файл конфигурации WSO2 API-Manager и добавили в конец запись resource.access_control.(рис. [-@fig:045] - [-@fig:046]). 

![Файл конфигурации](image/45.png){#fig:045 width=70%}

![Редактирование](image/46.png){#fig:046 width=70%}

Удалили загруженный exploit.jsp файл по пути /opt/wso2am-4.0.0/repository/deployment/server/webapps/authentic
ationendpoint. (рис. [-@fig:047]). 

![Удаление файла](image/47.png){#fig:047 width=70%}

Удалили сгенерированный файл /tmpp/payload.elf. (рис. [-@fig:048]). 

![Удаление файла](image/48.png){#fig:048 width=70%}

С помощью утилиты ss и команды kill закрыли meterpreter сессии. (рис. [-@fig:049]).

![Закрытие meterpreter сессий](image/49.png){#fig:049 width=70%}

Зашли в веб-интерфейс WSO2 API-Manager по ссылке https://10.10.2.27:9443/carbon и авторизовались под учетной записью администратора. (рис. [-@fig:050]). 

![Вход в веб-интерфейс](image/50.png){#fig:050 width=70%}

Просмотрели список пользователей. (рис. [-@fig:051]). 

![Пользователи](image/51.png){#fig:051 width=70%}

Удалили пользователя hacker.(рис. [-@fig:052]). 

![Удаление пользователя](image/52.png){#fig:052 width=70%}

Уязвимость с ее последствием успешно устранены. (рис. [-@fig:053]). 

![Успех](image/53.png){#fig:053 width=70%}

# Выводы

В результате выполнения лабораторной работы мы получили навыки обнаружения и устранение уязвимостей Bitrix vote RCE, GitLab RCE, WSO2 API-Manager RCE и их последствий.
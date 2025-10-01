---
## Front matter
title: "Лабораторная работа № 1-D"
subtitle: "Защита корпоративного мессенджера"
author: "Доберштейн А. С., Оразгелдиев Я. О., Барабанова К. А."

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

Основной целью работы является получение навыков обнаружения и устранение уязвимостей  WordPress-wpDiscuz, Proxylogon, Rocket.Chat и их последствий.

# Выполнение лабораторной работы

## Подготовка к выполнению лабораторной работы

Для начала изучили вектор атаки, адреса злоумышленника и атакуемых серверов.(рис. [-@fig:001]).

![Вектор атаки](image/1.png){#fig:001 width=70%}

## Уязвимость WordPress-wpDiscuz

Залогинились в ViPNet для обнаружения уязвимости в журнале событий.(рис. [-@fig:002]).

![Вход в ViPNet](image/2.png){#fig:002 width=70%}

В "Событиях" обнаружили событие AM Exploit Wordpress с программным кодом, предназначенным для эксплуатации уязвимости(рис. [-@fig:003]).

![Журнал событий](image/3.png){#fig:003 width=70%}

Изучили информацию по CVE-коду об обнаруженной уязвимости, изучили рекомендации по нейтрализации. (рис. [-@fig:004]).

![Обзор уязвимости](image/4.png){#fig:004 width=70%}

Для устранения уязвимости подключились к удаленному рабочему столу по адресу 10.140.2.180(рис. [-@fig:005]).

![Подключение к удаленному рабочему столу](image/5.png){#fig:005 width=70%}

Вошли под указанной учетной записью. (рис. [-@fig:006]).

![Вход](image/6.png){#fig:006 width=70%}

В соответствии с вектором атаки в KeePass нашли CMS WordPress.(рис. [-@fig:007]). 

![KeePass](image/7.png){#fig:007 width=70%}

Просмотрели сайт WordPress по указанному адресу. Здесь обнаружили последствие - Deface - изменение внешнего вида интерфейса. (рис. [-@fig:008]).

![Deface](image/8.png){#fig:008 width=70%}

В панели администрирования перешли во вкладку с плагинами и деактивировали плагин wpDiscuz (рис. [-@fig:009]).

![Плагин wpDiscuz](image/9.png){#fig:009 width=70%}

Для того, чтобы устранить последствие Deface, необходимо откатить сайт до предыдущей резервной копии. Для этого перешли в панель администрирования и во вкладке с плагинами нашли плагин UpdraftPlus - Backup/Restore, перешли в "Settings".(рис. [-@fig:010]).

![Плагин UpdraftPlus - Backup/Restore](image/10.png){#fig:010 width=70%}

Выбрали последнюю резервную копию и нажали "Restore". (рис. [-@fig:011]).

![Restore](image/11.png){#fig:011 width=70%}

Поставили флажки у компонентов "Themes" и "Uploads". (рис. [-@fig:012]).

![Параметры восстановления](image/12.png){#fig:012 width=70%}

Во всплывшем окне с ошибкой нажали "Удалить старые директории"(рис. [-@fig:013]).

![Удаление старых директорий](image/13.png){#fig:013 width=70%}

Когда директории удалились, нажали "Return to UpdraftPlus configuration"(рис. [-@fig:014]).

![Восстановление версии](image/14.png){#fig:014 width=70%}

Обновили страницу сайта. Убедились, что последствие Deface успешно устранено. (рис. [-@fig:015]).

![Устранено последствие](image/15.png){#fig:015 width=70%}

Перешли в Putty web-portal, чтобы проверить сокеты на наличие подозрительных процессов с помощью утилиты ss. (рис. [-@fig:016]).

![Проверка процесов](image/16.png){#fig:016 width=70%}

Уничтожили вредоносные соединения с помощью команды kill {pid}. Убедились в их отсутствии. (рис. [-@fig:017]).

![Уничтожение вредоносных процессов](image/17.png){#fig:017 width=70%}

Первая уязвимость с ее последствием успешно устранены (рис. [-@fig:018]).

![Успех](image/18.png){#fig:018 width=70%}

## Уязвимость Proxylogon

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. (рис. [-@fig:019]).

![Журнал событий](image/19.png){#fig:019 width=70%}

Изучили информацию об обнаруженной уязвимости.(рис. [-@fig:020]).

![Обзор уязвимости](image/20.png){#fig:020 width=70%}

В соответствии с вектором атаки в KeePass нашли MS Exchange.(рис. [-@fig:021]). 

![KeePass](image/21.png){#fig:021 width=70%}

Подключились к удаленному рабочему столу по адресу в соответствии с вектором атаки. Открыли Internet Information Services Manager. (рис. [-@fig:022]). 

![inetmgr](image/22.png){#fig:022 width=70%}

Перешли в /MAIL/Sites/Default Web Site/ecp(рис. [-@fig:023]). 

![inetmgr](image/23.png){#fig:023 width=70%}

Перешли в IP Address and Domain Restrictions, в "Actions" выбрали "Edit Feature Settings", в открывшемся окне в параметре "Access for unspecified clients" выбрали "Deny".(рис. [-@fig:024]). 

![inetmgr](image/24.png){#fig:024 width=70%}

Далее открыли терминал, чтобы обнаружить вредоносные процессы с помощью утилиты netstat. (рис. [-@fig:025]-[-@fig:026]). 

![Вредоносные процессы](image/25.png){#fig:025 width=70%}

![Вредоносные процессы](image/26.png){#fig:026 width=70%}

Остановили эти процессы и проверили их отсутствие.(рис. [-@fig:027]). 

![Вредоносные процессы](image/27.png){#fig:027 width=70%}

Далее в директории /C:/Program Files/Microsoft/Exchange Server/V15/FrontEnd/HttpProxy/owa/auth удалили файл AM_Backdoor.aspx(рис. [-@fig:028]). 

![Удаление файла .aspx](image/28.png){#fig:028 width=70%}

Уязвимость Proxylogon и ее последствие China Chopper успешно устранены.(рис. [-@fig:029]). 

![Успех](image/29.png){#fig:029 width=70%}

## Уязвимость Rocket.Chat

Вернулись в ViPNet для обнаружения подозрительной активности в журнале событий. (рис. [-@fig:030]).

![Журнал событий](image/30.png){#fig:030 width=70%}

Изучили информацию об обнаруженной уязвимости.(рис. [-@fig:031]).

![Обзор уязвимости](image/31.png){#fig:031 width=70%}

В соответствии с вектором атаки в KeePass нашли RocketChat.(рис. [-@fig:032]). 

![KeePass](image/32.png){#fig:032 width=70%}

Открыли веб-версию Rocket.Chat и нажали на сброс пароля для указанной учетной записи.(рис. [-@fig:033]). 

![Сброс пароля](image/33.png){#fig:033 width=70%}

На почту администратора Rocket.Chat было направлено email-письмо с инструкциями по сбросу пароля.(рис. [-@fig:034]). 

![Сообщение об отправке email](image/34.png){#fig:034 width=70%}

В консоли от администратор просмотрели это письмо. Скопировали ссылку со сгенерированным токеном для сброса пароля. (рис. [-@fig:035]). 

![Инструкция по сбросу пароля](image/35.png){#fig:035 width=70%}

Перешли по скопированному адресу в браузере(рис. [-@fig:036]). 

![Сброс пароля](image/36.png){#fig:036 width=70%}

Задали новый пароль для пользователя(рис. [-@fig:037]). 

![Сброс пароля](image/37.png){#fig:037 width=70%}

Просмотрели /home/user/backup_codes для прохождения двухфакторной аутентификации при сбросе пароля. (рис. [-@fig:038]). 

![Backup_codes](image/38.png){#fig:038 width=70%}

Ввели один из них в соответствующее поле ввода.(рис. [-@fig:039]). 

![Сброс пароля](image/39.png){#fig:039 width=70%}

Залогинились с измененным паролем. (рис. [-@fig:040]). 

![Вход в учетную запись](image/40.png){#fig:040 width=70%}

В панели администриования перешли в "Права доступа", и для роли "User" поставили флажок "User must use Two factor Authentication".(рис. [-@fig:041]). 

![Права доступа](image/41.png){#fig:041 width=70%}

Перешли во вкладку "Учетные записи" и настроили подтверждение адреса электронной почты при регистрации для роли "User" и автоматическую настройку двухфакторной аутентификации по электронной почте для новых пользователей.(рис. [-@fig:042]-[-@fig:043]). 

![Настройки двухфакторной аутентификации](image/42.png){#fig:042 width=70%}

![Настройки регистрации](image/43.png){#fig:043 width=70%}

В терминале администратора Rocket.Chat-server отредактировали файл mongod.conf, расскомментировав параметр "security:" и прописав отключение выполнения JavaScript на стороне сервера базы данных "javascriptEnabled: False".(рис. [-@fig:044]). 

![Редактирование mongod.conf](image/44.png){#fig:044 width=70%}

Перезапустили службу mongod.service(рис. [-@fig:045]). 

![Restart mongod.service](image/45.png){#fig:045 width=70%}

С помощью утилиты ss обнаружили и остановили вредоносные процессы.(рис. [-@fig:046]). 

![Уничтожение вредоносных соединений](image/46.png){#fig:046 width=70%}

Уязвимость RocketChat RCE и ее последствие meterpreter успешно устранены.(рис. [-@fig:047]). 

![Успех](image/47.png){#fig:047 width=70%}

## Завершение выполнения лабораторной работы

Заполнили карточки инцидентов для уязвимостей и их последствий.(рис. [-@fig:048]). 

![Карточки инцидентов](image/48.png){#fig:048 width=70%}

# Выводы

В результате выполнения лабораторной работы мы получили навыки обнаружения и устранение уязвимостей  WordPress-wpDiscuz, Proxylogon, Rocket.Chat и их последствий.

# Список литературы{.unnumbered}

::: {#refs}
:::

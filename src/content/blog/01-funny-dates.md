---
title: Про работу с датами
postSlug: 01-funny-dates
date: 2021-08-08T01:00:00
tags:
  - дата
  - js
---
Недавно фиксил баг с датой. Поймал себя на мысли, что часто работа с датой оказывается довольно "интересной". Появляются специфичные проблемы и требования. Некоторая время мысли про это крутились у меня в голове. Потом я написал тред про это и вот теперь он превратился в статью. Не претендую на полноту и академичность, скорее это рефлексия над моим опытом. Поехали!

Сначала про тот самый баг. Была утилитарная ф-я которая считала что-то вроде номера недели в месяце. "Вроде" потому, что оказалось никто не знает что это такое и как должно работать. И наверно это так и осталось бы тайной, если бы однажды в интерфейсе не появились отрицательные числа)

Фикс выглядел довольно тривиальным. По крайней мере так казалось некоторое время. Скоро стало понятно, что в алгоритме имеется приличное количество эдж кейсов. Было принято решение сначала написать все тест кейсы с ожидаемым поведением и потом написать код. Классический TDD. И это сработало.

> Action point #1: при прямых манипуляциях с временем имеет смысл писать юнит тесты. Это документирует код гораздо лучше комментариев и позволяет сразу подсветить все эдж кейсы

> Action point #2: при проверке алгоритма лучше проверить кейсы на несколько месяцев/лет в будущее/прошлое. Даже если кажется очевидным, что все будет работать. Ну и офк не стоит вводить "магических дат". Все очень удивятся, когда в один "магический день" фича сломается на проде

Следующая интересная штука, с которой довелось столкнуться- работа с дедлайнами. Если фича подразумевает что-то вроде таймера обратного отсчета стоит быть внимательным тк появляется много кейсов, когда что-то можно продолбать.

Во первых локальное время пользователя - время и часовой пояс. Что-то из этого может быть неверным, а может и оба. От неверного времени спасает получение времени с сервера, от неверного часового пояса не спасает ничего.

> Action point #3: фичи где присутствует манипуляции с датой нужно тестировать с неправильным временем/часовым поясом. Артефакты тестирования пригодятся поддержке, чтобы сказать пользователю почему у него что-то не так

Гораздо сложнее все, если вам нужен честный обратный отсчет. Проблема в том, что таймеры в браузере могут съезжать из-за экономии энергии и сна. Чтобы пофиксить нужно периодически трекать рассинхрон времени таймера и периодически ходить на сервер за актуальным временем.

У нас в проекте был написан хок, который прокидывал время в компонент м обновлял его с некоторым интервалом. Он использовал синглтон таймер и периодическую синхронизацию с сервером.

> Action point #4: точный таймер - технически сложная штука, которую может не получиться написать с первого раза. Советов кроме - внимательно продумать юз кейсы и написать побольше юнитов, особо в голову не приходит

Еще один интересный момент случился когда мы делали интерфейс выбора даты старта и дедлайна контрольной работы. Фича была подробно спроектированная и выглядела довольно логично. Разработка прошла с некоторым количеством вопросов и фичу отдали в тестирование. Тут выяснилось, что для пользователя все не очень прозрачно. Например фразы типа "следующий вторник" и "эта пятница" оказались очень непонятными тк понимание, что это за день зависело от того, какой день сейчас.

Нужно было сделать понятнее пользователю. Это с одной стороны добавило эдж кейсов в фичу, с другой стороны заставило отказаться от части запланированного функционала. После этого на одной из досок офиса появилась надпись "время относительно")

> Action point #5: осторожнее с относительными датами. Может оказаться, что пользователь понял совсем не то, что вы имели ввиду. Особенно если еще есть валидация

Кстати о валидации. Тут никаких странных моментов не было кроме того, что приходилось держать в голове мысль, что валидация - относительная штука. Сейчас все ок, а если выдать через минуту,  ученику уже не хватит времени решить. Пользователь выбирает "доступна сейчас", а на самом деле "сейчас" должно наступить, когда он отправит форму м т.п. И, кажется, была еще парочка интересных моментов, про которые я не вспомню.

> Action point #6: тут можно сказать, что вынос на бэкенд всех предпоставленных дат типа "через 5 мин" или "следующий понедельник" решает часть проблем валидации

Ну и немного про инструменты и библиотеки. Не будет лишним сказать про то, что не стоит использовать moment. Кроме проблем с размером и производительностью он уже перестал поддерживаться. Лично мне нравится date-fns. Почему 1) работа с нативной датой 2) формат библиотеки - набор маленьких ф-й которые легко композятся в ФП стиле 3) вытекает из второго пункта - модульность, забираешь в проект только то, что тебе нужно.

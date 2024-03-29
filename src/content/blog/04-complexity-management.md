---
title: Архитектура и управление сложностью
postSlug: 04-complexity-management
date: 2022-12-13
tags:
  - фронтенд
  - архитектура
---
Решил написать об ещё одной стороне архитектуры. Это знание оказалось очень полезным для меня и позволило оценивать качество технических решений и архитектуры более объективно. Заодно появился повод освежить знания при написании поста, тк изучал все это давно.
Исходные мысли не мои. Главным образом из книг «Совершенный код» и «Чистая архитектура». Но моя интерпретация. Поехали.

### Что такое архитектура?
Вроде понятная штука, но дать четкое определение сходу сложно. «Чистая архитектура» даёт определение архитектуры как формы системы. Это объединение верхнеуровневой структуры и низкоуровневой реализации.
Википедия говорит
> Архитектура программного обеспечения — совокупность важнейших решений об организации программной системы.
Получается архитектура это смысловая структура проекта. На большую часть значимых вопросов вида “как мы в этой системе делаем X?” ответ даёт архитектура.

### Зачем нужна архитектура?
Ещё один хороший вопрос. Зачем нам принимать кучу решений и определять, как будет организована система.

Краткий ответ - если явно не принимать решения, то все будет «как то».
Для более полного ответа нужно разобраться со стоимостью/скоростью/сложностью разработки. Эти понятия в первом приближении одинаковы тк обозначают количество работы, нужное для изменения поведения системы.
«Чистая архитектура» утверждает
> Цель архитектуры программного обеспечения - уменьшить человеческие трудозатраты на создание и сопровождение системы.
Получается главная характеристика архитектуры — сложность внесения изменений.

Кстати, про изменения было в предыдущем посте [Архитектура и разнообразие вариантов](https://vanilla-wave.dev/posts/02-variety-of-options/)

Если опустить уровень мастерства команды, то получится что трудозатраты прямо пропорциональны сложности системы. Чем сложнее система, тем больше трудозатраты на внесение изменений.
Получается логично — если проект фиг пойми как устроен внутри, то внесение даже простых изменения окажется сложной задачей. Если выбрана неудачная архитектура, результат окажется таким себе.

Итого - архитектура нужна, уменьшить усилия необходимые для изменения системы. Другими словами, чтобы справиться со сложностью системы.
И тут важно понять, откуда берётся сложность системы и что с ней делать.

### Существенная и не существенная сложность
Сложность при создании ПО может быть разной. В Чистом коде выделяют существенную и несущественную.

К несущественной относят например неудобный синтаксис языков программирования, несовершенное ПО для написания и отладки, ограничения оборудования на котором ведётся разработки. Вряд ли можно назвать простой и удобной разработку на компьютере 30 летней давности. Или написание кода на перфокарте с отладкой в уме. Но все это решаемые проблемы. Разработчик с опытом сможет разобраться и с языком и с оборудованием. Поэтому эта сложность называется несущественной. Большинство проблем в этой области уже решено.

### Сложность реального мира
Существенной же называется сложность исходной задачи, сложность реального мира, который нужно транслировать код. А реальный мир очень сложный. Гораздо сложнее, чем мы обычно думаем. Вот один пример.

Представьте приложение ToDo листа. Да, то самое, которое пишется первым при изучении фремймворка. Кажется, довольно простая штука. Написать такое быстро и несложно. Теперь представьте, что вы хотите его использовать сами. Наверняка чего-то будет не хватать. Можно прикинуть количество недостающих фичей. Можно спросить ещё 5 своих друзей и дополнить.
И вот простое приложение превращается в довольно нетривиального монстра. А это всего лишь тудушка. Надо ли говорить, что в системах большего размера эффект будет выражен ещё сильнее.

Ну и ещё пример ситуации, с которой многие наверняка сталкивались. Вы пишите код для какой-то фичи. Вы придумали очень красивую и элегантную реализацию и она хорошо решает проблему. Все идеально, вы радуетесь. Но тут всплывает парочка корнер кейсов или забытых требований. И вот решение становится куда более громоздком и менее элегантным.

Эта и есть та сама сложность реального мира, которую мы часто недооцениваем.
А ещё в реальном мире не всегда можно получить весь набор сведений по предметной области. Это называют «грязной проблемой». В таких случаях задачу нужно решить один раз, чтобы собрать информацию и ограничениях и уже вторым заходом решить проблему. Самый известный кейс — Такомский мост. Когда его строили не знали, что порывы ветра будут критичными. В итоге мост рухнул, но появился «полный список требований».

### Важность управления сложностью
Итак, реальный мир сложный. И это сильно влияет на создание программ.

Качество архитектуры влияет на способность вносить изменения в код. Этот эффект хорошо проявляется на отрезке времени. Если с течением временем и ростом кодовой базы сохраняется около константная сложность внесения изменений, архитектура хорошая. Если со временем растёт сложность внесения изменений, то архитектура плохая.

Мы, как разработчики, должны уметь управлять сложностью разработки. Представьте продукт, код которого нельзя изменить. Нельзя - наверно, слишком громкое слово и такие ситуации встречаются редко. Но вот, когда количество усилий для внедрения фичи настолько огромное, что проще ее не делать - вполне реальная история. Наверняка в голову придёт пара случаев, когда от изменений в какой-то части проекта отказывались, тк это было очень сложно. В любом случае такого хочется избегать.

Управление сложностью проекта настолько важно, что в совершенном коде его называют Главным Техническим Императивом Разработки ПО.

### Умственное жонглирование
Для полноты картины осталось связать теорию и практику. Важность архитектуры и управления сложностью, с собственно работой с кодом.

Мы имеем сложную проблему реального мира. И имеем программу, которая реализует логику из предметной области реального мира для решения этой проблемы. Мы хотим внести изменения в программу.
Для этого нам нужно выгрузить в голову какое-то количество контекста и понять как работает программа сейчас. Только очень ограниченное количество задач можно решить правкой одного метода или функции. Как правило, нужно посмотреть больше кода, чтобы понять, как именно правка повлияет на программу.
Для решения задачи нужно удержать в памяти какое-то количество блоков кода. Можно представить это как набор шариков, которые нужно поднять одновременно, чтобы решить проблему. (Аналогия из Совершенного кода)

Но человек может удержать в голове очень ограниченное количество сущностей - шариков. Обычно считают, что около 5-7.

И вот она проблема. Вы «не можете» решить задачу, если одновременно нужно поднять слишком много шариков. Можно поднимать по очереди разные комбинации шариков, чтобы решить проблему по частям. Но проблема слишком большая и придётся много раз поднять разные комбинации шариков, чтобы ее решить. В любом случае это сделает решение проблемы более сложным.

В качестве примера можно вспомнить какую нибудь фичу, которую приходить очень долго делать не потому, что она большая, а потому что в коде творится какой-то ад.

Сложно привести какому-то абстрактный пример, но я сталкивался с двумя симптомами правки кода с архитектурой. Бесконечное переключение, между разными файлами и бесконечный скролинг одного большого файла. И то и другое происходит из-за того, что логики слишком много для разовой загрузки в голову и приходится долго бродить по коду, чтобы понять, как он работает.

### Роль архитектуры в управлении сложностью
Мы имеем проблему реального мира и ограничения мозга, которое не позволяет работать с множеством объектов одновременно. Это накладывает ограничения на то, как следует писать код, чтобы его можно было легко править. И в этом помогает хорошая архитектура.

При описании предметной области в коде мы используем абстракции. Они позволяют выделить важные свойства и опустить несущественное.
Нужно структурировать проект так, чтобы для решения проблемы не приходилось поднимать слишком много шариков. Нужно, чтобы архитектура позволяла решать проблему в конкретном месте и при этом не думать об остальных частях проекта.

К сожалению сказать проще чем сделать. Огромное количество информации по архитектуре и проектированию посвящено тому как этого достичь. И сейчас точно я не буду в это углубляться. Но можно сделать парочку общих выводов, которые могут помочь на практике.

### Выводы
Здесь кратко сделаю выводы по написанному выше
- Управление сложностью - очень важная часть разработки, о которой не стоит забывать
- Задача архитектуры — помогать структурировать код так, чтобы внесение изменений было легким и не дорожало со временем
- Мозг не может держать в уме слишком много «шариков». Это не дает эффективно работать со сложной логикой в плохой архитектуре.
- Качество решения можно прикинуть по количеству, «шариков», которые нужно поднять в голове для понимания логики
  
Отдельно отмечу, что под «структурировать код так, чтобы внесение изменений было легким» не является простой задачей. И есть множество книг про архитектуру, паттерны и приемы, которые позволяют это делать.

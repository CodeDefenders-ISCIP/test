# MissInfoAggregator

Доброго дня ми команда **CodeDefenders**! Раді представити вам наше рішення **MissInfoAggregator**.

1. [Які проблеми ми вирішуємо](#Які-проблеми-ми-вирішуємо)
2. [Що ми зробили](#Що-ми-зробили)
3. [Як це працює](#Як-це-працює)
	- [Модуль зберігання даних](#Модуль-зберігання-даних)
	- [Модуль збору та обробки даних](#Модуль-збору-та-обробки-даних)
	- [Модуль аналітичної системи](#Модуль-аналітичної-системи)
	- [Формування списку джерел](#Формування-списку-джерел)
	- [Навчання моделі FastText](#Навчання-моделі-FastText)
	- [Оцінка ознак дезінформативності](#Оцінка-ознак-дезінформативності)
	- [Інформаційний портрет джерела](#Інформаційний-портрет-джерела)
	- [Кластеризація джерел](#Кластеризація-джерел)
4. [Як це виглядає](#Як-це-виглядає)
	-	[Відео](#Відео)
	-	[Скріншоти](#Скріншоти)
5. [Як це оживити](#Як-це-оживити)
	-	[Вимоги](#Вимоги)
	-	[Інструкція](#Інструкція)
7. [Що далі](#Що-далі)

## Які проблеми ми вирішуємо 
Існує багато маніпулятивних та дезінформаційних джерел. Впоратися з їхнім впливом на інформаційне середовище та пересічного користувача досить складно. В умовах російського вторгнення на територію нашої держави, підтриманого інформаційною війною, такий вплив може бути небезпечним. Через великі обсяги інформаційних потоків звичайній людині складно обробляти цю інформацію і знаходити те, що є необхідним та правильним для неї.

Таким чином, ми вирішуємо такі проблеми:
-   відсутність єдиного підходу та алгоритмів для класифікації джерел за їхньою причетністю до маніпулятивних дій росії;
-   відсутність єдиного реєстру інформаційних джерел із зазначенням цієї класифікації;
-   відсутність автоматизованих інструментів і програмних рішень для забезпечення класифікації.

## Що ми зробили

Розроблено інструмент **MissInfoAggregator** для аналітиків, щодо виявлення, аналізу та протидії дезінформації, що поширюється у відкритих джерелах. Його суть полягає в автоматизованій агрегації новин з різноманітних відкритих джерел інформації (новинні сайти, Telegram, X, Odnoklassniki, Vkontakte, Facebook, та ін.), та їх подальшу обробку з допомогою нейронних мереж та штучного інтелекту.  

![Функціонал аналітиків](https://drive.google.com/uc?id=1w0rGb9UJUmbtCBm9wqWDEacjZ_RRdTeV)

Функціонал для аналітиків відображено на діаграмі варіантів використання (рис. 1):
- Розширений пошук новин
	- пошук за ключовими словами
	- пошук за датами
	- пошук за джерелами новин
- Аналітика новин за пошуком
	- побудова інфографіки за динамікою публікацій, ключовими джерелами/авторами
	- побудова інфографіки за джерелами та типами 
	- побудова семантичних мап
	- можливість надання оцінки новині
	- можливість надання оцінки джерелу новини або її автору
	- можливість класифікації лдерела новини
	- експорт аналітичних даних

Все це можливо через постійний моніторинг новин з визначених попередньо джерел інформації та їх обробки:
- Аналіз отриманих новин з використанням АІ
	- виокремлення сутностей (осіб, організацій) з новини з використанням генеративного штучного інтелекту
	- аналіз попередньо натренованою нейронною мережею на наявність ознак дизінформації
- Аналіз новини на ознаки дезінформації
	- перегляд рейтингу джерела
	- перегляд рейтингу автора

![Функціонал адміністраторів](https://drive.google.com/uc?id=1wx5Cda2q1a20QciEWUeSUfIHl1u2htjK)

Адміністраторові системи надається можливість:
- Додавання нових коннекторів для збору даних з нових джерел
- Додавання нових каналів для збору новин з соціальних мереж
- Поповнення списку “недобросовісних джерел та авторів”
- Розробка та впровадження нових агрегацій та інфорграфік.

## Як це працює
![Cхема функціонування застосунку](https://drive.google.com/uc?id=1P2pp7YafvJ8GXRToAaXtoPEHrYxII8_3)

Весь застосунок складається з трьох основних модулів:
- Модуля збору та обробки даних
- Модуля аналітичної системи
- Модуль зберігання даних

### Модуль збору та обробки даних
**Модуль збору та обробки даних** призначений для збору даних, їх обробки та зберігання обробленої інформації до бази даних.
Він складається з 3 компонент:
- Модуль парсингу даних (Python, Beautiful Soup, Selenium)
- Модуль початкової обробки новини (Python, ChatGPT_API)
- Модуль натренованої NLP моделі (Python, FastText)

Послідовність роботи в межах цієї підсистеми наступна:
1) Модуль парсингу перідочно запускає скрипти, що парсять інформацію з новинних сайтів та різноманітних соцівльних мереж (Telegram, X, Odnoklassniki, VK тощо).  
2) Кожна з новин передається на модуль початковох обробки новин. За допомогою генеративного штучного інтелекту ChatGPT з використанням підготовлених заздалегідь промптів кожна з коної новини виокремлюються:
	- ключові слова;
	- персони;
	- локації;
	- організації;
	- лайливі слова;
	- основні поняття з їх ключовими словами.
3) Відправка новини на обробку в ChatGPT.
4) Повернення опрацьованого результату.
5) Відправка новини в натреновану модель FastText, що дозволяє оцінити її на наявність ознак дезінформації.
6) Отримання результату оцінки, його опрацювання з врахуванням попередніх результатів.
7) Відправка опрацованої новини на зберігання до модуля зберігання даних.

### Модуль зберігання даних
**Модуль зберігання даних**  це ніщо інше як нереляціна база даних Elasticsearch. ЇЇ перевага полягає в можливості зберігання документів у вигляді JSON, а також можливості щодо повнотекстового пошуку із багатьма додатковими функціями.
В Elasticsearch є ряд індексів призначених для зберігання даних, а також службових індексів.
Структура документа, що зберігається в Elasticsearch, і відповідає опрацьованій новині наступна:
``` json
{
    "user": "ANNA NEWS",
    "_title": "Пашинян заявил, что возвращение Армении в ОДКБ \"становится все более трудным, ес...",
    "_content": "Пашинян заявил, что возвращение Армении в ОДКБ \"становится все более трудным, если не сказать невозможным\".",
    "url": "https://t.me/anna_news/73799",
    "time": "2024-12-04 16:36",
    "persons": [
        "Пашинян"
    ],
    "organizations": [
        "ОДКБ"
    ],
    "locations": [
        "Арменія"
    ],
    "keywords": [
        "повернення",
        "трудним",
        "неможливим"
    ],
    "pejoratives": [],
    "connections": "Заява Пашиняна про повернення Арменії в ОДКБ; Пашинян\nЗаява Пашиняна про повернення Арменії в ОДКБ; Арменія\nЗаява Пашиняна про повернення Арменії в ОДКБ; ОДКБ\nЗаява Пашиняна про повернення Арменії в ОДКБ; трудності\nЗаява Пашиняна про повернення Арменії в ОДКБ; неможливість\nЗаява Пашиняна про повернення Арменії в ОДКБ; політична ситуація\nЗаява Пашиняна про повернення Арменії в ОДКБ; міжнародні відносини\nЗаява Пашиняна про повернення Арменії в ОДКБ; регіональна безпека\nЗаява Пашиняна про повернення Арменії в ОДКБ; конфлікти\nЗаява Пашиняна про повернення Арменії в ОДКБ; дипломатія",
    "channel_reputation": "negative",
    "model_mark": 0.56,
    "common_mark": 0.66    
}
```

В базовому випадку кожна новина містить:
- заголовок (_title)
- вміст (_title)
- автора/джерело (_title)
- посилання (url) 
- відмітку часу (time). 

Крім цього вона доповнюється такою інформацією після обробки:
- ключові слова (keywords)
- персони, що згадуються в новині (persons)
- локації, що згадуються в новині (locations)
- організації, що згадуються в новині (organizations)
- лайливий/токсичний контент (pejoratives)
- систему понять (connections)
- репутацію джерела новини (channel_reputation)
- оцінку надану натренованою моделлю (model_mark)
- загальну оцінку надану новині (common_mark)

 Крім цього є індекс Channels призначений для зберігання інформації про джерела новин. Він має наступну структуру документа:
 ```json
 {
    "type": "telegram"
    "source_name": "Главное в Мелитополе",
    "url": "https://t.me/melitopol_ru",
    "reputation": "negative"
  }
```

###  Модуль аналітичної системи
**Модуль аналітичної системи** являє собою застосунок зяким напряму може працювати аналітик. 
Він складається з фронтенду (HTML, CSS, JS) розгорнутого в контейнері Docker, та REST API бекенду (Python, Flask).

Цей модуль дозволяє аналітику працювати з новинами, та каналами, зокрема:
- здійснювати перегляд останніх новин збережених в системі та їх характеристик (репутацію джерела, наявність токсичного контенту, оцінка ознак дезінформативності новини);
- здійснювати пошук за тематикою, ключовими словами в певному проміжку часу, та з можливістю фільтрування за типом джерела;
- перехід за посиланнями до новини або її джерела;
- перегляд інформаційного портрету новин джерела за останній тиждень у вигляді симантичної моделі;
- кластеризація джерел за їх інформаційним портретом;
- перегляд інфографіки за останій тиждень.

Адміністратор може налаштовувати систему шляхом безпосередньої роботи з базою даних (для редагування списку джерел, що відслідковуються та їх рейтингу), або із системою для додавання нових парсерів, чи інфографіки для аналітиків.

### Формування списку джерел
На даному етапі **MissInfoAggregator** працює з трьома джерелами новин, зокрема:
- Telegram
- Twitter / X
- Odnoklassniki

Підключення інших типів джерел новин є відносно легким, для цього достатньо написати нові парсери за зразком існуючих.

Для збору даних було обрано близько 500 інформаційних джерел. Серед яких є канали з гарною, нейтральною, та поганою репутацією. Репутація джерел визначається за тим чи були вони помічені за публікацією фейкових або маніпулятивних новин. При визначенні їх рейтингу команда спиралась на офіційно опубліковані рейтинги надані наприклад РНБО ([Список безпечних каналів отримання інформації](https://cpd.gov.ua/announcement/spysok-bezpechnyh-kanaliv-otrymannya-informacziyi/), [Список інструментів поширення ворожої дезінформації](https://cpd.gov.ua/reports/spysok-instrumentiv-poshyrennya-vorozhoyi-dezinformacziyi/), [Список каналів поширення ворожої пропаганди в соцмережі Х](https://cpd.gov.ua/reports/spysok-kanaliv-poshyrennya-vorozhoyi-propagandy-v-soczmerezhi-h/)).

### Навчання моделі FastText
Для навчання моделі на основі FastText було зібрано корпус документів загальним розміром близько 3000. До них увійшли як новини, які явно розцінюються як дезінформація чи фейк, так і нейтральні новини. Для збору фейкових новин було обрано канали агрегації таких новин, а також новини з каналів з негативною репутацією (описано вище).

Для більш точної роботи необхідно постійно донавчати модель, та, зокрема, збільшити корпус новин для навчання, та можливо покращити його.

### Оцінка ознак дезінформативності
Дана оцінка є збірною і обраховується в межах 0,0 .. 1,0.
Для інтерпретації оцінок вибрано три числові проміжки:
- 0 .. 0,29 : не дезінформація
- 0.3 .. 0.69 : потенційно дезінформація
- 0.7 .. 1.0 : не дезінформація

Основу даної оцінки складає оцінка (*model_mark*) отримана від моделі FastText.
Додатково на цю оцінку можуть впливати негативний рейтинг джерела (*channel_reputation*) та наявність токсичного тексту в новині (*pejoratives*), що додає по 0,1 кожен. 
Таким чином формується загальна оцінка новини (*common_mark*).

### Інформаційний портрет джерела
Інформаційний портрет джерела доступний за попередній тиждень і являє собою семантичну мапу понять, з'єднаних зв'язками. Він дозволяє проаналізувати повістку тижня, що підіймається джерелом, а також виявити приховані зв'язки між поняттями, для їх подальшого дослідження.
Приклад:
![Приклад інформаційного портрету джерела](https://drive.google.com/uc?id=1-UErm-3vdyFOKKRR221PnayghNbntxLP)

### Кластеризація джерел
Кластеризація джерел відбувається за зв'язками між їх ключовими словами та поняттями. Всі джерела розбиваються на два кластери. Проаналізувавши ці кластери можна виокремити їх проукраїнську чи проросійську спрямованість (за каналами, що явно відносяться до тієї чи іншої категорії).
Що дає така кластеризація? Вона дозволяє класифікувати джерела, які були визначені "нейтральними" до цього, за тим в який вони кластер потрапляють. І після більш детального аналізу відносити їх до тієї чи  іншої категорії.
Приклад:
![Приклад кластеризації джерел](https://drive.google.com/uc?id=1HmKvp4_LXHTLtq4JNgqT98043nBJpcf2)

## Як це виглядає
### Відео
Детальний огляд роботи системи можна переглянути в ролику на YouTube за [посиланням ](https://youtu.be/OSnqAyBP9eQ)

### Скріншоти
![Основне вікно](https://drive.google.com/uc?id=11bEHzq3gEgTUrVkCWpzHLlXG5YJPwQMR)
![Основне вікно / перегляд новин](https://drive.google.com/uc?id=1G795vmgXi2tmsccP0G2n0QlFQvARJGZE)
![Вікно статистики](https://drive.google.com/uc?id=11I4udP3NLtNPSkg-nAXdRI98jcQjjA99)

## Як це оживити
### Вимоги
вимоги до АЗ ПЗ
### Інструкція
інструкція розгортання

## Що далі
Ми хочемо наголосити, що наше рішення має великий потенціал. Це лише частина того, що нам вдалося реалізувати у відведений час. Рішення може бути впроваджене як модуль більш складної системи аналізу інформаційного контексту.

У майбутньому вдосконалене рішення може використовуватися для прогнозування можливої ескалації бойових дій у війні, що допоможе уникнути жертв серед населення і бути готовими до подальших подій. Постійна взаємодія з користувачем дозволяє оновлювати та наповнювати базу даних інформаційних джерел, виділених сутностей та аналітичних звітів, які можуть бути корисними для правоохоронних органів та спеціальних державних установ.

Основним зацікавленим суб’єктом для нас є звичайний користувач, який хоче отримати інформацію про джерело. Наші розробки також можуть використовуватися державними та недержавними структурами, міжнародними організаціями, що займаються отриманням інформації, аналізом даних, розслідуванням злочинів у інформаційному середовищі, OSINT та HUMINT.

**Амбіції щодо вдосконалення:**

-   підключення парсерів для інших соціальних мереж (Facrbook, Viber, WhatsApp, тощо);
-   підключення парсерів для збору даних з новинних сайтів;
-   покращення модулю візуалізації та звітності програми;
-   розширення списку критеріїв класифікації (прополітична орієнтація, орієнтованість на особу тощо);
-   впровадження більш гнучкої системи класифікації новин, що передбачатиме взаємодію та можливість оцінки аналітиком;
-   розширення списку підтримуваних мов.

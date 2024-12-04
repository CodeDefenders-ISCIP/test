# MissInfoAggregator

Доброго дня ми команда **CodeDefenders**! Раді представити вам наше рішення **MissInfoAggregator**.

## Які проблеми ми вирішуємо?  
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

![Функціонал адміністраторів](https://drive.google.com/uc?id=1wx5Cda2q1a20QciEWUeSUfIHl1u2htjK){ width=50% }

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

**Модуля збору та обробки даних** призначений для збору даних, їх обробки та зберігання обробленої інформації до бази даних.
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

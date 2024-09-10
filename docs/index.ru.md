# О чем эта книга?

Приветствую вас и благодарю за интерес к моей книге о Zabbix. Я написал книгу [Zabbix cookbook](https://www.packtpub.com/product/zabbix-cookbook/9781784397586) и совместно с Ричардсом [Zabbix 4 Network Monitoring](https://www.packtpub.com/product/zabbix-4-network-monitoring-third-edition/9781789340266) несколько лет назад для PackPub. 

![Zabbix cookbook](images/book1.png){width=200} ![Zabbix Network Monitoring 4](images/book2.png){width=200}

Первая в своем роде поваренная книга, вероятно, устарела и будет заменена на [Zabbix 7 IT Infrastructure Monitoring Cookbook](https://www.packtpub.com/product/zabbix-7-it-infrastructure-monitoring-cookbook-third-edition/9781801078320), написанную Брайаном и Натаном, двумя людьми, с которыми мне очень нравится работать и которых я могу горячо рекомендовать. У Packt есть еще много книг о Zabbix, полный обзор которых можно найти здесь [Zabbix books at pack](https://www.packtpub.com/search?query=zabbix). Или, если вы хотите найти книги не на английском языке, на Amazon есть книги от Packt и других издательств на китайском, испанском и, возможно, некоторых других языках. [Другие книги](https://www.amazon.com/s?k=zabbix&crid=3G0JRTVTKS7YU&sprefix=zabbix+%2Caps%2C167&ref=nb_sb_noss_2)

Поскольку Zabbix - продукт с открытым исходным кодом, и зарабатывать на книгах я никогда не собирался, это заставило меня задуматься о том, как сделать все по-другому.
Как сделать новую книгу, не прибегая к услугам издательства, как я делал раньше.
Через некоторое время мне пришла в голову идея сделать книгу, которая была бы бесплатной и обновлялась бы по мере выхода новых версий.
Поскольку я большой поклонник документации в формате markdown или asciidoc, мне пришла в голову идея выложить книгу в git и использовать markdown.
Оставалась только одна проблема: как сделать эти файлы в формате markdown удобными для чтения, как в книге? После некоторых поисков хорошего решения я нашел [MkDocs](https://www.mkdocs.org). MkDocs - это библиотека Python-Markdown, которая может конвертировать все в HTML и может быть шаблонизирована. Таким образом, проблема была решена, и родилась новая книга.


## Кто я?

Меня зовут Патрик Уйттерхувен, и я работаю в бельгийской компании Open-Future. Я начал работать в этой компании в январе 2013 года, и тогда же начался мой путь в Zabbix. Они дали мне возможность накопить опыт и получить сертификат тренера Zabbix.
С этого года я официально являюсь 10-м тренером Zabbix. Если вы хотите пройти один из моих тренингов, смело регистрируйтесь на нашем сайте [www.open-future.be](https://www.open-future.be). Зачем вам проходить тренинг, если вы можете прочитать эту книгу бесплатно, думаете вы? Потому что тренинги, как и книга, объясняют вам все детали того, как все настроить и сделать, а также дают ценные советы и отзывы, которые вы никогда не получите из книги. Книги просто не могут охватить все.


## Какая ОС мне нужна?

Поскольку я работаю в основном с системами на базе RHEL, и потому убежден, что RHEL - лучший выбор для продакшена, я решил сосредоточиться на использовании одного из форков, доступных бесплатно. Zabbix поддерживается на Ubuntu, Debian, Suse, Raspberry .... и может быть скомпилирован на любой ОС, основанной на Unix, так что охватить их все практически невозможно. Тем не менее, книга имеет открытый исходный код и находится в GIT, так что не стесняйтесь вносить код для вашей любимой версии :). В этой книге я использую [Rocky Linux](https://rockylinux.org/) 9, но оно также должно работать и для большинства других дистрибутивов.

## Какая версия Zabbix используется в этой книге?

Так как мы почти достигли релиза Zabbix 7, я сосредоточусь на этой версии, поскольку это будет новая LTS. Всё изложенное также должно быть применимо к большинству других версий, но, конечно, будут небольшие изменения. В будущем, если сообщество окажет достаточную поддержку для совместного обновления этой книги, было бы здорово, если бы мы смогли создать книгу для каждой доступной версии LTS.

## Как использовать эту книгу?

В книге мы постараемся охватить все темы, не стесняйтесь сообщить мне, если что-то упущено, или сделать запрос на исправление. 
Нет необходимости начинать со страницы 1 и читать книгу до конца. Некоторые люди будут искать базовые знания, другие, возможно, захотят перейти к интересной части, поэтому я хочу, чтобы книга была полезна для всех. Поэтому я постараюсь как можно лучше объяснить в каждой теме точные шаги, необходимые для воспроизведения.
 
В книге будут моменты, когда вам нужно будет набрать код, я покажу команды, которые вам нужно будет набрать в окошке, как здесь.

```
# some command 
```

Внизу страницы будут добавлены примечания с полезной информацией.


Вот простая сноска[^1]. С некоторым дополнительным текстом после нее.

[^1]: Моя ссылка.


В случае, если есть какая-то важная информация, я буду добавлять заметки в документацию, как это можно увидеть здесь:

???+ note
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ info
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ tip
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ question
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ warning
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ bug
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

???+ example
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.



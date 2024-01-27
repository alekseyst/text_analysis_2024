## Конкордансеры. Практикум AncConc

__AntConc__ — корпусный менеджер. Это программа с пользовательским интерфейсом, которая позволяет достаточно простым образом собрать корпус из имеющихся файлов и предлагает широкий инструментарий для работы с ними. 

* [Страница программы](http://www.laurenceanthony.net/software/antconc/), где её можно скачать и посмотреть инструкции

### Материал для работы на семинаре
Анна Каренина: [plain text](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/karenina.txt), [xml](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/karenina.xhtml)

Война и Мир, т. 1: [plain text](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/TolstoyVojnaIMir1.txt)

Тихий Дон: [plain text](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/tihiyd.txt) 

СинТагРус: [tokens](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/syntagrus_tokens.txt) [lemma_POS](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/syntagrus_lemmas.txt) 

LiveCorpus: 
[tokens](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/LiveCorpus2019.txt) [lemma_POS](https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/LiveCorpus2019_lemmas.txt) 

[Все файлы вместе](https://disk.yandex.ru/d/xJ9VJT5zi70Qsg)

### Credits

В задании использованы материалы О. Н. Ляшевской.

### Знакомство с основными функциями

* Быстрый способ загрузать файлы: File — Open File(s) as Quick Corpus
* Если нужны более точные настройки: File — Open Corpus Manager
 - Во вкладке Raw File(s) можно создавать корпуса и давать им имена
 - Затем их можно найти во вкладке Corpus Database
 - Создавай корпусы из Raw File(s), можно регулировать всякие настройки
 - Чтобы слова с дефисами считались одним токеном, выберите Connector и Dash в меню Basic Settings — Show Token Definition Settings — Unicode Punctuation
 - Там же можно указать, какого поведения вы хотите от других знаков — скобок, пунктуации и т. д. (наличие галочке означает, что они присоединяются к слову)
 - Если нужно, там же (Open Corpus Manager — Raw File(s)) настройте кодировку (на этом занятии не нужно)
 - Работая с файлами, в которых приводятся леммы и части речи (лемма_POS), выберите там же настройку Indexer — simple_pos_headword_indexer — это понадобится нам позже
* Загрузив корпус с любым романом, постройте частотный список слов для него (вкладка Word List, нажмите кнопку Start). Кликнув на слово, вы сможете попасть в конкорданс, построенный для этого слова. 
* В Word List отсортируйте частотный список по алфавиту (Sort by Word внизу страницы). 
* Постройте частотный список двух-, трех- и т.д. -словных словосочетаний (вкладка Cluster/N-Grams, поставьте галочку на N-Grams, укажите, сколько слов в ngram-е вы хотите видеть, например, Min:3, Max:3, установите порог вхождений в корпусе, например, 10). Кликнув на n-грам, вы также можете попасть в его конкорданс. 
* Постройте списки коллокатов выбранного вами слова (вкладка Collocates), указав границы окна справа / слева. 

### Работа с регулярными выражениями 

* Конкордансы и частотные списки можно строить с использованием Regex в Search Term. Например, `\w+ну` найдет любое слово, содержащее -_ну_, но не частицу _ну_. Вот так можно найти все глаголы на _-ну-_.

<img src="https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/antconc_1.png" width = "800"/>

### Ключевые слова (лексические маркеры)

Чтобы определить характерные для некоторого корпуса слова, мы должны сравнить их частоты в данном корпусе с частотами в другом корпусе — reference corpus. 

* Загрузите SynTagRus в качестве reference corpus: откройте Corpus Manager, создайте в окне Raw File(s) необходимые корпуса, во вкладке Corpus Databases выберите (справа) Target Corpus и Reference Corpus
* Во вкладке Settings — Tool Sattings — Keyword установите Log-Likelyhood (4-term) в качестве статистической метрики определения keyness и длину списка в 1000 слов (Threshold). 
— Apply 

<img src="https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/antconc_3.png" width = "800"/>


* Перейдите на вкладку Keyword List 
 \> Start 
Для новых файлов AntConc начнет генерацию словника (выдаст предупреждение jump to Word List). 
В результате на вкладке Keyword List появится список ключевых слов, отсортированный по убыванию метрики Keyness (Log-Likelyhood). 

<img src="https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/antconc_4.png" width = "800"/>

* На ключевое слово можно кликнуть, чтобы получить конкорданс
* Любую часть таблицы с ключевыми словами можно выделить и скопировать в Excel, и после этого работать с ней как с обычной электронной таблицей: сортировать, искать и т. д.


### Частотные списки лемм и списки ключевых слов-леммы  

Чтобы построить частотный список лемм, ваш корпус должен быть лемматизирован (reference corpus, естественно, тоже). Мы будем использовать версии корпусов с подстановкой вместо токена метки леммы и части речи (в формате lemma_POS).

У вас может возникнуть вопрос — как такие размеченные файлы добыть? Это можно сделать различными готовыми программами, например [Mystem](https://yandex.ru/dev/mystem/doc/), или же сделать такой файл в Python.

Напоминаем:

- Работая с файлами, в которых приводятся леммы и части речи (лемма_POS), при загрузке корпуса выбирайте в Corpus Manager'е настройку Indexer — simple_pos_headword_indexer
- Во вкладке Settings — Global Settings — Tags можно выбрать, в каком виде вы хотите выбрать токены для файлов с тегами

### Самостоятельное исследование корпуса устной спонтанной речи. 

С помощью AntConc постройте частотные списки словоформ и лемм корпуса LiveCorpus. Определите лексические маркеры этого корпуса. (Для сравнения мы снова возьмем SynTagRus). 


### Дополнительные материалы  

### Voyant Tools 

Еще одно полезное онлайн-приложение, которое активно используют литературоведы и историки — [Voyant Tools](https://voyant-tools.org).

* Изучите основные возможности инструмента на примере романов Дж. Остин
\> Open > Choose a corpus > Austen's Novels 

<img src="https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/voyant-tools_1.png" width = "800"/>

* Voyant Tools умеет строить облака слов (для всего корпуса и отдельных документов)   
* показывает распределение частоты слов в документах  
* показывает свойства документов, такие как длина в словах, среднее количество слов в предложении и т. д. [пример](https://voyant-tools.org/?corpus=austen)  
* вернувшись на исходную страницу, вы можете загрузить и исследовать свой пользовательский корпус  

<img src="https://github.com/alekseyst/text_analysis_2024/blob/main/Practical_3/voyant-tools_2.png" width = "800"/>

#### Полезное  
* [Мануал](http://www.laurenceanthony.net/software/antconc/resources/help_AntConc321_english.pdf) (на английском)
* [Видео-тьюториал от автора](https://www.youtube.com/playlist?list=PLiRIDpYmiC0Ta0-Hdvc1D7hG6dmiS_TZj)
* [Тьюториал для семинара](https://drive.google.com/file/d/0B6-5pzCmb8MOblpzRXI3elFFeFU/view?usp=sharing)
* [Справка](https://voyant-tools.org/docs/#!/guide/start) по Voyant Tools 

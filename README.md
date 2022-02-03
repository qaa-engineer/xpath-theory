# XPath
Поиск элементов с помощью XPath

<article id="ember2143" class="step-show ember-view"><div class="step-dynamic-container">
<!---->
      <div id="ember2144" class="step-view step-view_material ember-view"><!----><div class="step-wrapper">
  <div class="step-inner page-fragment">
    <div id="ember2145" class="html-content rich-text-viewer ember-view" data-ready=""><span><h2>Поиск элементов с помощью XPath</h2>

<p>В работе с веб-страницами не всегда получается найти селектор, однозначно описывающий путь к нужному элементу. В такой ситуации для тестировщика лучшим решением проблемы будет пойти к фронтенд-разработчику проекта и договориться о специальном атрибуте, который будет использоваться в автотестах. Таким образом можно повысить тестируемость приложения. Увы, проекты бывают разные, и не всегда это возможно.&nbsp;И&nbsp;когда другого выхода больше нет, а автоматизировать как-то надо, можно обратиться к помощи языка запросов&nbsp;<strong>XPath</strong>.</p>


<p>На тему XPath мнения расходятся, но, как бы то ни было, это мощный и гибкий инструмент, который позволяет писать сложные запросы для поиска элементов.</p>

<p>Прежде всего, XPath (XML Path Language) это язык запросов, который использует древовидную структуру документа. Проверять XPath-запросы можно точно так же как и CSS-селекторы&nbsp;— в консоли разработчика. Откройте консоль на странице с котиками <a href="http://suninjuly.github.io/cats.html" rel="noopener noreferrer nofollow" target="_blank">http://suninjuly.github.io/cats.html</a>,&nbsp;и давайте на её примере разберемся в основах синтаксиса. Попробуйте вбить каждый из запросов-примеров в строку поиска, чтобы увидеть, что именно находит поисковый запрос.</p>

<h3>&nbsp;1. XPath запрос всегда начинается с символа / или //</h3>

<p>Символ / аналогичен символу &gt; в CSS-селекторе, а символ //&nbsp;— пробелу. Их смысл:</p>

<ul>
	<li>el1/el2&nbsp;— выбирает&nbsp;элементы el2, являющиеся прямыми потомками el1;</li>
	<li>el1//el2&nbsp;— выбирает элементы el2, являющиеся потомками el1 любой степени вложенности.</li>
</ul>

<p>Разница состоит в том, что&nbsp;в XPath,&nbsp;когда мы начинаем запрос с символа /,&nbsp; мы должны указать элемент, являющийся&nbsp;корнем нашего документа.&nbsp;Корнем всегда будет элемент с тегом <code>&lt;html&gt;</code>. Пример: <code>/html/body/header</code></p>

<p>Мы можем начинать запрос и&nbsp;с символа&nbsp;//.&nbsp;Это будет означать, что мы хотим найти всех потомков корневого элемента без указания корневого элемента. В этом случае, для поиска того же хедера, мы можем выполнить запрос&nbsp;<code>//header</code>, так как других заголовков у нас нет.</p>

<p><span style="color: #ff4363;">Важно!</span> Такой поиск может быть неоднозначным. Например, запрос <code>//div</code> вернет вам все элементы с тегом <code>&lt;div&gt;</code>. Избегайте неоднозначных ситуаций, они плохо влияют на здоровье ваших автотестов.</p>

<ul>
</ul>

<h3>2. Символ [ ]&nbsp;— это команда фильтрации</h3>

<p>Если по запросу найдено несколько элементов, то будет произведена фильтрация по правилу, указанному в скобках.</p>

<p>Правил фильтрации очень много:</p>

<ul>
	<li>по любому <strong>атрибуту</strong>, будь то id, class, title (или любой другой). Например, мы хотим найти картинку с летящим котом, для этого можно выполнить запрос <code>//img[@id='bullet']</code></li>
	<li>по <strong>порядковому номеру</strong>. Допустим, мы хотим выбрать вторую по порядку карточку с котом. Для этого найдем элемент с классом "row" и возьмем его второго потомка: <code>//div[@class="row"]/div[2]</code></li>
	<li>по <strong>полному совпадению текста.</strong> Да, XPath&nbsp;— это единственный способ найти элемент по внутреннему тексту. Если мы хотим найти блок текста с котом-Лениным, можно воспользоваться XPath селектором <code>//p[text()="Lenin cat"]</code>. Такой селектор вернет элемент, только если текст полностью совпадет. Здесь важно сказать, что не всегда поиск по тексту&nbsp;— это хорошая практика, особенно в случае мультиязычных сайтов.</li>
	<li>по <strong>частичному совпадению</strong> текста или атрибута. Для этого нужна функция <code>contains</code>. Запрос <code>//p[contains(text(), "cat")]</code> вернет нам все абзацы текста, которые содержат слово cat. Точно так же можно искать по частичному совпадению других атрибутов, это удобно, если у элемента несколько классов. Посмотрите на код навбара сайта с котами. Его можно найти селектором <code>//div[contains(@class, "navbar")]</code></li>
	<li>в фильтрации еще можно использовать булевы операции (and, or, not) и некоторые простые арифметические выражения (но вообще не стоит, наверное). Допустим, мы хотим найти картинку обязательно с data-type "animal" и именем "bullet-cat", для этого подойдет запрос:&nbsp;<code>//img[@name='bullet-cat' and @data-type='animal']</code></li>
</ul>

<h3>&nbsp;3.&nbsp;Символ *&nbsp;— команда выбора всех элементов</h3>

<ul>
	<li>Например можем найти текст в заголовке запросом <code>//div/*[@class="jumbotron-heading"]</code>. Это может быть удобно, когда мы не знаем точно тег элемента, который ищем.</li>
</ul>

<h3>4. Поиск по классу в XPath регистрозависим</h3>

<p>Также как и в случае поиска по CSS-селектором будьте внимательными к регистру при поиске по классам:&nbsp;</p>

<p><strong>//div/*[@class="Jumbotron-heading"]</strong>&nbsp;не найдет элемент на нашей странице.</p>

<p>&nbsp;</p>

<p>Что важно знать&nbsp;про XPath, чтобы пользоваться им безболезненно:</p>

<ul>
	<li>Не используйте селекторы вида <code>//div[1]/div[2]/div[3]</code> без крайней нужды: по такому селектору невозможно с первого раза понять,&nbsp;что за элемент вы ищете. А когда&nbsp;структура страницы хоть немного изменится, то ваш селектор с большой вероятностью перестанет работать;</li>
	<li>Если есть возможность использовать CSS-селекторы: сlass, id или name&nbsp;— лучше использовать их вместо поиска по XPath;</li>
	<li>Можно искать по полному или частичному совпадению текста или любого атрибута;</li>
	<li>Можно использовать булевы операции и простую арифметику;</li>
	<li>Можно удобно перемещаться по структуре документа (переходить к потомкам и к родителям);</li>
	<li>Подойдет, когда у сайта всё плохо с атрибутами и нет возможности достучаться до разработчиков;</li>
	<li>Есть мнение, что поиск по XPath в среднем медленнее, чем по css. Но достоверно это неизвестно;</li>
	<li>Не стоит использовать разные расширения для браузеров по поиску XPath: они подбирают нечитабельные и переусложненные&nbsp;селекторы. Лучше потратить немного времени и разобраться в синтаксисе самостоятельно, тем более, что он не очень сложный.</li>
</ul>

<p>В случае необходимости можно познакомиться с XPath подробнее по следующим ссылкам:</p>

<p><a href="https://www.w3schools.com/xml/xpath_syntax.asp" rel="nofollow noopener noreferrer" style="font-size: inherit; font-weight: inherit;" title="Link: https://www.w3schools.com/xml/xpath_syntax.asp" target="_blank">https://www.w3schools.com/xml/xpath_syntax.asp</a></p>

<p><a href="https://msdn.microsoft.com/ru-ru/library/ms256086(v=vs.120).aspx" rel="nofollow noopener noreferrer" title="Link: https://msdn.microsoft.com/ru-ru/library/ms256086(v=vs.120).aspx" target="_blank">https://msdn.microsoft.com/ru-ru/library/ms256086(v=vs.120).aspx</a></p>

<p><a href="https://msiter.ru/tutorials/xpath/syntax" rel="nofollow noopener noreferrer" title="Link: https://msiter.ru/tutorials/xpath/syntax" target="_blank">https://msiter.ru/tutorials/xpath/syntax</a></p>

<p><a href="https://habr.com/post/114772/" rel="nofollow noopener noreferrer" title="Link: https://habr.com/post/114772/" target="_blank">https://habr.com/post/114772/</a></p>

<p><a href="http://internetka.in.ua/xpath-start-part1/" rel="nofollow noopener noreferrer" target="_blank">http://internetka.in.ua/xpath-start-part1/</a></p>

<p><a href="http://internetka.in.ua/xpath-start-part2/" rel="nofollow noopener noreferrer" target="_blank">http://internetka.in.ua/xpath-start-part2/</a></p>

<p><a href="http://internetka.in.ua/xpath-start-part3/" rel="nofollow noopener noreferrer" target="_blank">http://internetka.in.ua/xpath-start-part3/</a></p>

<p><a href="http://internetka.in.ua/xpath-start4/" rel="nofollow noopener noreferrer" target="_blank">http://internetka.in.ua/xpath-start4/</a>﻿</p>

<p><a href="https://testerslittlehelper.wordpress.com/2016/07/10/real-xpath/" rel="noopener noreferrer nofollow" target="_blank">https://testerslittlehelper.wordpress.com/2016/07/10/real-xpath/</a></p>

<p><a href="https://drive.google.com/file/d/0B_IyyodHL4rXcU1BY1R1TzFXbmc/view" rel="noopener noreferrer nofollow" target="_blank">https://drive.google.com/file/d/0B_IyyodHL4rXcU1BY1R1TzFXbmc/view</a></p></span></div>
    </div>
</div>
</div>
</div>
</article>

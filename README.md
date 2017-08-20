# GridBugs

Вдохновленный [Flexbugs](https://github.com/philipwalton/flexbugs) этот список предназначен для того, чтобы быть курируемым списком ошибок, багов, неполных реализаций и особенностей работы CSS Grid Layout в разных браузерах. [Grid реализован в браузерах](https://gridbyexample.com/browsers) в работоспособном состоянии, однако есть некоторые проблемы - давайте документировать их все здесь.

Здесь я хотел бы сосредоточиться на вопросах, связанных со [спецификацией Grid](https://drafts.csswg.org/css-grid/), но вполне вероятно, что придется отдельно ссылаться на некоторые другие спецификации, такие как Box Alignment. Если вы считаете, что заметили проблему, и, похоже, она относится к Гридам, [опубликуйте её](https://github.com/rachelandrew/gridbugs/issues). Мы сможем подробно разобраться в деталях и убедиться, что проблемы с реализацией в браузере или ошибки спецификации попали в нужное место.

*Также, пожалуйста, поднимите вопрос, если я ошибаюсь в отношении любого из описаных багов * или у вас есть дополнительная информация или примеры для добавления. Вероятно, я мог сослаться на неправильный UA или пропустил изменение в спецификации. Помогите сделать список более точным и актуальным!

## Это не техническая поддержка CSS Grid

Присылая вопрос, пожалуйста, подробно опишите путь воспроизведения проблемы и создайте **Сокращенный Тестовый Пример,** чтобы проверить, что проблема не является следствием вашего кода. Если вы хотите больше узнать о CSS Grid Layout, посмотрите на [Гриды в Примерах](https://gridbyexample.com) где вы найдете видеоуроки, примеры и ссылки на другие полезные ресурсы. На больше вопросов о CSS Гридах я отвечу в своем [AMA](https://github.com/rachelandrew/cssgrid-ama) когда будет время. Также я собрал нужную информацию о [фоллбеках для старых браузеров](https://rachelandrew.co.uk/archives/2017/07/04/is-it-really-safe-to-start-using-css-grid-layout/).

## Баги

1. [`grid-auto-rows` и `grid-auto-columns` должен принимать список направляющих.](#1-grid-auto-rows-and-grid-auto-columns-should-accept-a-track-listing)
2. [Повторная нотация должна заново принимать список направляющих.](#2-repeat-notation-should-accept-a-track-listing)
3. [Поддержка фрагментации.](#3-fragmentation-support)
4. [Выравнивание элементов с собственными размерами в сетках с фиксированными направляющими.](#4-sizing-of-items-with-an-intrinsic-size-inside-fixed-size-tracks)
5. [Элементы с внутренним соотношением сторон должны выравниваться в начале.](#5-items-with-an-intrinsic-aspect-ratio-should-align-start)
6. [`grid-gap` параметры должны принимать значение в процентах.](#6-the-grid-gap-property-should-accept-percentage-values)
7. [Grid отступы должны вести себя как направляющие с авто-размерами?](#7-grid-gaps-should-behave-as-auto-sized-tracks)
8. [Установка максимальной высоты в процентах должна уменьшать изображение большего размера внутри направляющих сетки.](#8-setting-max-height-to-a-percentage-should-scale-down-a-larger-image-inside-a-grid-track)
9. [`min-content` и `max-content` в направляющих.](#9-min-content-and-max-content-in-track-listings)
10. [Некоторые HTML элементы не могут быть грид-контейнерами.](#10-some-html-elements-cant-be-grid-containers)
11. [Текстовое поле, которое является элементом сетки, сворачивается до нулевой ширины.](#11-a-textarea-that-is-a-grid-item-collapses-to-zero-width)
12. [Пространство, распространяемое на пустые направляющие, растягивается элементами с контентом.](#12-space-distributed-to-empty-tracks-spanned-by-an-item-with-content)
13. [Несостыковки с процентными паддингами и марджинами.](#13-inconsistency-with-percentage-padding-and-margins)
14. [fit-content растягивается.](#14-fit-content-is-stretching)

### 1. `grid-auto-rows` и `grid-auto-columns` должен принимать список направляющих.


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
    <th align="left">Тесты</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/Bdoome">1.1</a> — <em>bug</em>
    </td>
    <td>
     Firefox
    </td>
    <td>
      <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1339672">Firefox #1339672</a>
    </td>
    <td><a href="https://github.com/w3c/web-platform-tests/blob/master/css/css-grid-1/implicit-grids/grid-support-grid-auto-columns-rows-002.html">WPT</a></td>
  </tr>
</table>


Параметры`grid-auto-rows` и `grid-auto-columns` дают возможность задавать размеры направляющих в неявных сетках. Спецификация говорит:

> "Если заданы несколько размеров направляющих, шаблон повторяется по мере необходимости, чтобы найти размеры неявных направляющих. Первая неявная направляющая сетки перед явной сеткой принимает первый указанный размер и так далее вперед.; И последняя неявная направляющая сетки до того, как явная сетка получит последний указанный размер и так далее в обратном порядке." - [7.6 Implicit Track Sizing (Размеры Неявных Направляющих)](https://www.w3.org/TR/css-grid-1/#auto-tracks)

### 2. Повторная нотация должна заново принимать список направляющих.


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/prjjLy">2.1</a> — <em>bug</em>
    </td>
    <td>
     Firefox
    </td>
    <td>
      <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1341507">Firefox #1341507</a>
    </td>
  </tr>
</table>


Повторная нотация может принимать однотипное значение направляющей для повторения, например:

`repeat(3,200px);`

Или для списка направляющих:

`repeat(3, 200px 100px);`

Это возможно когда числа повторяются как `auto-fill` или `auto-fit` для объявления как можно большего количества направляющих.

> "Первый аргумент указывает количество повторений. Второй аргумент - это список направляющих, которые повторяются определенное количество раз." - [Синтакс `repeat()`](https://www.w3.org/TR/css-grid-1/#auto-repeat)

### 3. Поддержка фрагментации


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td></td>
    <td>
     Firefox
     <br>Chrome
     <br>Safari
    </td>
    <td>
      <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1266265">Firefox #1266265</a><br>
      <a href="https://bugs.chromium.org/p/chromium/issues/detail?id=614667">Chrome #614667</a>
</td>
  </tr>
</table>


В настоящее время существует ограниченная поддержка фрагментации в браузерах, поэтому такие функции, как свойства `break-*` вряд ли будут адекватно работать. Подробнее о том, как должна работать фрагментация в CSS Grid Layout: [12. Fragmenting Grid Layout](https://www.w3.org/TR/css-grid-1/#pagination). 

### 4. Выравнивание элементов с собственными размерами в сетках с фиксированными направляющими


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/eEpeXW/">4.1</a> — <em>bug</em>
    </td>
    <td>
     Firefox
    </td>
    <td>
      <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1385410">Firefox #1385410</a>
    </td>
  </tr>
</table>


В Firefox элементы с внутренним размером масштабируются, чтобы вписаться в сетку с фиксированным размером, а не вылезать за неё.

Я считаю, что правильное поведение в данном кейсе состоит в переполнении элементов с фиксированным размером, в соответствии с резолюцией WG [Issue 523](https://github.com/w3c/csswg-drafts/issues/523#issuecomment-300539109).

### 5. Элементы с внутренним соотношением сторон должны выравниваться в начале


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/LjprNE">5.1</a> — <em>bug</em><br>
      <a href="https://codepen.io/rachelandrew/pen/prjQRz">5.2</a> — <em>workaround</em>
    </td>
    <td>
     Safari 10 (fixed in Technical Preview)
    </td>
    <td></td>
  </tr>
</table>


В Safari 10 элемент сетки поумолчанию выравнивался как `stretch`, а не `start`.

#### Временное решение

Временным решением проблемы является декларирование `align-self: start` и `justify-self: start` для элемента.

### 6. The `grid-gap` параметры должны принимать значение в процентах


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/QMjPXq">6.1</a> — <em>bug</em>
    </td>
    <td>
     Safari 10 (fixed in <a href="https://developer.apple.com/safari/technology-preview/release-notes/#r29">Technical Preview 29</a>)
       </td>
    <td>
      <a href="https://bugs.webkit.org/show_bug.cgi?id=170764">WebKit #170764</a>
    </td>
  </tr>
</table>


В настоящее время процентные значения `grid-gap` помечаются как **рискованные** в Уровне 1 спецификации. Сильным вариантом использования для процентного значения является добавление некоторых функций грид-лейаута к макету, который уже использует разметку на флоатах или флексбоксах: [посмотрите твит от @minamarkham](https://twitter.com/minamarkham/status/891029296604618752). Эти системы должны использовать проценты для интервалов, чтобы создать «сетку».

### 7. Grid отступы должны вести себя как направляющие с авто-размерами?

Существует также несоответствие касательно процентного значения `grid-row-gap`,  если высота сетки не является фиксированной. Chrome, Safari TP, и Edge Preview сжимают разрыв между рядами до 0. Firefox вероятно использует проценты от общей высоты сетки.

- [Пример 7.1](https://codepen.io/rachelandrew/pen/xLZbMm)
- [CSS WG дискуссия](https://github.com/w3c/csswg-drafts/issues/509#issuecomment-318812608)

### 8. Установка максимальной высоты в процентах должна уменьшать изображение большего размера внутри направляющих сетки


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/YxqVNz">8.1</a> — <em>bug</em>
    </td>
    <td>
    Chrome
       </td>
    <td>
      <a href="https://bugs.chromium.org/p/chromium/issues/detail?id=750631">Chromium #750631</a>
    </td>
  </tr>
</table>


С помощью `max-height` изображение внутри направляющих фиксированного размера должно быть распределено так, чтобы изображение масштабировалось и  помещалось внутри сетки.На данный момент в Chrome это не работает и изображение вылезает за направляющие сетки. Использование max-width работает, когда изображение ограничено размером направляющих столбца, установка фиксированной высоты работает так, как ожидалось, как и установка максимальной высоты с использованием px (пикселей) в качестве единицы измерения.

### 9. `min-content` и `max-content` в направляющих


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/eEzbpN">9.1</a> — <em>bug</em>
    </td>
    <td>
     Safari 10 (fixed in Technology Preview)
    </td>
    <td><a href="https://bugs.webkit.org/show_bug.cgi?id=169195">WebKit #169195</a></td>
  </tr>
</table>


В Safari 10, значения `min-content`, `max-content` и `fit-content` были под префиксами. В Safari Technology Preview они уже работают без префиксов.

#### Временное решение

Как временное решение используйте префиксы `-webkit-min-content`, `-webkit-max-content` и `-webkit-fit-content`.

### 10. Некоторые HTML элементы не могут быть грид-контейнерами


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/dzXwRJ">10.1</a> — <em>bug (fieldset)</em><br>
      <a href="https://codepen.io/rachelandrew/pen/dzXwRJ">10.2</a> — <em>bug (button)</em>
    </td>
    <td>
    Chrome<br>
     Safari 10 (fieldset fixed in Technology Preview)
    </td>
    <td><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=375693">Chromium #375693</a></td>
  </tr>
</table>


В Chrome (и Safari 10) некоторые строчные элементы с `display: grid` не работают как грид-контейнер. Элемент button функционирует только как контейнер сетки в Firefox. Эта же проблема есть и в Flexbox: [подробней на странице Flexbugs](https://github.com/philipwalton/flexbugs#9-some-html-elements-cant-be-flex-containers).

### 11. Текстовое поле, которое является элементом сетки, сворачивается до нулевой ширины


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://yuheiy.github.io/pub/textarea-in-grid.html">11.1</a> — <em>bug</em>
    </td>
    <td>
    Chrome (on OSX only)
    </td>
    <td><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=727076">Chromium #727076</a></td>
  </tr>
</table>


В OS X Chrome, если текстовая область является элементом сетки, её ширина сворачивается до 0, когда в неё начинают вводить текст. *Ссылка на пример ведет на Chromium, поскольку из-за того, как CodePen загружается в iframe, баг, похоже, не воспроизводится.*

### 12. Пространство, распространяемое на пустые направляющие, растягивается элементы с контентом


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/vJXJQr">12.1</a> — <em>bug 1</em><br>
      <a href="https://codepen.io/rachelandrew/pen/aymLmq">12.2</a> — <em>bug 2</em>
    </td>
    <td>
    Firefox
    </td>
    <td>
<a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1386921">Firefox #1386921</a><br>
    <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1386932">Firefox #1386932</a>
</td>
  </tr>
</table>


Если у вас есть сетка с пустыми дорожками, которые связаны с элементами в сетке, этим дорожкам не следует назначать дополнительное пространство из-за перекрывания элемента. Firefox назначает дополнительное пространство (bug#1), связанный с тем, что Firefox делает разные вещи в зависимости от исходного порядка (bug#2).

Спасибо всем, кто помог изолировать эти баги [здесь](https://github.com/rachelandrew/gridbugs/issues/2). Дискуссия будет вам очень полезна, если вы действительно хотите разобраться в причинах багов.

### 13. Несостыковки с процентными паддингами и марджинами


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/brYXNQ">13.1 — <em>bug</em></a>
    </td>
    <td>
    Firefox & Edge match<br>
    Chrome & Webkit match
    </td>
    <td><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=229568">Chrome #229568</a></td>
  </tr>
</table>


Как указывает @mrego, [существует многолетняя проблема совместимости с процентным заполнением и полями как в Grid, так и в Flexbox](https://github.com/rachelandrew/gridbugs/issues/7). 

Firefox и Edge (Preview) разрешают процентные margin/padding для элементов в соответствии с их размером (шириной или высотой), Chrome всегда использует ширину, точно так же, как проценты разрешены в блочном лейауте. [Спецификация допускает обе возможности](https://drafts.csswg.org/css-grid/#item-margins)  и предполагает, что авторы не используют процентные паддинги и марджины.

### 14. fit-content растягивается


<table>
  <tr>
    <th align="left">Примеры</th>
    <th align="left">Пострадавшие браузеры</th>
    <th align="left">Отслеживание ошибок</th>
  </tr>
  <tr valign="top">
    <td>
      <a href="https://codepen.io/rachelandrew/pen/WEXBMR">14.1 — <em>bug</em></a>
    </td>
    <td>
    Chrome<br>
    Webkit
    </td>
    <td><a href="https://bugs.chromium.org/p/chromium/issues/detail?id=755994">Chrome #755994</a></td>
  </tr>
</table>


@simevidas заявляет, что [Chrome растягивает направляющии по размеру fit-content](https://github.com/rachelandrew/gridbugs/issues/9), а Firefox нет. Я считаю, что Firefox ведет себя в этом случае корректно, и направляющие должны быть привязаны к значению длины, если контент превышает значение. Edge (Preview) ведет себя в этом случае так же как и Firefox.

#### Временное решение

Обходным путем для этого является использование `justify-content: start` или `justify-content: end` для направляющих.

## Примечание

Gridbugs курируется и создан [@rachelandrew](https://twitter.com/rachelandrew) под вдохновлением от замечательного [Flexbugs](https://github.com/philipwalton/flexbugs). <br><br>Автор данного перевода - [Владимир Гуменюк (Vladimir Humeniuk)](https://github.com/VladimirHumeniuk).

## Содействие

Прежде всего [создавая новую ошибку](https://github.com/rachelandrew/gridbugs/issues), прикрепляйте сокращенный пример кода и пути воспроизведения ошибки. Если вам известны обходные пути также опишите их.

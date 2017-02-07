# Личный сайт-портфолио
https://kornilova-l.github.io

## О чем этот сайт
Этот сайт сделан для того, чтобы представить на нем мои рисунки.
На нем есть 3 основных раздела.
* Иллюстрации
* Линогравюры
* Наброски

Для каждого проекта есть своя станица, куда я добавила фотографии процесса создания рисунка. К некоторым рисункам я написала историю их создания. Советую прочитать историю [линогравюры "Туман"](https://kornilova-l.github.io/linocut-fog)

## Немножко космоса
[На главной странице](https://kornilova-l.github.io/) с помощью JavaScript сделана искусственная гравитация. Благодаря ей планеты двигаются по эллиптическим траекториям и увеличивают скорость, когда приближаются к звезде.

![скриншот страницы https://kornilova-l.github.io/](http://i91.fastpic.ru/big/2017/0208/53/375fe02cc656fe871e462ee0f1f7c653.jpg)

## Как работает гравитация?
Для того, чтобы планеты двигались, нам нужно посчитать их координаты в следующий момент времени, для этого нам нужно знать вектор скорости. Но вектор скорости постоянно меняется (если бы он не менялся, то планета бы летела по прямой).

Чтобы узнать, как меняется скорость, нам нужно узнать ускорение. Тут на помощь приходит второй закон Ньютона:
![формула ускорения](http://csfm.volgatech.net/elearning/Nurgaliev/pictures/formula2_4.jpg)

С m все понятно — это масса планеты. А как посчитать силу F?

Для подсчета силы гравитации используется известная всем со школы формула:
![формула всемирного тяготения](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0e/NewtonsLawOfUniversalGravitation.svg/400px-NewtonsLawOfUniversalGravitation.svg.png)

Получается, всё, что нужно делать, это постоянно считать ускорение для планет, обновлять вектор скорости, а затем считать новые координаты

## Подробнее о JavaScript коде
Основой код [animateSpaceObjects.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/animateSpaceObjects.js)

Для анимации используется requestAnimationFrame. Так как JS не может гарантировать, что код будет исполняться через фиксированные промежутки времени, для анимации используется таймер, который замеряет dt, этот промежуток времени затем будет использоваться при обновлении вектора скорости и координат.




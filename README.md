# Personal site-portfolio
https://kornilova-l.github.io

## What's this about?
This site was made for my drawings. It has 3 main sections:
* Illustrations
* Linocuts
* Sketches

For each drawing there is separate page where I added photos of process. For some of my drawings I added stories how I was inspired to draw it. This stories are awailable only in Russian language.

## What about cosmos?
[On main page](https://kornilova-l.github.io/) there is a gravity simulation. It is written in JavaScript. Planets are moving in elliptic path and their speed increases when they are close to the star.

![screen of main page](http://i91.fastpic.ru/big/2017/0208/53/375fe02cc656fe871e462ee0f1f7c653.jpg)

## How does gravity work?
To move a planet we have to count it's coordinates for next moment of time. For this we should know vector of velocity. But vector of velocity is always changing (in opposite case path of planet would be a strait line)

Fortunately we have second law of Newton so it is not difficult to figure out what an acceleration will be equal to.

![formula of acceleration](http://csfm.volgatech.net/elearning/Nurgaliev/pictures/formula2_4.jpg)

m is a mass of planet. But how we count force F?

For this we have to remember well known formula of gravity:

![formula of gravitation](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0e/NewtonsLawOfUniversalGravitation.svg/400px-NewtonsLawOfUniversalGravitation.svg.png)

It turns out that all we have to do is to count planet's acceleration, apdate vector of velocity and count new coordinates. Obviously we have to do it in loop. 

## About JavaScript
Code can be found here: [animateSpaceObjects.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/animateSpaceObjects.js)

JS cannot garantee that your timer code will be executed in time, so we have to create variable which will store how much time passed since last cicle. It will be used for updating velocity and coordinates.

The star has coordinates (0, 0) although it is in center of screen
Несмотря на то, что звезда находится в центре экрана, ее координаты всегда равны (0, 0), планеты крутятся так же вокруг начала координат. В центре экрана звезда оказывается благодаря тому, что функция отрисовки помещает ее туда. Это нужно для того, чтобы не приходилось пересчитывать положение объктов и их вектора скорости при масштабировании окна браузера.

### Класс космического объекта
Класс для космических объектов находится здесь: [ball.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/ball.js)
Создание новой планеты (или звезды):
```
var planet = new Ball(img, radius, mass)
```
Экземпляр класса содержит:
* изображение объекта
* радиус 
* массу
* координаты позиции x и y
* координаты вектора скорости
* ссылку на элемент \<canvas> (см ниже)

При этом создается новый элемент \<canvas> (он нужен для того, чтобы на нем отрисовывать двухмерную растровую графику), который помещается в конец \<body>. 
```
this.canvas = document.createElement('canvas');
document.getElementsByTagName('body')[0].appendChild(this.canvas);
```
Ссылка на canvas записывается в соответствующий параметр экземпляра. Для каждого космического объекта создается свой объект \<canvas>, так как при перемещении объекта, нужно сначала стереть его предыдущее положение, затем отрисовать новое. Если объекты расположены очень близко к друг другу, это сделать будет сложно. Есть другой вариант — стирать все объекты сразу, но тогда планеты будут мерцать, так как нужно относительно много времени для просчета и отрисовкой всех планет.

Класс содержит следующие методы:
* отрисовка объекта на его \<canvas>
* клонирование объекта
* поворот объекта на определенное количество градусов относительно звезды (используется для создания кругового массива планет)
* изменение коэфициента размера (коэфициент размера зависит от размера окна браузера)
* set и get методы, которые упрощают работу с координатами объекта и его вектором скорости

### Класс вектора
Координаты объекта и его скорость можно представить в виде пары значений (x, y). По сути и положение объекта и его скорость являются векторами. Чтобы упростить манипуляции ими используется специальный класс [Vector2D](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/vector2D.js)

Элемент класса содержит:
* координату x
* координату y

Методы класса:
* получение длины вектора
* получение длины вектора в квадрате
* сложение векторов
* увеличение вектора в k раз
* прибавление другого вектора, увеличенного в k раз
* поворот вектора вокруг заданного центра

Этот класс упрощает задачу обновления позиции объкта и его вектора скорости.

## Могу ли я сам что-нибудь поменять и посмотреть, как оно работает?
Репозиторий можно клонировать и изменить код файла [animateSpaceObjects.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/animateSpaceObjects.js). В начале задаются константы такие как масса планет и солнца, гравитационная постоянная
```
var m = 1; 
var M = 1000000; 
var G = 1;
```
Их можно поменять и посмотреть, что произойдет.

Переменная amountOfPlanets отвечает за количество планет в группе (в моем коде в группе 3 планеты).

Функция `createPlanets(amount, distance)` создает круговой массив, где amount - количество планет, distance - удаленность от солнца. В этой функции изначальная скорость планет равна `sqrt(G*M*m/distance)*0.75` это сделано для того, чтобы планеты двигались по эллиптическим орбитам. Если задать скорость `sqrt(G*M*m/distance)`, то орбита будет круговая.

В ф-ции `init()` создается 2 группы по 3 планеты.

Планету можно создать и не используя функцию createPlanets, например так:
```
var planet = new Ball (planetImg, 30, m); // создаем новую планету
planet.pos2D = new Vector2D(0,200); // задаем ей начальные координаты
planet.velo2D = new Vector2D(250, 0); // задаем вектор скорости планете
planets.push(planet); // добавляем новую планету в массив к другим планетам
```
Главное - не забыть добавить новую планету в массив planets, иначе она не будет двигаться.

## О чем этот сайт
Это сайт создан для представления моих рисунков. В нем есть 3 основных раздела.
* Иллюстрации
* Линогравюры
* Наброски

Для каждого проекта есть своя страница, куда я добавила фотографии процесса создания рисунка. К некоторым рисункам я добавила историю их создания. Советую прочитать историю [линогравюры "Туман"](https://kornilova-l.github.io/linocut-fog)

## Немножко космоса
[На главной странице](https://kornilova-l.github.io/) с помощью JavaScript сделана искусственная гравитация. Благодаря ей планеты двигаются по эллиптическим траекториям и увеличивают скорость, когда приближаются к звезде.

![скриншот страницы https://kornilova-l.github.io/](http://i91.fastpic.ru/big/2017/0208/53/375fe02cc656fe871e462ee0f1f7c653.jpg)

## Как работает гравитация?
Для того, чтобы планеты двигались, необходимо посчитать их координаты в следующий момент времени, для этого нам нужно знать вектор скорости. Но вектор скорости постоянно меняется (если бы он не менялся, то планета бы летела по прямой).

Чтобы узнать, как меняется скорость, нам нужно узнать ускорение. Тут на помощь приходит второй закон Ньютона:
![формула ускорения](http://csfm.volgatech.net/elearning/Nurgaliev/pictures/formula2_4.jpg)

С m всё понятно — это масса планеты. А как посчитать силу F?

Для подсчета силы гравитации используется известная всем со школы формула:
![формула всемирного тяготения](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0e/NewtonsLawOfUniversalGravitation.svg/400px-NewtonsLawOfUniversalGravitation.svg.png)

Получается, всё, что нужно делать, это постоянно считать ускорение для планет, обновлять вектор скорости, а затем считать новые координаты

## Подробнее о JavaScript коде
Основой код [animateSpaceObjects.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/animateSpaceObjects.js)

Для анимации используется requestAnimationFrame. Так как JS не может гарантировать, что код будет исполняться через фиксированные промежутки времени, для анимации используется таймер, который замеряет dt, этот промежуток времени затем будет использоваться при обновлении вектора скорости и координат.

Несмотря на то, что звезда находится в центре экрана, ее координаты всегда равны (0, 0), планеты крутятся так же вокруг начала координат. В центре экрана звезда оказывается благодаря тому, что функция отрисовки помещает ее туда. Это нужно для того, чтобы не приходилось пересчитывать положение объктов и их вектора скорости при масштабировании окна браузера.

### Класс космического объекта
Класс для космических объектов находится здесь: [ball.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/ball.js)
Создание новой планеты (или звезды):
```
var planet = new Ball(img, radius, mass)
```
Экземпляр класса содержит:
* изображение объекта
* радиус 
* массу
* координаты позиции x и y
* координаты вектора скорости
* ссылку на элемент \<canvas> (см ниже)

При этом создается новый элемент \<canvas> (он нужен для того, чтобы на нем отрисовывать двухмерную растровую графику), который помещается в конец \<body>. 
```
this.canvas = document.createElement('canvas');
document.getElementsByTagName('body')[0].appendChild(this.canvas);
```
Ссылка на canvas записывается в соответствующий параметр экземпляра. Для каждого космического объекта создается свой объект \<canvas>, так как при перемещении объекта, нужно сначала стереть его предыдущее положение, затем отрисовать новое. Если объекты расположены очень близко к друг другу, это сделать будет сложно. Есть другой вариант — стирать все объекты сразу, но тогда планеты будут мерцать, так как нужно относительно много времени для просчета и отрисовкой всех планет.

Класс содержит следующие методы:
* отрисовка объекта на его \<canvas>
* клонирование объекта
* поворот объекта на определенное количество градусов относительно звезды (используется для создания кругового массива планет)
* изменение коэфициента размера (коэфициент размера зависит от размера окна браузера)
* set и get методы, которые упрощают работу с координатами объекта и его вектором скорости

### Класс вектора
Координаты объекта и его скорость можно представить в виде пары значений (x, y). По сути и положение объекта и его скорость являются векторами. Чтобы упростить манипуляции ими используется специальный класс [Vector2D](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/vector2D.js)

Элемент класса содержит:
* координату x
* координату y

Методы класса:
* получение длины вектора
* получение длины вектора в квадрате
* сложение векторов
* увеличение вектора в k раз
* прибавление другого вектора, увеличенного в k раз
* поворот вектора вокруг заданного центра

Этот класс упрощает задачу обновления позиции объкта и его вектора скорости.

## Могу ли я сам что-нибудь поменять и посмотреть, как оно работает?
Репозиторий можно клонировать и изменить код файла [animateSpaceObjects.js](https://github.com/kornilova-l/kornilova-l.github.io/blob/master/js/animateSpaceObjects.js). В начале задаются константы такие как масса планет и солнца, гравитационная постоянная
```
var m = 1; 
var M = 1000000; 
var G = 1;
```
Их можно поменять и посмотреть, что произойдет.

Переменная amountOfPlanets отвечает за количество планет в группе (в моем коде в группе 3 планеты).

Функция `createPlanets(amount, distance)` создает круговой массив, где amount - количество планет, distance - удаленность от солнца. В этой функции изначальная скорость планет равна `sqrt(G*M*m/distance)*0.75` это сделано для того, чтобы планеты двигались по эллиптическим орбитам. Если задать скорость `sqrt(G*M*m/distance)`, то орбита будет круговая.

В ф-ции `init()` создается 2 группы по 3 планеты.

Планету можно создать и не используя функцию createPlanets, например так:
```
var planet = new Ball (planetImg, 30, m); // создаем новую планету
planet.pos2D = new Vector2D(0,200); // задаем ей начальные координаты
planet.velo2D = new Vector2D(250, 0); // задаем вектор скорости планете
planets.push(planet); // добавляем новую планету в массив к другим планетам
```
Главное - не забыть добавить новую планету в массив planets, иначе она не будет двигаться.

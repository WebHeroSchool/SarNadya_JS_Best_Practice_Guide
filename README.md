# JS Best Practice Guide


## Переменные

## 1. Значимые имена

Убедитесь, что ваши переменные названы осмысленно. Это уменьшит необходимость в дополнительных комментариях, так как ваш код говорит сам за себя.

``` js
//BAD
cosnt ddmmyyyy = new Date();

//GOOD
const date = new Date();
```


## 2. Имена с возможностью поиска


Мы тратим больше времени на чтение кода, чем на его написание. Поэтому важно, чтобы он был читаемым и доступным для поиска. Если вы видите значение и понятия не имеете, что оно делает или должно делать, это сбивает с толку.

``` js
//BAD
//Reader would have no clue on what 86400000 is
setTimeout(randomFunction, 86400000);

//GOOD
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86_400_000;
setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

## 3. Избегайте мысленного сопоставления

Не заставляйте людей запоминать контекст переменной. Переменные следует понимать даже тогда, когда читателю не удалось проследить всю историю их возникновения.

``` js
// BAD
const names = ["John", "Jane", "Joseph"];
names.forEach(v => {
 doStuff();
 doSomethingExtra();
 // ...
 // ...
 // ...
 // What is this 'v' for?
 dispatch(v);
});

// GOOD
const names = ["John", "Jane", "Joseph"];
names.forEach(name => {
 doStuff();
 doSomethingExtra();
 // ...
 // ...
 // ...
 // 'name' makes sense now
 dispatch(name);
});
```

## 4. Не добавлять нежелательный контекст

Если имя класса или объекта говорит вам, что это такое, не включайте это в имя переменной.

``` js
// BAD
const Book = {
 bookName: "Programming with JavaScript",
 bookPublisher: "Penguin",
 bookColour: "Yellow"
};
function wrapBook(book) {
 book.bookColour = "Brown";
}

// GOOD
const Book = {
 name: "Programming with JavaScript",
 publisher: "Penguin",
 colour: "Yellow"
};
function wrapBook(book) {
 book.colour = "Brown";
}
```

## 5. Использовать аргументы по умолчанию

Устанавливайте праметрам значения по-умолчанию, чтобы получить более читаемый код.

``` js
// BAD
function addEmployeeType(type){
 const employeeType = type || "intern";
 //............
}

// GOOD
function addEmployeeType(type = "intern"){
 //............
}
```


## 6. Используйте строгую проверку типа

Используйте === вместо == . Это поможет избежать всяких ненужных проблем в дальнейшем. Если не делать проверки должным образом, то это может существенно повлиять на логику программы.

``` js
 0 == false // true
 0 === false // false
 2 == "2" // true
 2 === "2" // false
```


## 7. Не загрязняйте глобальное окружение.

Загрязнение глобального окружения - плохая практика в JavaScript, потому что вы можете столкнуться с другой библиотекой, и пользователь вашего API будет не подозревать об изменениях, пока не получит ошибку. Например, если вы хотите расширить нативный метод JavaScript Array, чтобы он имел другой метод diff, который бы показывал разницу между двумя массивами. Вы можете написать свой метод в Array.prototype, но можетe столкнуться с другой библиотекой, которая пыталась вызвать тот же самый diff-метод для реализации другой возможности.

Вот почему было бы намного лучше просто использовать классы ES2015/ES6 и просто расширить массив в глобальном окружении

``` js
//BAD
Array.prototype.diff = function diff(comparisonArray) {
 const hash = new Set(comparisonArray);
 return this.filter(elem => !hash.has(elem));
};

//GOOD
class SuperArray extends Array {
 diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
 }
}
```


## 8. По-минимуму количество параметров функции

В идеале следует избегать большого количества параметров. Уменьшение количества параметров функции облегчило бы ее тестирование.

Один или два аргумента - идеальный вариант, а три следует по возможности избегать. Все, что больше этого, должно быть сведено воедино. Обычно, если у вас более двух аргументов, то ваша функция пытается сделать слишком много. В тех случаях, когда это не так, в большинстве случаев в качестве аргумента будет достаточно высокоуровненового объекта.

``` js
//BAD
function createMenu(title, body, buttonText, cancellable) {
 // ...
}
createMenu("Foo", "Bar", "Baz", true);

//GOOD
function createMenu({ title, body, buttonText, cancellable }) {
 // ...
}
createMenu({
 title: "Foo",
 body: "Bar",
 buttonText: "Baz",
 cancellable: true
});
```


## 9. Функции должны делать что-то одно

Это одно из важнейших правил в программировании. Если функция выполняет более чем одну вещь, ee сложнее проверить и отладить. Если функция изолированна и делает что-то одно, ее проще отрефакторить

::: warning
Функции должны делать одну вещь. Они должны делать это хорошо. Они должны делать только это. - Роберт К. Мартин
:::

``` js
//BAD
function notifyListeners(listeners) {
 listeners.forEach(listener => {
  const listenerRecord = database.lookup(listener);
  if (listenerRecord.isActive()) {
   notify(listener);
  }
 });
}

//GOOD
function notifyActiveListeners(listeners) {
 listeners.filter(isListenerActive).forEach(notify);
}

function isListenerActive(listener) {
 const listenerRecord = database.lookup(listener);
 return listenerRecord.isActive();
}
```


## 10. Удалять дубликат кода

Делайте все возможное, чтобы избежать дублирования кода. Писать один и тот же код более одного раза не только бесполезно, но и еще приведет к проблемам при рефакторинге кода. Вместо того, чтобы одно изменение повлияло на все соответствующие модули, вы должны найти все дубликаты модулей и повторить это изменение.

Часто дублирование в коде происходит потому, что два или более модуля имеют небольшие различия, так как у вас есть две или более немного отличающихся сущностей, которые имеют много общего.

Небольшие различия заставляют вас иметь очень похожие модули. Удаление дублирующего кода означает создание абстракции, которая может обрабатывать этот набор различных сущностей с помощью одной функции/модуля/класса.

``` js
//BAD
function showDeveloperList(developers) {
 developers.forEach(developer => {
  const expectedSalary = developer.calculateExpectedSalary();
  const experience = developer.getExperience();
  const githubLink = developer.getGithubLink();
  const data = {
   expectedSalary,
   experience,
   githubLink
  };
  render(data);
 });
}

function showManagerList(managers) {
 managers.forEach(manager => {
  const expectedSalary = manager.calculateExpectedSalary();
  const experience = manager.getExperience();
  const portfolio = manager.getMBAProjects();
  const data = {
   expectedSalary,
   experience,
   portfolio
  };
  render(data);
 });
}

//GOOD
function showEmployeeList(employees) {
 employees.forEach(employee => {
  const expectedSalary = employee.calculateExpectedSalary();
  const experience = employee.getExperience();
  const data = {
   expectedSalary,
   experience
  };
  switch (employee.type) {
   case "manager":
    data.portfolio = employee.getMBAProjects();
    break;
   case "developer":
    data.githubLink = employee.getGithubLink();
    break;
  }
  render(data);
 });
}
```
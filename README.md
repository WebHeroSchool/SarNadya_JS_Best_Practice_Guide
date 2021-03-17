# JS Best Practice Guide


## ����������

## 1. �������� �����

���������, ��� ���� ���������� ������� ����������. ��� �������� ������������� � �������������� ������������, ��� ��� ��� ��� ������� ��� �� ����.

``` js
//BAD
cosnt ddmmyyyy = new Date();

//GOOD
const date = new Date();
```


## 2. ����� � ������������ ������


�� ������ ������ ������� �� ������ ����, ��� �� ��� ���������. ������� �����, ����� �� ��� �������� � ��������� ��� ������. ���� �� ������ �������� � ������� �� ������, ��� ��� ������ ��� ������ ������, ��� ������� � �����.

``` js
//BAD
//Reader would have no clue on what 86400000 is
setTimeout(randomFunction, 86400000);

//GOOD
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86_400_000;
setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

## 3. ��������� ���������� �������������

�� ����������� ����� ���������� �������� ����������. ���������� ������� �������� ���� �����, ����� �������� �� ������� ���������� ��� ������� �� �������������.

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

## 4. �� ��������� ������������� ��������

���� ��� ������ ��� ������� ������� ���, ��� ��� �����, �� ��������� ��� � ��� ����������.

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

## 5. ������������ ��������� �� ���������

�������������� ��������� �������� ��-���������, ����� �������� ����� �������� ���.

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


## 6. ����������� ������� �������� ����

����������� === ������ == . ��� ������� �������� ������ �������� ������� � ����������. ���� �� ������ �������� ������� �������, �� ��� ����� ����������� �������� �� ������ ���������.

``` js
 0 == false // true
 0 === false // false
 2 == "2" // true
 2 === "2" // false
```


## 7. �� ����������� ���������� ���������.

����������� ����������� ��������� - ������ �������� � JavaScript, ������ ��� �� ������ ����������� � ������ �����������, � ������������ ������ API ����� �� ����������� �� ����������, ���� �� ������� ������. ��������, ���� �� ������ ��������� �������� ����� JavaScript Array, ����� �� ���� ������ ����� diff, ������� �� ��������� ������� ����� ����� ���������. �� ������ �������� ���� ����� � Array.prototype, �� �����e ����������� � ������ �����������, ������� �������� ������� ��� �� ����� diff-����� ��� ���������� ������ �����������.

��� ������ ���� �� ������� ����� ������ ������������ ������ ES2015/ES6 � ������ ��������� ������ � ���������� ���������

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


## 8. ��-�������� ���������� ���������� �������

� ������ ������� �������� �������� ���������� ����������. ���������� ���������� ���������� ������� ��������� �� �� ������������.

���� ��� ��� ��������� - ��������� �������, � ��� ������� �� ����������� ��������. ���, ��� ������ �����, ������ ���� ������� �������. ������, ���� � ��� ����� ���� ����������, �� ���� ������� �������� ������� ������� �����. � ��� �������, ����� ��� �� ���, � ����������� ������� � �������� ��������� ����� ���������� ������������������ �������.

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


## 9. ������� ������ ������ ���-�� ����

��� ���� �� ��������� ������ � ����������������. ���� ������� ��������� ����� ��� ���� ����, ee ������� ��������� � ��������. ���� ������� ������������ � ������ ���-�� ����, �� ����� �������������

::: warning
������� ������ ������ ���� ����. ��� ������ ������ ��� ������. ��� ������ ������ ������ ���. - ������ �. ������
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


## 10. ������� �������� ����

������� ��� ���������, ����� �������� ������������ ����. ������ ���� � ��� �� ��� ����� ������ ���� �� ������ ����������, �� � ��� �������� � ��������� ��� ������������ ����. ������ ����, ����� ���� ��������� �������� �� ��� ��������������� ������, �� ������ ����� ��� ��������� ������� � ��������� ��� ���������.

����� ������������ � ���� ���������� ������, ��� ��� ��� ����� ������ ����� ��������� ��������, ��� ��� � ��� ���� ��� ��� ����� ������� ������������ ���������, ������� ����� ����� ������.

��������� �������� ���������� ��� ����� ����� ������� ������. �������� ������������ ���� �������� �������� ����������, ������� ����� ������������ ���� ����� ��������� ��������� � ������� ����� �������/������/������.

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
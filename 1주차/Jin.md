# Jin
리팩터링 스터디
---

# 냄새 1. 이해하기 힘든 이름
mysterious name

## 함수 선언 변경하기
change function declaration  
함수에 주석을 작성한 다음, 주석을 함수 이름으로 만들어 본다.

## 변수 이름 변경하기
rename variable  
처음부터 이름을 잘 짓기는 어렵다. 나중에 보면 적절한 이름이 떠오를 수 있다.

## 필드 이름 바꾸기
rename field  
갑자기 왜 record 자료 구조의 필드 이름이 나왔을까?

# 냄새 2. 중복 코드
duplicated code

## 함수 추출하기
extract function  
장점: 이름을 부여할 수 있다.
한 줄짜리 코드라도 괜찮다.

## 코드 정리하기
slide statements
```
const pricingPlan = retrievePricingPlan();
const order = retreiveOrder();
let charge;
const chargePerUnit = pricingPlan.unit;
->
const pricingPlan = retrievePricingPlan();
const chargePerUnit = pricingPlan.unit;
const order = retreiveOrder();
let charge;
```

## 메서드 올리기
pull up method
```
class Employee {...}

class Salesman extends Employee {
  get name() {...}
}

class Engineer extends Employee {
  get name() {...}
}
->
class Employee {
  get name() {...}
}

class Salesman extends Employee {...}
class Engineer extends Employee {...}
```

# 냄새 3. 긴 함수
long function

## 임시 변수를 질의 함수로 바꾸기
replace temp with query
```
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000)
  return basePrice * 0.95;
else
  return basePrice * 0.98;
->
get basePrice() {this._quantity * this._itemPrice;}

...

if (this.basePrice > 1000)
  return this.basePrice * 0.95;
else
  return this.basePrice * 0.98;
```

## 매개변수 객체 만들기
introduce parameter object
```
function amountInvoiced(startDate, endDate) {...}
function amountReceived(startDate, endDate) {...}
function amountOverdue(startDate, endDate) {...}
->
function amountInvoiced(aDateRange) {...}
function amountReceived(aDateRange) {...}
function amountOverdue(aDateRange) {...}
```

## 객체 통째로 넘기기
preserve whole object
```
  const low = aRoom.daysTempRange.low;
  const high = aRoom.daysTempRange.high;
  if (aPlan.withinRange(low, high))
->
  if (aPlan.withinRange(aRoom.daysTempRange))
```


## 함수를 명령으로 바꾸기
replace function with command
```
  function score(candidate, medicalExam, scoringGuide) {
    let result = 0;
    let healthLevel = 0;
    // long body code
  }
->
class Scorer {
  constructor(candidate, medicalExam, scoringGuide) {
    this._candidate = candidate;
    this._medicalExam = medicalExam;
    this._scoringGuide = scoringGuide;
  }

  execute() {
    this._result = 0;
    this._healthLevel = 0;
    // long body code
  }
}
```
클래스로 만들어서 execute 구문을 실행한다.

## 조건문 분해하기
decompose conditional
```
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd))
  charge = quantity * plan.summerRate;
else
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
->
if (summer())
  charge = summerCharge();
else
  charge = regularCharge();
```
## 반복문 쪼개기
split loop
```
  let averageAge = 0;
  let totalSalary = 0;
  for (const p of people) {
    averageAge += p.age;
    totalSalary += p.salary;
  }
  averageAge = averageAge / people.length;
->
  let totalSalary = 0;
  for (const p of people) {
    totalSalary += p.salary;
  }

  let averageAge = 0;
  for (const p of people) {
    averageAge += p.age;
  }
  averageAge = averageAge / people.length;
```

## 조건문을 다형성으로 바꾸기
replace conditional with polymorphism
```
switch (bird.type) {
  case 'EuropeanSwallow':
    return "average";
  case 'AfricanSwallow':
    return (bird.numberOfCoconuts > 2) ? "tired" : "average";
  case 'NorwegianBlueParrot':
    return (bird.voltage > 100) ? "scorched" : "beautiful";
  default:
    return "unknown";
->
class EuropeanSwallow {
  get plumage() {
    return "average";
  }
class AfricanSwallow {
  get plumage() {
     return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
class NorwegianBlueParrot {
  get plumage() {
     return (this.voltage > 100) ? "scorched" : "beautiful";
  }
```


# 냄새 4. 긴 매개변수 목록
long parameter list  
정확한 기준은 없는 것 같다.


## 매개변수를 질의 함수로 바꾸기
replace parameter with query
```
availableVacation(anEmployee, anEmployee.grade);

function availableVacation(anEmployee, grade) {
  // calculate vacation...
->
availableVacation(anEmployee)

function availableVacation(anEmployee) {
  const grade = anEmployee.grade;
  // calculate vacation...
```

## 플래그 인수 제거하기
remove flag argument
```
function setDimension(name, value) {
  if (name === "height") {
    this._height = value;
    return;
  }
  if (name === "width") {
    this._width = value;
    return;
  }
}

function setHeight(value) {this._height = value;}
function setWidth (value) {this._width = value;}
```
## 여러 함수를 클래스로 묶기
combine functions into class
```
function base(aReading) {...}
function taxableCharge(aReading) {...}
function calculateBaseCharge(aReading) {...}
->
class Reading {
  base() {...}
  taxableCharge() {...}
  calculateBaseCharge() {...}
}
```

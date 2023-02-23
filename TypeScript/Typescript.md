# TypeScript

### Strongly Typed

작성한 코드를 JavaScript로 변환(브라우저는 TypeScript가 아니라 JavaScript를 이해)

## Type

```TypeScript
const user: {
    name: string
    age?: number //선택적 변수(optional parameter)
} = {
    name: "user1"
}

if (user.age < 10) { //err: user.age의 타입 설정 필요
}

if(user.age && user.age < 10) {
}
```

## Alias

여러 변수에 재사용 가능한 타입 생성

```TypeScript
type Name = string
type Age = number

type User = {
    name: Name
    age?: Age
}

const user1: User = {
    name: "user1"
}
```

## Readonly

선언 후 수정 불가함을 설정(JavaScript에는 존재하지 않는 속성)

```TypeScript
type User = {
    readonly name: string
    age: number
}

function makeUser (name:string): User {
    return {
        name
    }
}

const user1 = makeUser("user1")
user1.name = "user2" //err
```

```TypeScript
const numbers : readonly number[] = [1,2,3,4]
numbers.push(1) //err
```

## Tuple

최소한의 길이를 가지며 정해진 순서, 개수를 따라 배열되도록 함

```TypeScript
const user: [string, number, boolean] = ["user", 1, true]
```

## Any

TypeScript의 보호 장치를 비활성화

## Unknown

변수의 타입을 알지 못하는 경우

```TypeScript
let a: unknown

if (typeOf a === "number") {
    let b = a + 1
}
if (typeOf a === "string") {
    let b = a.toUpperCase()
}
```

## Void

아무것도 return하지 않는 함수에서 사용

```TypeScript
function hello() {
    console.log("hello")
}
const a = hello()
a.toUpperCase() //err
```

## Never

함수가 return하지 않는 경우

```TypeScript
function user (name: string|number): never {
    if (typeOf name === "string") {
        name
    } else if (typeOf name === "number") {
        name
    } else {
        name //실행되지 않아야 함
    }
}
```

## Call Signatures

함수의 변수와 return 값의 타입을 미리 선언(함수가 어떻게 실행되는지)

```TypeScript
type Add = (a: number, b: number) => number

const add: Add = (a, b) => a + b
```

## Overloading

하나의 함수가 여러 개의 Call Signature를 가지는 경우 발생

```TypeScript
//변수의 타입이 다른 경우
type Add = {
    (a: number, b: number): number
    (a: number, b: string): number
}

const add: Add = (a, b) => {
    if (typeOf b === "string") return a
    return a + b
}

//변수의 개수가 다른 경우
type Add2 = {
    (a: number, b: number): number
    (a: number, b: number, c: number): number
}

const add2: Add2 = (a, b, c?: number) => {
    if (c) return a + b + c
    return a + b
}
```

```TypeScript
type Config = {
    path: string
    state: number
}

type Push = {
    (path: string): void
    (config: Config): void
}

const push: Push = (config) => {
    if (typeOf config === "string") {
        console.log(config)
    } else {
        console.log(config.path) //객체임을 추론
    }
}
```

## Polymorphism(다형성)

poly(여러가지의) + morphos(형태, 구조)

```TypeScript
type SuperPrint = {
    (arr: number[]): void
    (arr: string[]): void
    (arr: boolean[]): void
}

superPrint([1, 2, true, "1"]) //err: Call Signature에 지정된 타입이 아님
```

Generic Type을 사용해서 함수의 Call Signature를 만들어 줄 수 있음(TypeScript의 추론 유도)

- Generic Type: 타입의 placeholder
- Concrete Type: number, boolean, void 등 일반적인 타입 종류

```TypeScript
type SuperPrint = {
    <T>(arr: T[]): void
}

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i))
}

type SuperReturn = {
    <T>(arr: T[]): T
}

const superReturn = (arr) => a[0]

//여러 개의 Generic Type을 사용하는 경우
type SuperPrint = {
    <T, M>(arr: T[], x: M): T
}
```

### (참고) Any 대신 Generic Type을 사용하는 이유

```TypeScript
type SuperReturn = {
    (arr: any[]): any
}

const superReturn: SuperReturn = (arr) => arr[0]

let a = superReturn([1, "b", true]);
a.toUpperCase(); //err 발생하지 않음


type SuperReturn = {
    <T>(arr: T[]): T
}

const superReturn: SuperReturn = (arr) => arr[0]

let a = superReturn([1, "b", true]);
a.toUpperCase(); //Call Signature를 각각의 concrete type으로 추가, err 발생
```

```TypeScript
type Player<T> = {
    name: string
    extraInfo: T
};

(예시 1) const player1 : Player<{age:number}> = {
    name: "player1",
    extraInfo: {
        age: 10
    }
}

(예시 2) type MeExtra = {
    age: number
}

const player1: Player<MeExtra> = {
    name: "player1",
    extraInfo: {
        age: number
    }
}

const player2: Player<null> = {
    name: "player2",
    extraInfo: null
};
```

## Class

| 접근/실행 가능 여부 | private | protected | public |
| ------------------- | ------- | --------- | ------ |
| 해당 클래스 내부    | O       | O         | O      |
| 상속받은 클래스     | X       | O         | O      |
| 인스턴스 생성       | X       | X         | O      |

## Interface

객체의 모양을 특정할 수 있음

### (참고) Type과 Interface 비교

- Type의 용도

  - 객체 모양 특정
  - Type Alias 지정
  - 값을 제한하여 지정하는 경우

    ```TypeScript
    type Team = "1" | "2" | "3"

    type Player = {
        name: string
        team: Team
    }
    ```

- 확장/상속 방식

```TypeScript
//Interface
interface PlayerA {
    name: string
}
interface PlayerAA extends PlayerA {
    age: number
}
const playerA: PlayerAA = {
    name: "playerA",
    age: 12
}
//Type
type PlayerB = {
    name: string
}
type PlayerBB = PlayerB & {
    age: number
}
const playerB: PlayerBB = {
    name:"playerB",
    age: 12
}
```

- 속성 추가 가능 여부

```TypeScript
//Interface
interface PlayerA {
    name: string
}
interface PlayerA {
    age: number
}
const playerA: PlayerA = {
    name: "playerA",
    age: 12
}
//Type
type PlayerB = {
    name: string
}
type PlayerB = { //err
    age: number
}
```

```TypeScript
//abstract class
abstract class User {
    constructor ( //추상 클래스: 다른 클래스가 상속받을 수 있는 클래스, 인스턴스 생성 불가
        public firstName: string,
        public lastName: string
    ) {}
    abstract fullName(): string //추상 메소드: 실행되어야 하는 함수의 Call Signature 작성
    abstract sayHi(name: string): string
    getFullName() { //메소드
        return `${this.firstName} ${this.lastName}`
    }
}

class Player extends User {
    fullName() { //추상 클래스를 상속받기 때문에 추상 메소드 구현 필요
        return `${this.firstName} ${this.lastName}`
    }
    sayHi(name: string) {
        return `Hello ${name}. My name ${this.fullName()}`
    }
}

//interface
interface User {
    firstName: string
    lastName: string
    sayHi(name: string): string
    fullName(): string
}

class Player implements User { //클래스가 상속받음
    constructor (
        public firstName: string //protected, private 속성 사용 불가능
        public lastName: string
    ) {}
    fullName() {
        return `${this.firstName} ${this.lastName}`
    }
    sayHi(name: string) {
        return `Hello ${name}. My name ${this.fullName()}`
    }
}
```

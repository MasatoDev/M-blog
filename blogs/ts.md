
| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |

![LGTMcat](https://user-images.githubusercontent.com/46220963/189363452-b95d2fd8-fe2b-475d-b5c6-82026181d589.jpeg)


# Prevent unnecessary network requests with the HTTP Cache

https://web.dev/http-cache/#invalidating_and_updating_cached_responses

※related to revving: https://developer.mozilla.org/ja/docs/Web/HTTP/Caching#revved_resources
https://web.dev/http-cache/#invalidating_and_updating_cached_responses

# Caching bestr practices & max-age gotchas

https://jakearchibald.com/2016/caching-best-practices/

# Http-cache flowchart

https://web.dev/http-cache/#flowchart

# The Basis of HTTP caching

https://developer.mozilla.org/ja/docs/Web/HTTP/Caching
https://www.typescriptlang.org/docs/handbook/intro.html

# Narrowing

## Truthiness narrowing

```ts
Boolean('hello'); // type: boolean, value: true
!!'world'; // type: true,    value: true
```

下記は false

- 0
- NaN
- "" (the empty string)
- 0n (the bigint version of zero)
- null
- undefined

```ts
function printAll(strs: string | string[] | null) {
  // !!!!!!!!!!!!!!!!
  //  DON'T DO THIS!
  //   KEEP READING
  // !!!!!!!!!!!!!!!!
  if (strs) {
    if (typeof strs === "object") {
      for (const s of strs) {
        console.log(s);
      }
    } else if (typeof strs === "string") {
      console.log(strs);
    }
  }
```

上記は空の文字列も省いてしまう危険性がある。

## The in operator narrowing

```ts
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ('swim' in animal) {
    return animal.swim();
  }

  return animal.fly();
}
```

## instanceof narrowing

```ts
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());

(parameter) x: Date
  } else {
    console.log(x.toUpperCase());

(parameter) x: string
  }
}
```

## predicates

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

//isはtrueの場合Fishの方がつく
```

:::note
`&`　は intersection

```ts
type TwoDimensionalPoint = {
  x: number;
  y: number;
};

type Z = {
  z: number;
};

type ThreeDimensionalPoint = TwoDimensionalPoint & Z;

const p: ThreeDimensionalPoint = {
  x: 0,
  y: 1,
  z: 2,
};
```

インターセクションを使いこなす

https://typescriptbook.jp/reference/values-types-variables/intersection
:::

# More on Function

## Construct Signature

:::tip

`new` operator

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);
// expected output: "Eagle"
```

:::

```ts
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor('hello');
}
```

## constraints

```ts
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}

// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest('alice', 'bob');
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
```

## overload (optional)

https://typescriptbook.jp/reference/functions/overload-functions

```ts
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
const d3 = makeDate(1, 3); // No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
```

## Declaring this in a Function

```ts
const user = {
  id: 123,

  admin: false,
  becomeAdmin: function () {
    this.admin = true;
  },
};

interface DB {
  filterUsers(filter: (this: User) => boolean): User[];
}

const db = getDB();
const admins = db.filterUsers(function (this: User) {
  return this.admin;
});
```

Note that you need to use function and not arrow functions to get this behavior.

## Other type to know about

- void
  - void is not the same as undefined.
- object
  - object is not Object. Always use object!
    - This is different from the empty object type { }, and also different from the global type Object. It’s very likely you will never use Object.
- unknown
  - This is useful when describing function types because you can describe functions that accept any value without having any values in your function body.
- never
  - The never type represents values which are never observed. In a return type, this means that the function throws an exception or terminates execution of the program.
-

:::tip
call/apply/bind

https://qiita.com/hosomichi/items/155af03120a330fd4437
https://qiita.com/hosomichi/items/e11ad0c4ea79db2dee84

:::

## parameter destructuring

```ts
// Same as prior example
type ABC = { a: number; b: number; c: number };
function sum({ a, b, c }: ABC) {
  console.log(a + b + c);
}
```

# Object Types

## read only

```ts
interface Home {
  readonly resident: { name: string; age: number };
}

function visitForBirthday(home: Home) {
  // We can read and update properties from 'home.resident'.
  console.log(`Happy birthday ${home.resident.name}!`);
  home.resident.age++;
}

function evict(home: Home) {
  // But we can't write to the 'resident' property itself on a 'Home'.
  home.resident = {
Cannot assign to 'resident' because it is a read-only property.
    name: "Victor the Evictor",
    age: 42,
  };
}
```

## extends

```ts
interface Colorful {
  color: string;
}

interface Circle {
  radius: number;
}

interface ColorfulCircle extends Colorful, Circle {}

const cc: ColorfulCircle = {
  color: 'red',
  radius: 42,
};
```

## intersection

```ts
interface Colorful {
  color: string;
}
interface Circle {
  radius: number;
}

type ColorfulCircle = Colorful & Circle;
```

:::important
extends vs intersection
The principle difference between the two is how conflicts are handled,
:::

## generic object type

```ts
interface Box<Type> {
  contents: Type;
}
```

:::tip
unknown vs any

https://dmitripavlutin.com/typescript-unknown-vs-any/

unknown is recommended over any because it provides safer typing — you have to use type assertion or narrow to a specific type if you want to perform operations on unknown.
:::

# Type Manipulation

## Generics

standard

```ts
function identity<Type>(arg: Type): Type {
  return arg;
}

let myIdentity: <Type>(arg: Type) => Type = identity;
```

interface

```ts
interface GenericIdentityFn<Type> {
  (arg: Type): Type;
}

function identity<Type>(arg: Type): Type {
  return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

class

```ts
class GenericNumber<NumType> {
  zeroValue: NumType;
  add: (x: NumType, y: NumType) => NumType;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
Try;
```

extends

```ts
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length); // Now we know it has a .length property, so no more error
  return arg;
}
```

type parameters in generic constraint

```ts
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a");
getProperty(x, "m");
Argument of type '"m"' is not assignable to parameter of type '"a" | "b" | "c" | "d"'.
```

using class types in generic

```ts
class BeeKeeper {
  hasMask: boolean = true;
}

class ZooKeeper {
  nametag: string = 'Mikle';
}

class Animal {
  numLegs: number = 4;
}

class Bee extends Animal {
  keeper: BeeKeeper = new BeeKeeper();
}

class Lion extends Animal {
  keeper: ZooKeeper = new ZooKeeper();
}

function createInstance<A extends Animal>(c: new () => A): A {
  return new c();
}

createInstance(Lion).keeper.nametag;
createInstance(Bee).keeper.hasMask;
```

## keyof type operator

```ts
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish;

type A = number;

type Mapish = { [k: string]: boolean };
type M = keyof Mapish;

type M = string | number;
```

## typeof type parameter

```ts
let s = 'hello';
let n: typeof s;

let n: string;
```

ReturnType

```ts
type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>;

function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;

type P = {
  x: number;
  y: number;
};
```

## indexed access types

```ts
const MyArray = [
  { name: 'Alice', age: 15 },
  { name: 'Bob', age: 23 },
  { name: 'Eve', age: 38 },
];

type Person = typeof MyArray[number];

type Person = {
  name: string;
  age: number;
};
type Age = typeof MyArray[number]['age'];

type Age = number;
// Or
type Age2 = Person['age'];

type Age2 = number;
```

```ts
const key = "age";
type Age = Person[key];
Type 'key' cannot be used as an index type.
'key' refers to a value, but is being used as a type here. Did you mean 'typeof key'?

type key = "age";
type Age = Person[key];
```

## conditional types

```ts
interface Animal {
  live(): void;
}
interface Dog extends Animal {
  woof(): void;
}

type Example1 = Dog extends Animal ? number : string;

type Example1 = number;

type Example2 = RegExp extends Animal ? number : string;

type Example2 = string;
```

infer

https://zenn.dev/brachio_takumi/articles/464106a6a80eca8ab919

```ts
type GetReturnType<Type> = Type extends (...args: never[]) => infer Return ? Return : never;

type Num = GetReturnType<() => number>;

type Num = number;

type Str = GetReturnType<(x: string) => string>;

type Str = string;

type Bools = GetReturnType<(a: boolean, b: boolean) => boolean[]>;

type Bools = boolean[];
```

## Maped Types

```ts
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type FeatureFlags = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<FeatureFlags>;
```

Mapping modifiers (+, -)

```ts
// Removes 'readonly' attributes from a type's properties
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property];
};

type LockedAccount = {
  readonly id: string;
  readonly name: string;
};

type UnlockedAccount = CreateMutable<LockedAccount>;

type UnlockedAccount = {
  id: string;
  name: string;
};
```

```ts
// Removes 'optional' attributes from a type's properties
type Concrete<Type> = {
  [Property in keyof Type]-?: Type[Property];
};

type MaybeUser = {
  id: string;
  name?: string;
  age?: number;
};

type User = Concrete<MaybeUser>;

type User = {
  id: string;
  name: string;
  age: number;
};
```

as

```ts
type Getters<Type> = {
  [Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property];
};

interface Person {
  name: string;
  age: number;
  location: string;
}

type LazyPerson = Getters<Person>;

type LazyPerson = {
  getName: () => string;
  getAge: () => number;
  getLocation: () => string;
};
```

```ts
type EventConfig<Events extends { kind: string }> = {
  [E in Events as E['kind']]: (event: E) => void;
};

type SquareEvent = { kind: 'square'; x: number; y: number };
type CircleEvent = { kind: 'circle'; radius: number };

type Config = EventConfig<SquareEvent | CircleEvent>;

type Config = {
  square: (event: SquareEvent) => void;
  circle: (event: CircleEvent) => void;
};
```

# Template Literal Types

```ts
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
type Lang = 'en' | 'ja' | 'pt';

type LocaleMessageIDs = `${Lang}_${AllLocaleIDs}`;

type LocaleMessageIDs =
  | 'en_welcome_email_id'
  | 'en_email_heading_id'
  | 'en_footer_title_id'
  | 'en_footer_sendoff_id'
  | 'ja_welcome_email_id'
  | 'ja_email_heading_id'
  | 'ja_footer_title_id'
  | 'ja_footer_sendoff_id'
  | 'pt_welcome_email_id'
  | 'pt_email_heading_id'
  | 'pt_footer_title_id'
  | 'pt_footer_sendoff_id';
```

```ts
type PropEventSource<Type> = {
    on(eventName: `${string & keyof Type}Changed`, callback: (newValue: any) => void): void;
};

/// Create a "watched object" with an 'on' method
/// so that you can watch for changes to properties.
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
Tr

const person = makeWatchedObject({
  firstName: "Saoirse",
  lastName: "Ronan",
  age: 26
});

person.on("firstNameChanged", () => {});

// Prevent easy human error (using the key instead of the event name)
person.on("firstName", () => {});
Argument of type '"firstName"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.

// It's typo-resistant
person.on("frstNameChanged", () => {});
Argument of type '"frstNameChanged"' is not assignable to parameter of type '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.
```

- `Uppercase<StringType>`
- `Lowercase<StringType>`
- `Capitalize<StringType>`
- `Uncapitalize<StringType>`

# Class

## methods

```ts
let x: number = 0;

class C {
  x: string = "hello";

  m() {
    // This is trying to modify 'x' from line 1, not the class property
    x = "world";
//Type 'string' is not assignable to type 'number'.
```

override methods

```ts
class Base {
  greet() {
    console.log('Hello, world!');
  }
}

class Derived extends Base {
  greet(name?: string) {
    if (name === undefined) {
      super.greet();
    } else {
      console.log(`Hello, ${name.toUpperCase()}`);
    }
  }
}

const d = new Derived();
d.greet();
d.greet('reader');
```

## getter/setter

```ts
class C {
  _length = 0;
  get length() {
    return this._length;
  }
  set length(value) {
    this._length = value;
  }
}
```

## index signature

```ts
class MyClass {
  [s: string]: boolean | ((s: string) => boolean);

  check(s: string) {
    return this[s] as boolean;
  }
}
```

Because the index signature type needs to also capture the types of methods, it’s not easy to usefully use these types. Generally it’s better to store indexed data in another place instead of on the class instance itself.

## Heritage

```ts
interface Pingable {
  ping(): void;
}

class Sonar implements Pingable {
  ping() {
    console.log('ping!');
  }
}

class Ball implements Pingable {
  // Class 'Ball' incorrectly implements interface 'Pingable'.
  // Property 'ping' is missing in type 'Ball' but required in type 'Pingable'.
  pong() {
    console.log('pong!');
  }
}
```

declare

```ts
interface Animal {
  dateOfBirth: any;
}

interface Dog extends Animal {
  breed: any;
}

class AnimalHouse {
  resident: Animal;
  constructor(animal: Animal) {
    this.resident = animal;
  }
}

class DogHouse extends AnimalHouse {
  // Does not emit JavaScript code,
  // only ensures the types are correct
  declare resident: Dog;
  constructor(dog: Dog) {
    super(dog);
  }
}
```

- public
- protected
  - protected members are only visible to subclasses of the class they’re declared in.
- private
  - doesn't allow access to the member even from subclasses

allow cross-instance private access

```ts
class A {
  private x = 10;

  public sameAs(other: A) {
    // No error
    return other.x === this.x;
  }
}
```

:::tip
Javascript private field

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields

---

WeakMaps

```ts
'use strict';
var _Dog_barkAmount;
class Dog {
  constructor() {
    _Dog_barkAmount.set(this, 0);
    this.personality = 'happy';
  }
}
_Dog_barkAmount = new WeakMap();
```

:::

## static block

```ts
class C {
  static foo: any;
  hoge: number;
  constructor() {
    this.hoge = 1;
    // C.foo = 0 は反映されない
  }

  static {
    C.foo = 0;
  }
}

// C.foo = 1

console.log(C.foo);
```

## Generic Clasa

```ts
class Box<Type> {
  contents: Type;
  constructor(value: Type) {
    this.contents = value;
  }
}

const b = new Box('hello!');
```

## this at runtime in class

**This はちょっと難しいので原文を見た方が良さそう**

```ts
class MyClass {
  name = 'MyClass';
  getName() {
    return this.name;
  }
}
const c = new MyClass();
const obj = {
  name: 'obj',
  getName: c.getName,
};

// Prints "obj", not "MyClass"
console.log(obj.getName());
```

### in case of arrow function

```ts
class MyClass {
  name = 'MyClass';
  getName = () => {
    return this.name;
  };
}
const c = new MyClass();
const g = c.getName;
// Prints "MyClass" instead of crashing
console.log(g());
```

## abstract 　 Classes and Members

```ts
abstract class Base {
  abstract getName(): string;

  printName() {
    console.log("Hello, " + this.getName());
  }
}

const b = new Base();
Cannot create an instance of an abstract class.
```

# Modules

~omit standard rules~

## type

```ts
// @filename: animal.ts
export type Cat = { breed: string; yearOfBirth: number };
'createCatName' cannot be used as a value because it was imported using 'import type'.
export type Dog = { breeds: string[]; yearOfBirth: number };
export const createCatName = () => "fluffy";

// @filename: valid.ts
import type { Cat, Dog } from "./animal.js";
export type Animals = Cat | Dog;

// @filename: app.ts
import type { createCatName } from "./animal.js";
const name = createCatName();
```

https://www.typescriptlang.org/docs/handbook/2/functions.html#function-overloads

# Browser cache 　ボキャブラリー

- Stale Content
- Fresh Content

- Cache Validation
- Cache Invalidation

- Caching Headers
  - expires
    - Mon, 13 Oct 2020....
    - この期限が切れるまでは、ブラウザは一度リソースを取得するとマシン内部にキャッシュする
  - pragma(only http1.0)
    - no-cache
  - content-control(can mix)
    - Private
    - Public
    - no-store
    - no-cache
    - max-age=3600, public
    - max-age=600, s-max-age=3600
    - must-revalidate
    - proxy-revalidate
- Validators
  - etag(entity tags)
    - check have been changed
    - strong/weak
  - if-none-match
  - last-modified
  - if-modified-since

# private/shared

- private
  - ローカル（クライアント）に格納可能
  - If a response contains personalized content and you want to store the response only in the private cache, you must specify a private directive.
- shared
  - ローカル、経路上、ゲートウェイ（CDN, Proxy）

# cache-control

`Cache-Control: no-store, no-cache, max-age=0, must-revalidate, proxy-revalidate`

- no-chache
  - オリジンに問い合わせを行い、そのキャッシュが有効でない限り使用しては行けない
- no-store
  - いかなる部分もキャッシュしては行けない

### 注意

- no-chache は毎回検証が必要なので max-age は共存できないが、max-age と must-revalidate（期限切れ時に検証を矯正する）は共存できる。
- immutable は変更できなくなるため慎重に扱う。仮に値を変更したい場合は URL を変えるしかなくなる
- proxy-revalidate は、private キャッシュに適用されない以外は must-revalidate と同様。
- no-transform は、Proxy などの中間でオブジェクトに対して操作を行うことを許容しない。例えば HTML などに対して通信量削減を目的とした圧縮を、Chrome のデータセーバーが行うなど

# 強いキャッシュ

- expires
  - Mon, 13 Oct 2020....
  - この期限が切れるまでは、ブラウザは一度リソースを取得するとマシン内部にキャッシュする

強制的にキャッシュされ続けるので、内容が全く変化されないコンテンツに利用するか、キャッシュ期間を短くする。

# 弱いキャッシュ

リソースの変更に応じて柔軟に変更できる。優先度はタグ>期間。

## 条件付き HTTP リクエスト

条件が合えば、304 Not Modified が返り余分な通信を避けられる

### 期間

1. 初めてデータを取得。レスポンスには`Last-Modified`がつける。
2. リクエストする際、ヘッダーに`if-modified-since`を指定。
3. サーバー側で URL リソースの最終更新日が指定を超えていればデータの返却。超えていなければ 304。

### タグ

1. 初めてデータを取得。レスポンスには`ETag`がつける。
2. リクエストする際、ヘッダーに`if-none-match`を指定。
3. サーバー側で URL リソースのタグが異なればデータの返却。同じなら 304。

## 有効期限

- Flash
  - 検証が成功してキャッシュが有効な状態
- Stale
  - 検証が無効な状態

Time to live の間必ず保存されるわけでもなく、キャッシュストレージの容量がいっぱいになれば押し出されるように削除される。一方で期限切れだからと言ってすぐに削除されるわけでもなく Stale の再利用に備えて保持する場合もある。

- max-age
  - キャッシュの期限
- s-max-age
  - 経路上のキャッシュのみ有効な max-age
- stale-while-revalidate
  - 期限切れの時にバックグラウンドで再検証を行う間に古いキャッシュを使うことを許容する期限
- stale-if-error
  - 期限切れ時の再検証でオリジンがダウンしていた場合に古いキャッシュを使うことを許容する期限
- expires
  - 期限切れを絶対時間で指定できる
  - 利用には max-age が優先され、システム的にも優先される

## 期間の指定をしない場合

デフォルトでキャッシュされるため、TTL がしていなくても条件に合えばキャッシュされる。
その場合は以下を利用して計算される。

- Date
- Last-Modified

`TTL = (Date - Last-Modified) / ブラウザ実装による定数(基本は10)`

10 日前から更新していないなら、1 日キャッシュしても大丈夫だろうという計算。
勝手にキャッシュされている場合は注意しましょう。

## キャッシュさせたくない場合

- private で経路上のキャッシュを避ける
- キャッシュへ保存させないように no-store を指定
- キャッシュを利用しない no-cache をつける
- オリジンがダウンしていた場合にキャッシュが使われないために must-revalidate をつける

`cache-control: private, no-store, no-cache, must-revalidate`

※no-store 以外にもいくつか指定するのは、Proxy や CDN 間の互換性の問題をなるべく軽減するため。（ブラウザのみなら no-store だけでも十分かも？）

Last-Modified ヘッダを消すことで TTL 未指定の動きも排除できる。
キャッシュを防ぐために max-age=0 を設定するケースもありますが、他の項目が解釈されなかった場合はキャッシュが作られるので注意が必要。

## さまざまなヘッダ

- accept-encoding: gzip, deflate, br
  - コンテンツの圧縮
  - サーバーとクライアントが直接やりとりするだけであれば気にする必要はない。CDN などの場合は vary を利用する
- vary
  - どの値をもとにキャッシュがバリエーションを持つかの指定をするヘッダ
  - vary: Accept-Encoding
    - リクエストヘッダに含まれる acccept-encoding の値をセカンダリキーとして利用する
  - 複数指定可能、vary: Accept_Encoding, User-Agent
- content-type
  - 圧縮対象の指定の方法はミドルウェアによっても異なり、複数指定方法がありますが、個別の MIME タイプを指定している場合圧縮対象から漏れることがあるので注意





## cache-status

https://postd.cc/status-targeted-caching-headers/

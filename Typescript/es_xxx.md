- [ES1 ECMAScript 1 (1997) First edition](#es1-ecmascript-1-1997-first-edition)
- [ES2 ECMAScript 2 (1998) Editorial changes](#es2-ecmascript-2-1998-editorial-changes)
- [ES3 ECMAScript 3 (1999) Added regular expressions](#es3-ecmascript-3-1999-added-regular-expressions)
  - [Added try/catch](#added-trycatch)
  - [Added switch](#added-switch)
  - [Added do##while](#added-dowhile)
- [ES4 ECMAScript 4 Never released](#es4-ecmascript-4-never-released)
- [ES5 ECMAScript 5 (2009)](#es5-ecmascript-5-2009)
  - [Added "strict mode"](#added-strict-mode)
  - [Added JSON support](#added-json-support)
  - [Added String.trim()](#added-stringtrim)
  - [Added Array.isArray()](#added-arrayisarray)
  - [Added Array iteration methods](#added-array-iteration-methods)
  - [Allows trailing commas for object literals](#allows-trailing-commas-for-object-literals)
- [ES6 ECMAScript 2015](#es6-ecmascript-2015)
  - [参照](#参照)
  - [Added let and const](#added-let-and-const)
  - [Added default parameter values](#added-default-parameter-values)
  - [Added Array.find()](#added-arrayfind)
  - [Added Array.findIndex()](#added-arrayfindindex)
- [ECMAScript 2016](#ecmascript-2016)
  - [Added exponential operator (\*\*)](#added-exponential-operator-)
  - [Added Array.includes()](#added-arrayincludes)
- [ECMAScript 2017](#ecmascript-2017)
  - [Added string padding](#added-string-padding)
  - [Added Object.entries(), Added Object.values()](#added-objectentries-added-objectvalues)
  - [Added async functions](#added-async-functions)
  - [Added shared memory](#added-shared-memory)
    - [Atomics](#atomics)
- [ECMAScript 2018](#ecmascript-2018)
  - [Added rest / spread properties](#added-rest--spread-properties)
  - [Added asynchronous iteration](#added-asynchronous-iteration)
  - [Added Promise.finally()](#added-promisefinally)
  - [Additions to RegExp](#additions-to-regexp)
- [ECMAScript 2019](#ecmascript-2019)
  - [Added Optional catch binding](#added-optional-catch-binding)
  - [Added Symbol.description](#added-symboldescription)
  - [Function.toString revision](#functiontostring-revision)
  - [Object.fromEntries](#objectfromentries)
  - [String.trimStart/String.trimEnd](#stringtrimstartstringtrimend)
  - [Array.flat/Array.flatMap](#arrayflatarrayflatmap)
  - [Well-formed JSON.stringify](#well-formed-jsonstringify)
  - [Added JSON superset](#added-json-superset)
- [ECMAScript 2020](#ecmascript-2020)
  - [String.protype.matchAll](#stringprotypematchall)
  - [Dynamic Import](#dynamic-import)
  - [BigInt — Arbitrary Precision Integers](#bigint--arbitrary-precision-integers)
  - [Promise.allSettled](#promiseallsettled)
  - [Standardized globalThis object](#standardized-globalthis-object)
  - [For-in Mechanics](#for-in-mechanics)
  - [Nullish Coalescing Operator](#nullish-coalescing-operator)
  - [Optional Chaining](#optional-chaining)
- [ECMAScript 2021](#ecmascript-2021)
  - [String.protype.replaceAll](#stringprotypereplaceall)
  - [Promise.any](#promiseany)
  - [WeakRef](#weakref)
  - [Logical Assignment Operators](#logical-assignment-operators)
  - [Numeric separators](#numeric-separators)
- [ECMAScript 2022](#ecmascript-2022)
  - [参照](#参照-1)
  - [Array, String, TypedArray の .at()](#array-string-typedarray-の-at)
  - [Top-Level Await](#top-level-await)
  - [クラスフィールド宣言と static](#クラスフィールド宣言と-static)
  - [プライベートのインスタンスフィールド、メソッド](#プライベートのインスタンスフィールドメソッド)
  - [クラスの静的イニシャライザーブロック](#クラスの静的イニシャライザーブロック)
  - [hasOwnProperty の代わりの Object.hasOwn()](#hasownproperty-の代わりの-objecthasown)
  - [エラーをチェインできる Error.cause](#エラーをチェインできる-errorcause)
  - [正規表現の d フラグ](#正規表現の-d-フラグ)
  - [instanceof の代わりの in](#instanceof-の代わりの-in)
  - [配列の最後の要素を取得できる at()](#配列の最後の要素を取得できる-at)

# ES1 ECMAScript 1 (1997) First edition

# ES2 ECMAScript 2 (1998) Editorial changes

# ES3 ECMAScript 3 (1999) Added regular expressions

## Added try/catch

## Added switch

## Added do##while

# ES4 ECMAScript 4 Never released

# ES5 ECMAScript 5 (2009)

## Added "strict mode"

## Added JSON support

## Added String.trim()

## Added Array.isArray()

## Added Array iteration methods

## Allows trailing commas for object literals

# ES6 ECMAScript 2015

## 参照

https://qiita.com/soarflat/items/b251caf9cb59b72beb9b

## Added let and const

## Added default parameter values

## Added Array.find()

## Added Array.findIndex()

# ECMAScript 2016

## Added exponential operator (\*\*)

```js
// before
Math.pow(2, 3); //8
Math.pow(3, 3); //27

// after
2 ** 3; //8
3 ** 3; //27
```

## Added Array.includes()

```js
let array = [1, 2, 3, 4, 5];

array.includes(2); //true
array.includes(6); //false

array.includes(1, 0); //true
array.includes(4, 3); //true

array.includes(1, 3); //false

array.includes(1, -1); //false
array.includes(3, -4); //true
```

# ECMAScript 2017

## Added string padding

## Added Object.entries(), Added Object.values()

```js
let drone = {
  x: 1,
  y: 2,
  z: 3,
  name: 'drone1',
};

Object.values(drone); //[1, 2, 3, "drone1"]
Object.entries(drone);
//["x", 1]
// ["y", 2]
// ["z", 3]
// ["name", "drone1"]
```

## Added async functions

## Added shared memory

### Atomics

https://qiita.com/yosuke_furukawa/items/923f8d40c451713dfbd3

アトミック操作(不可分操作)とはある処理(仮に処理 A と呼称)が次に行う処理(処理 B)が開始される前に処理が終了し、 処理 A が中断されないようにすること。

Atomics.add(typedArray, index, values):配列内の指定したインデックスに値を追加して、追加する前の値を返します。このアトミック操作は修正された値が書き戻されるまで、他の書き込みが起こらないことを保証します。

- typedArray:共有された整数の TypedArray。Int8Array、Uint8Array、Int16Array、Uint16Array、Int32Array、Uint32Array のいずれか。
- Atomic.sub(typedArray, index, values):配列内の指定したインデックスに値を取り除き、取り除く前の値を返します。
- Atomic.load(typedArray, index):配列中の指定した位置(インデックス)の値を返します。
- Atomics.store(typedArray, index, value):指定した位置に指定した値を保存して、その値を返します。

```js
const buffer = new SharedArrayBuffer(16);
const uint8 = new Uint8Array(buffer);
uint8[0] = 3;

//Atomic.add()
console.log(Atomics.add(uint8, 0, 2)); //3

console.log(Atomics.add(uint8, 0, 1)); //5

//Atomic.load()
console.log(Atomics.load(uint8, 0));
```

# ECMAScript 2018

## Added rest / spread properties

rest, spread が obj にも使えるようになった

spread は`...`

rest は下記

```js
function sum(...theArgs) {
  let total = 0;
  for (const arg of theArgs) {
    total += arg;
  }
  return total;
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10
```

## Added asynchronous iteration

## Added Promise.finally()

## Additions to RegExp

# ECMAScript 2019

## Added Optional catch binding

```js
try {
  throw new Error('🙅');
} catch {
  // (error)の記載はしなくても良い
  console.warn('エラーをキャッチしました');
}
```

## Added Symbol.description

```js
Symbol('矢部').description;
// 結果："矢部"
```

## Function.toString revision

```js
function /* こんにちは */ myFunction() {}

myFunction.toString();
// 結果："function /* こんにちは */ myFunction  () {}"
// コメントや文字列が保持される
```

## Object.fromEntries

Symbol の説明（description）を返すプロパティーです。説明とは、デバッグ用に Symbol を区別するためのものです。

```js
Object.fromEntries([
  ['id', 16],
  ['name', '鈴木'],
]);
// 結果：{id: 16, name: "鈴木"}
```

## String.trimStart/String.trimEnd

文字列の先頭または末尾の空白を除去するメソッド

```js
'  寿司ざんまい　'.trim();
// 結果："寿司ざんまい"
```

## Array.flat/Array.flatMap

```js
[[1, 2], 3, 4].flat();
// 結果：[1, 2, 3, 4]
```

```js
const MyData = [
  {
    name: '菅原さん',
    favorite: ['大豆', 'プロテイン', 'つけ麺'],
  },
  {
    name: '山根さん',
    favorite: ['プロテイン', 'ビタミンC'],
  },
  {
    name: '鹿野さん',
    favorite: ['ラーメン', 'つけ麺', '軟骨の唐揚げ'],
  },
];

MyData.flatMap((data) => data.favorite);
// 結果：["大豆", "プロテイン", "つけ麺", /*中略*/ "軟骨の唐揚げ"]
```

## Well-formed JSON.stringify

## Added JSON superset

# ECMAScript 2020

## String.protype.matchAll

```js
const string = 'ldashdhewdh';
const regex = '.......';

for (const match of string.matchAll(regex)) {
  console.log(match);
}
```

## Dynamic Import

```js
import(hogeModule).then((module) => {
  module.doSomething();
});

const module = await import(hogeModule);
module.doSomething();
```

## BigInt — Arbitrary Precision Integers

## Promise.allSettled

promise that’s fulfilled with an array of promise state snapshots

```js
const p1 = new Promise((resolve) => resoleve('hoge'));
const p2 = new Promise((resolve) => resoleve('hoge2'));

Promise.all([p1, p2]).then((res) => console.log(res));
// => 'hoge'
// => 'hoge2'
Promise.allSettled([p1, p2]).then((res) => console.log(res));
// => {status: fulfilled, value: 'hoge'}
// => {status: fulfilled, value: 'hoge2'}
```

## Standardized globalThis object

## For-in Mechanics

`for(a in b)`

## Nullish Coalescing Operator

`const a = value || 'nothing value'`

## Optional Chaining

`a?.errror?.val`

# ECMAScript 2021

## String.protype.replaceAll

before

```js
const fruits = '🍎+🍐+🍓+';
const fruitsWithBanana = fruits.replace(/\+/g, '🍌');
console.log(fruitsWithBanana); //🍎🍌🍐🍌🍓🍌
```

after

```js
const fruits = '🍎+🍐+🍓+';
const fruitsWithBanana = fruits.replaceAll('+', '🍌');
console.log(fruitsWithBanana); //🍎🍌🍐🍌🍓🍌
```

## Promise.any

どれか 1 個でも resolve されれば OK「Promise.any」

```js
const SlowlyDone = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'Done slowly');
}); //resolves after 500ms

const QuicklyDone = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'Done quickly');
}); //resolves after 100ms

const Rejection = new Promise((resolve, reject) => {
  setTimeout(reject, 100, 'Rejected'); //always rejected
});

Promise.any([SlowlyDone, QuicklyDone, Rejection])
  .then((value) => {
    console.log(value);
    //  QuicklyDone fulfils first
  })
  .catch((err) => {
    console.log(err);
  });

//expected output: Done quickly
```

## WeakRef

`const weakElement = new WeakRef(element);`

element というオブジェクトを参照をしているのですが、これが WeakRef 以外で参照されることがなくなった場合。
例えば WeakRef でオブジェクトをなにか参照していて、そのオブジェクトがこの WeakRef 以外で参照されなくなった場合、たとえ WeakRef が参照していても、ガベージコレクションによってこのオブジェクトが消されてしまう

**FinalizationRegistry**
先ほど、ガベージコレクションというものが出てきたのですが、FinalizationRegistry というのは、オブジェクトがガベージコレクションされたことをトリガーにして、起動するコールバック関数を提供する機能です。

```js
function toogle(element) {
  const weakElement = new WeakRef(element);
  let intervalId = null;

  function toggle() {
    const el = weakElement.deref();
    if (!el) {
      return clearInterval(intervalId);
    }
    const decoration = weakElement.style.textDecoration;
    const style = decoration === 'none' ? 'underline' : 'none';
    decoration = style;
  }
  intervalId = setInterval(toggle, 1000);
}
const element = document.getElementById('link');
toogle(element);
setTimeout(() => element.remove(), 10000);
```

## Logical Assignment Operators

```js
a ||= b;
// Equivalent to:
a || (a = b);

// Or Or Equals
|   a   |   b   | a ||= b | a (after operation) |
| true  | true  |   true  |        true         |
| true  | false |   true  |        true         |
| false | true  |   true  |        true         |
| false | false |   false |        false        |



a &&= b;
// Equivalent to:
a && (a = b);

// And And Equals
|   a   |   b   | a &&= b | a (after operation) |
| true  | true  |   true  |        true         |
| true  | false |   false |        false        |
| false | true  |   false |        false        |
| false | false |   false |        false        |
```

## Numeric separators

```js
1_000_000_000; // A billion
101_475_938.38; // Hundreds of millions
const amount = 12345_00; // 12,345 (1234500 cents, apparently)
const amount = 123_4500; // 123.45 (4-fixed financial)
const amount = 1_234_500; // 1,234,500
0.000_001; // 1 millionth
1e10_000; // 10^10000 -- granted, far less useful / in-range...

const binary_literals = 0b1010_0001_1000_0101;
const hex_literals = 0xa0_b0_c0;
const bigInt_literals = 1_000_000_000_000n;
const octal_literal = 0o1234_5670;
```

# ECMAScript 2022

## 参照

https://ics.media/entry/220610/

## Array, String, TypedArray の .at()

```js
const array = ['a', 'b', 'c'];

console.log(array[1]); // "b"

// ES2022
console.log(array.at(1)); // "b"

----

const array = ['a', 'b', 'c', 'd'];

// 従来の配列末尾からの参照
console.log(array[array.length - 1]); // "d"

// ES2022で可能になる書き方
console.log(array.at(-1)); // "d"
```

## Top-Level Await

トップレベルでしかできないので注意

```html
// 従来のコード
<script>
  const start = async () => {
    // JSONデータを読み込む
    const data = await fetch('./example/data.json');
    const object = await data.json();
  };
  start();
</script>

//ES2022
<script type="module">
  // ES2022で可能になった書き方
  const data = await fetch('./example/data.json');
  const object = await data.json();
  console.log(object);
</script>
```

## クラスフィールド宣言と static

before

```js
class Human {
  constructor() {
    this.age = 18;
  }
}

const human = new Human();
console.log(human.age); // 18
```

after

```js
class Human {
  age = 18;
  static category = 'animal';
}
```

```js
class Human {
  static category = 'animal';
}

console.log(Human.category); // animal
```

---

## プライベートのインスタンスフィールド、メソッド

```js
class MyCounter {
  #count;

  constructor(count) {
    this.#count = count;
  }

  #calc() {
    return this.#count * 10;
  }

  say() {
    // プライベート変数にアクセスできるのはクラスの内部だけ
    console.log(this.#calc());
  }
}

const object = new MyCounter(3);
object.say(); // 30

console.log(object.#count); // ❎️ シンタックスエラー
console.log(object.#calc()); // ❎️ シンタックスエラー
```

- TypeScript では、private は#へ変換されません。
- private アクセス修飾子はターゲットにかかわらず互換コードへコンパイルされます（取り除かれた形になります）。
- #は ES2015〜SES2021 ターゲットだと、互換コードへコンパイルされます（ES5 以下にはコンパイルできません）。
- #は ES2022 ターゲットだと、そのまま出力されます。

## クラスの静的イニシャライザーブロック

```js
class MyClass {
  static #myProperty;

  // 静的イニシャライザーブロック
  static {
    // 外部からデータととってくるとか、環境変数から加工するとか、複雑な処理等
    // 以下はダミーの処理
    const json = JSON.parse(`{"someField": "hoge"}`);
    this.#myProperty = json.someField;
  }
  constructor() {
    console.log(MyClass.#myProperty);
  }
}
```

## hasOwnProperty の代わりの Object.hasOwn()

before

```js
const example = {
  property: 'あいう',
};

console.log(Object.prototype.hasOwnProperty.call(example, 'property'));
console.log(example.hasOwnProperty('property')); // この書き方は動作するが注意が必要
```

after

```js
const example = {
  property: 'あいう',
};

console.log(Object.hasOwn(example, 'property')); // true
```

## エラーをチェインできる Error.cause

```js
throw new Error('失敗', { cause: error }); // error は元となるエラーオブジェクト
```

```js
async function start() {
  try {
    await load();
  } catch (error) {
    console.log(error);
    // console.log(error.cause); // さらに奥の情報を追跡できる
  }
}

async function load() {
  try {
    const result = await loadJson();
    console.log(result);
  } catch (error) {
    throw new Error('読み込みに失敗！', { cause: error });
  }
}

async function loadJson() {
  let data;
  try {
    data = await fetch('example.json');
  } catch (error) {
    // ネットワークがオフラインの場合
    throw new Error('fetchに失敗しました。', { cause: error });
  }

  if (data.ok === false) {
    // 404 の場合等（この場合は、明示的なエラーなのでcause未指定）
    throw new Error('ファイルの読み込みに失敗しました。');
  }

  let json;
  try {
    json = await data.json();
    return json;
  } catch (error) {
    // JSONのパースに失敗
    throw new Error('JSONデータの展開に失敗しました。', { cause: error });
  }
}
```

## 正規表現の d フラグ

正規表現にマッチインデックス機能（/d フラグ）が追加されます。このオプションを指定すると、ひっかかった部分文字列の先頭と末尾のインデックスについての追加情報が得られます。

```js
const text = "今日の夕食代：500円";
const regexp = /今日の夕食代：(?<digit>\d{3})円/dg;

for (const match of text.matchAll(regexp)) {
  console.log(match);
}

=>

[
  '今日の夕食代：500円',
  '500',
  index: 0,
  input: "今日の夕食代：500円",
  groups: { digit: '500' },
  indices: {
    [ 0, 11 ],
    [ 7, 10 ],
    groups: {
      digit: [7, 10]
    }
  }
]
```

## instanceof の代わりの in

## 配列の最後の要素を取得できる at()

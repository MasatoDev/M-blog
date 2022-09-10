# Conditional Type

`T extends number`を三項演算子の条件のような形で判定する

```ts
type IsNumber<T> = T extends number ? true : false;

type T1 = IsNumber<10>;       //type T1 = true
type T2 = IsNumber<"テスト">;  //type T2 = false
type T3 = IsNumber<number>;   //type T3 = true
```

## Union distribution

```ts
(T1 | T2 | T3) extends U ? X : Y

// result of above code
(T1 extends U ? X : Y) | (T2 extends U ? X : Y) | (T3 extends U ? X : Y)
```

抽出できる

```ts
type JobGrade = "S" | "A" | "B";

type SuperJobGrade = MyExtract<JobGrade, "S" | "A">; //type SuperJobGrade = "S" | "A"
```

# infer


```ts
//ユーティリティ型のReturnTypeを、MyReturnTypeとして定義してみる
type MyReturnType<T> = T extends (...args:any[])=> infer R ? R : never
```


```ts
// () => number　の返り値の型numberを抽出
type Num = MyReturnType<() => number>;           //type Num = number

// (x: string) => string の返り値の型stringを抽出
type Str = MyReturnType<(x: string) => string>;  //type Str = string

// (a: boolean, b: boolean) => boolean[]　の返り値の型boolean[]を抽出
type Bools = MyReturnType<(a: boolean, b: boolean) => boolean[]>;  //type Bools = boolean[]
```

# Never

neverには何も代入できず、neverは何にでも代入できる。

どんな時？

```ts

function throwError(): never {
  throw new Error();
}

function forever(): never {
  while (true) {} // 無限ループ
}

// 作り得ない値もnever型になります。たとえば、数値型と文字列型の両方に代入可能な値は作れません。
type NumberString = number & string;

```

neverを使った網羅性チェック。stringが残っていればneverには代入できないためエラーとなる。

```ts
class ExhaustiveError extends Error {
  constructor(value: never, message = `Unsupported type: ${value}`) {
    super(message);
  }
}

function printLang(ext: Extension): void {
  switch (ext) {
    case "js":
      console.log("JavaScript");
      break;
    case "ts":
      console.log("TypeScript");
      break;
    default:
      throw new ExhaustiveError(ext);
// Argument of type 'string' is not assignable to parameter of type 'never'.
  }
}
```

```js
class ExhaustiveError extends Error {
    constructor(value, message = `Unsupported type: ${value}`) {
        super(message);
    }
}
function func(value) {
    switch (value) {
        case "yes":
            console.log("YES");
            break;
        case "no":
            console.log("NO");
            break;
        default:
            throw new ExhaustiveError(value);
    }
}
```

# remove readonly and optional

```ts
type MutableRequired<T> = { -readonly [P in keyof T]-?: T[P] }; // Remove readonly and ?
type ReadonlyPartial<T> = { +readonly [P in keyof T]+?: T[P] }; // Add readonly and ?
```

```ts
type A = {
    readonly 'a'?: number,
    'b': number,
    'c': number,
}

const aa: A = { 'b': 2, 'c': 2 }

type B<T> = { -readonly [K in keyof T]-?: T[K] }

const bb: B<A> = { 'a': 1, 'b': 2, 'c': 2 }
```

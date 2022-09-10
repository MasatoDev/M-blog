# Decorator Evaluation

There is a well defined order to how decorators applied to various declarations inside of a class are applied:

1. Parameter Decorators, followed by Method, Accessor, or Property Decorators are applied for each instance member.
2. Parameter Decorators, followed by Method, Accessor, or Property Decorators are applied for each static member.
3. Parameter Decorators are applied for the constructor.
4. Class Decorators are applied for the class.

## Class decorator

```ts
function reportableClassDecorator<T extends { new (...args: any[]): {} }>(constructor: T) {
  return class extends constructor {
    reportingURL = "http://www...";
  };
}

@reportableClassDecorator
class BugReport {
  type = "report";
  title: string;

  constructor(t: string) {
    this.title = t;
  }
}

const bug = new BugReport("Needs dark mode");
console.log(bug.title); // Prints "Needs dark mode"
console.log(bug.type); // Prints "report"

// Note that the decorator _does not_ change the TypeScript type
// and so the new property `reportingURL` is not known
// to the type system:
bug.reportingURL;
Property 'reportingURL' does not exist on type 'BugReport'.
```

# Declaration Merging

## interface

```ts
interface Document {
  createElement(tagName: any): Element;
}
interface Document {
  createElement(tagName: 'div'): HTMLDivElement;
  createElement(tagName: 'span'): HTMLSpanElement;
}
interface Document {
  createElement(tagName: string): HTMLElement;
  createElement(tagName: 'canvas'): HTMLCanvasElement;
}

interface Document {
  createElement(tagName: 'canvas'): HTMLCanvasElement;
  createElement(tagName: 'div'): HTMLDivElement;
  createElement(tagName: 'span'): HTMLSpanElement;
  createElement(tagName: string): HTMLElement;
  createElement(tagName: any): Element;
}
```

## namespace

```ts
namespace Animal {
  let haveMuscles = true; // not export member
  export function animalsHaveMuscles() {
    return haveMuscles;
  }
}

namespace Animal {
  export function doAnimalsHaveMuscles() {
    return haveMuscles; // Error, because haveMuscles is not accessible here
  }
}
```

## namespace with class

You can also use namespaces to add more static members to an existing class.

```ts
class Album {
  label: Album.AlbumLabel;
}
namespace Album {
  export class AlbumLabel {}
}
```

```ts
function buildLabel(name: string): string {
  return buildLabel.prefix + name + buildLabel.suffix;
}
namespace buildLabel {
  export let suffix = '';
  export let prefix = 'Hello, ';
}
console.log(buildLabel('Sam Smith'));
```

```ts
enum Color {
  red = 1,
  green = 2,
  blue = 4,
}
namespace Color {
  export function mixColor(colorName: string) {
    if (colorName == 'yellow') {
      return Color.red + Color.green;
    } else if (colorName == 'white') {
      return Color.red + Color.green + Color.blue;
    } else if (colorName == 'magenta') {
      return Color.red + Color.blue;
    } else if (colorName == 'cyan') {
      return Color.green + Color.blue;
    }
  }
}
```

## Object vs Enum

```ts
const enum EDirection {
  Up,
  Down,
  Left,
  Right,
}

const ODirection = {
  Up: 0,
  Down: 1,
  Left: 2,
  Right: 3,
} as const;

EDirection.Up;

(enum member) EDirection.Up = 0

ODirection.Up;

(property) Up: 0

// Using the enum as a parameter
function walk(dir: EDirection) {}

// It requires an extra line to pull out the values
type Direction = typeof ODirection[keyof typeof ODirection];
function run(dir: Direction) {}

walk(EDirection.Left);
run(ODirection.Right);
```

# Symbol

https://note.affi-sapo-sv.com/js-symbol.php

```ts
const name = Symbol();
const log = Symbol();

const obj = {
  name: '太郎', // nameという名前のプロパティ
  [name]: '花子', // シンボル値nameのプロパティ

  log: function () {
    console.log(this.name);
  },
  [log]: function () {
    console.log(this[name]);
  },
};

console.log(obj.name); // 結果: "太郎"
console.log(obj[name]); // 結果: "花子"
```

シンボルの特性上、「プロパティ名の重複を避ける」ことと、「外部からアクセスできないプロパティを作成する」こと、「定数としての利用」の 3 点が考えられます。

下記のように列挙できない。

```ts
const data2 = Symbol();

const obj = {
  data1: 'abc',
  [data2]: 'efg',
};

Object.keys(obj).forEach((e) => console.log(e)); // data1 のみ出力

for (let e in obj) console.log(e); // data1 のみ出力
```

# Iterators and Generators

- for.. in ..
  - key
- for.. of ..
  - value

```ts
let list = [4, 5, 6];
for (let i in list) {
  console.log(i); // "0", "1", "2",
}
for (let i of list) {
  console.log(i); // 4, 5, 6
}

let pets = new Set(['Cat', 'Dog', 'Hamster']);
pets['species'] = 'mammals';
for (let pet in pets) {
  console.log(pet); // "species"
}
for (let pet of pets) {
  console.log(pet); // "Cat", "Dog", "Hamster"
}
```

# JSX

```ts
interface PropsType {
  children: JSX.Element
  name: string
}
class Component extends React.Component<PropsType, {}> {
  render() {
    return (
      <h2>
        {this.props.children}
      </h2>
    )
  }
}
// OK
<Component name="foo">
  <h1>Hello World</h1>
</Component>
// Error: children is of type JSX.Element not array of JSX.Element
<Component name="bar">
  <h1>Hello World</h1>
  <h2>Hello World</h2>
</Component>
// Error: children is of type JSX.Element not array of JSX.Element or string.
<Component name="baz">
  <h1>Hello</h1>
  World
</Component>
```

# Mixins

より単純な部分クラスを組み合わせることによってクラスを構築することです。Scala のような言語では mixin や trait という考え方に馴染みがあるかもしれませんし、このパターンは JavaScript コミュニティでもある程度の人気を博しています。

```ts
// To get started, we need a type which we'll use to extend
// other classes from. The main responsibility is to declare
// that the type being passed in is a class.

type Constructor = new (...args: any[]) => {};

// This mixin adds a scale property, with getters and setters
// for changing it with an encapsulated private property:

function Scale<TBase extends Constructor>(Base: TBase) {
  return class Scaling extends Base {
    // Mixins may not declare private/protected properties
    // however, you can use ES2020 private fields
    _scale = 1;

    setScale(scale: number) {
      this._scale = scale;
    }

    get scale(): number {
      return this._scale;
    }
  };
}
// Compose a new class from the Sprite class,
// with the Mixin Scale applier:
const EightBitSprite = Scale(Sprite);

const flappySprite = new EightBitSprite('Bird');
flappySprite.setScale(0.8);
console.log(flappySprite.scale);
```

# namespace and modules

# Triple-Slash Directives

# Type compatibility

```ts
interface Pet {
  name: string;
}
class Dog {
  name: string;
}
let pet: Pet;
// OK, because of structural typing
pet = new Dog();
```

```ts
interface Pet {
  name: string;
}
let dog = { name: 'Lassie', owner: 'Rudd Weatherwax' };
function greet(pet: Pet) {
  console.log('Hello, ' + pet.name);
}
greet(dog); // OK
```

```ts
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;
y = x; // OK
x = y; // Error
```

```ts
class Animal {
  feet: number;
  constructor(name: string, numFeet: number) {}
}
class Size {
  feet: number;
  constructor(numFeet: number) {}
}
let a: Animal;
let s: Size;
a = s; // OK
s = a; // OK
```

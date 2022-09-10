「Web Accessibility Initiative - Accessible Rich Internet Applications」

# WAI-ARIA とは

WAI-ARIA をつかえば、HTML だけでは不足しているセマンティクス（意味）を属性で補完することができ、支援技術（スクリーンリーダーなど）を通じて、障害をもつ人に対し適切な情報を伝えられるようになります。
あくまでも「必要な場合のみ使用する」技術です。HTML5 タグのセマンティクスで解決できるのであれば、HTML で対応するのが基本です。

https://www.w3.org/TR/wai-aria-practices/
https://www.w3.org/TR/wai-aria-implementation/

いろいろな要素があるが、主に下記の 2 つ

- これは見出しなのか、セクションなのかなどコンテンツの役割を表すための role 属性
  - https://www.w3.org/TR/wai-aria-1.0/roles#roles_categorization
- 表示されている／隠れているなどコンテンツの状態や、どこから参照されるのかなど性質を表すための aria 属性
  - 状態明示に関する aria は、多くの属性で、属性値に true か false を指定します。
  - 性質明示に関する aria は、属性値に参照先の ID や数値または真偽値を指定します。

# CSS に活用

```html
<a href="/admin.html" class="Link" aria-diabled="true">管理者専用リンク</a>
```

```css
.Link[aria-diabled='true'] {
  cursor: not-allowed;
  ...;
}
```

# WAI-ARIA を利用すべきタイミング

- **道しるべ/ランドマーク（Signpost/Landmark）:** ARIA の role 属性の値は、HTML 要素の意味論（例えば <nav>）を再現するランドマークとして振る舞ったり、 search 、 tabgroup 、 tab 、 listbox のように HTML5 の意味論の範囲外となる道しるべ（signpost）を異なる機能エリアに提供することができます。

- **動的なコンテンツの更新:** スクリーンリーダーは、絶えず更新されるコンテンツが得意ではない傾向があります。 ARIA の aria-live を使うことで、 XMLHttpRequest や DOM API を通してコンテンツが更新された場合に、スクリーンリーダーのユーザーに対してそれを伝えることができます。

- **キーボードのアクセシビリティの向上:** キーボードのアクセシビリティを最初から持つ HTML 要素がありますが、 JavaScript を使ってそれ以外の要素に同じようなインタラクションをさせる場合、スクリーンリーダーにとって困難が生じます。 こうしなければならない場合、 WAI-ARIA は他の要素に対してフォーカスを得る手段を提供しています（tabindex の使用）。

- **意味論的ではないコントロールのアクセシビリティ:** ネストした一連の <div> が CSS/JavaScript と共に複雑な UI 機能を構成していたり、ネイティブのコントロールが JavaScript によって大きく強化/変更されている場合、アクセシビリティの提供は困難になります。 そこに意味論や手がかりが無ければ、スクリーンリーダーのユーザーはその機能が何をするのか判断するのが難しくなるでしょう。 このような状況では、 button 、 listbox 、または tabgroup といったロールの組み合わせ、もしくは aria-required や aria-posinset などのプロパティにより機能の手がかりを提供することで、 ARIA は足りないものを補うことができます。

# 参照

[これみればロールとそれ以外の主要な wai-aria の属性が分かり、音声で流してくれる](https://www.codegrid.net/articles/2016-wai-aria-1)

[mdn web docs には全てが書いてある](https://developer.mozilla.org/ja/docs/Learn/Accessibility/WAI-ARIA_basics)

[`startTransition` をいつ使うべきか](https://github.com/reactwg/react-18/discussions/94#discussioncomment-1406166)

- 初めてそのページを訪れるユーザーに対しては、スピナーでデータを取得していることを明示しても良い
- ２度目の訪問でスピナーが出ると混乱させるため、`startTransition`によって、遷移できる状態になるまで前のページで待機するのが良さそう

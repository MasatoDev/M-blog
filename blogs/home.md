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

## 目次
- 概要
  - クライアント - サーバモデル
  - Webを表示する仕組み
- URI
  - URI概要
  - URIの構成
  - URIの設計で留意する点
- HTTP
  - HTTP概要
  - HTTPメッセージ
    - リクエストメッセージ
    - レスポンスメッセージ
  - HTTPメソッド
  - ステータスコード
- 実例：Chromeの開発者ツールを使用したHTTPメッセージの確認
- 参考にした書籍やwebページ

## 内容 
### 概要
#### クライアント - サーバモデル
- Webは、クライアントとサーバが通信するクライアント - サーバモデルを採用している。
- たくさんのクライアントに対して少数のサーバが対応している。
- クライアントは、コンピューターネットワーク上で必要なサーバを探して、作業を依頼する。
- サーバは、複数のクライアントの依頼を受け付けて処理を行う。
![スクリーンショット 2021-09-18 1.19.48.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/ac894620-7790-44f9-a9f9-139463c5b1f1.png)


#### Webを表示する仕組み
- Webページを表示したい時は、クライアントであるWebブラウザなどから、サーバにリクエストが送られる。（リクエストを送るときに、どのWebサーバのどのページを表示したいかは、URIで指定する。）
- サーバからのレスポンスをクライアントが受け取ることで、Webページを表示することができる。（レスポンスと一緒に送られるWebページはHTMLフォーマットで記述される。）
- リクエストを送ってレスポンスを受け取るという通信手順は、HTTPとして標準化されている。
![スクリーンショット 2021-09-18 1.20.00.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/a9ad8c74-f57e-4604-9cd3-1ad3ac086cce.png)


### URI
#### URI概要
- URIとは、Uniform Resorce Identifierの略で、「リソースを統一的に識別するID」のことである。URIを使うと、Web上に存在するすべてのリソースを一意に示すことができる。つまり、URIがあれば、すべてのリソースに簡単にアクセスすることができる。

#### URIの構成
- スキーム：通信手順を指定する
- ドメイン名：ネット上のサーバを指定する
- ポート番号：Webサーバのリソースにアクセスする技術的な出入口
- パス：Webサーバ内のリソースの位置を階層的に記述する
- クエリ：Webサーバに提供する追加パラメータ
- フラグメント：リソース自体の特定の場所へのアンカー

※なお、URIは、URL（Uniform Resource Locator, リソースの場所を示すもの）とURN（Uniform Resource Name, リソースの名前を示すもの）の総称である。WebにおいてはURNは普及しているとは言えない状況のため、Webの世界では基本的にURIとURLは同義で使われていると考えて差し支えない。

#### URIの設計
- URIの設計においては、下記の点を留意する
  - URIが変わらないようにする
  - プログラミング言語に依存した拡張子やパスを含めない
  - メソッド名やセッションIDを含めない
  - リソースを表現する名詞にする
  - シンプルでユーザビリティの高いURIにする

### HTTP
#### HTTP概要
- HTTPではクライアントが出したリクエストをサーバで処理してレスポンスを返す。

- クライアントでは、1つのリクエストを送信しレスポンスを受信する際に、下記のことを行う。
  - リクエストメッセージの構築
  - リクエストメッセージの送信
  - （レスポンスが返るまで待機）
  - レスポンスメッセージの受信
  - レスポンスメッセージの解析
  - クライアントの目的を達成するために必要な処理

- クライアントからリクエストを受けたサーバは、下記のことを行う。
  - （リクエストの待機）
  - リクエストメッセージの受信
  - リクエストメッセージの解析
  - 適切なアプリケーションプログラムへの処理の委譲
  - アプリケーションプログラムから結果を取得
  - レスポンスメッセージの構築
  - レスポンスメッセージの送信

#### HTTPメッセージ
- リクエストメッセージ（HTTPリクエスト）とレスポンスメッセージ（HTTPレスポンス）をまとめて、「HTTPメッセージ」と呼ぶ。

- リクエストメッセージの例
 
```
GET /test HTTP/1.1
HOST: example.jp
```

- リクエストメッセージの説明
  - リクエストライン
    - リクエストメッセージの1行目
    - 下記の要素から構成される（カッコ内は上記例での事例）
      - メソッド（GET）（※下記に別途詳細を記載）
      - リクエストURI（/test）
      - プロトコルバージョン（HTTP/1.1）
  - ヘッダ
    - リクエストメッセージの2行目以降
    - ヘッダはメッセージのメタデータであり、1つのメッセージは複数のヘッダを持てる
    - 各ヘッダは、「名前：値」という構成となる（HOST: example.jp）
  - ボディ
    - ヘッダのあとにボディが続くこともある
    - ボディには、そのメッセージを表す本質的な情報が入る

- レスポンスメッセージの例

```
 HTTP/1.1 200 OK
 Content - Type: application/xth.+xml; chaset=utf-8
 
 <html xmlns=“http://www.w3.org/1999/xhtml”>
 ・・・
 </html>
```

- レスポンスメッセージの説明
  - ステータスライン
    - レスポンスメッセージの1行目
    - 下記の要素から構成される（カッコ内は上記例での事例）
      - プロトコルバージョン（HTTP/1.1）
      - ステータスコード（200）（※下記に別途詳細を記載）
      - テキストフレーズ（OK）
  - ヘッダ
    - レスポンスメッセージの2行目以降
    - 上記例では、Content-TypeヘッダでHTMLのMIMEメディアタイプ（application/xth.+xml）と、その文字のエンコーディング方式（utf-8）を指定している
  - ボディ
    - ヘッダとボディは空行で区切られる
    - 上記例ではボディにHTMLが含まれている

#### HTTPメソッド
- HTTPメソッドは、クライアントが行いたい処理をサーバに伝えるものである。

- HTTPには以下の8つのメソッドが定義されている（主に使われるメソッドは5つか6つ）
  - GET：リソースの取得
  - POST：子リソースの作成、リソースへのデータの追加、ほかのメソッドでは対応できない処理
  - PUT：リソースの更新、リソースの作成
  - DELETE：リソースの削除
  - HEAD：リソースのヘッダ（メタデータの取得）
  - OPTIONS：リソースがサポートしているメソッドの取得
  - TRACE：自分宛にリクエストメッセージを返す（ループバック）試験
  - CONNECT：プロキシ動作のトンネル接続への変更

- CRUDとHTTPメソッドは下記のように対応している。
  - [凡例] CRUD名 - 意味 - メソッド
  - Create - 作成 - POST/PUT
  - Read - 読み込み - GET
  - Update - 更新 - PUT
  - Delete - 削除 - DELETE

#### ステータスコード
- レスポンスメッセージの中で、その意味を伝えるのがステータスコードである。例えば、ステータスコード「200」は、クライアントのリクエストが正常終了したことを示す。

- ステータスコードは3桁の数字であり、先頭の数字によって5つに分類される。
  - 1XX：処理中
  - 2XX：成功
  - 3XX：リダイレクト
  - 4XX：クライアントエラー
  - 5XX：サーバエラー

- 最もよく使われている9つのステータスコード
  - 200 OK：リクエスト成功
  - 201 Created：リソースの作成成功
  - 301 Moved Permanently：リソースの恒久的な移動
  - 303 See Other：別URIの参照
  - 400 Bad Request：リクエストの間違い
  - 401 Unauthorized：アクセス権不正
  - 404 Not Found：リソースの不在
  - 500 Internal Server Error：サーバ内部エラー
  - 503 Service Unavailable：サービス停止

### 実例
- HTTPメッセージ（HTTPリクエストとHTTPレスポンス）の内容は、Chromeの開発者ツールのネットワークタブで確認することができる。
- https://tech-essentials.work/dashboard のHTTPメッセージを実際に確認した結果が下記である。

![スクリーンショット 2021-09-18 0.42.41.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/fd8744a1-5318-4329-96c5-b0f0e03b43b5.png)![スクリーンショット 2021-09-18 0.42.49.png](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/448ca8b8-28c0-4f13-bc45-286674191f95.png)

- 1枚目の画像のHeadersタブの部分からは、下記のようなことを読み取ることができる。
  - リクエスト
    - リクエストメソッド : GET
    - URL : https://tech-essentials.work/dashboard
    - プロトコルバージョン : HTTP/1.1
  - レスポンス
    - プロトコルバージョン : HTTP/1.1
    - ステータスコード・テキストフレーズ : 200 OK（※GETメソッドでのリクエストが成功している）
    - Content - Type : text/html
    - 文字のエンコーディング方式 : charset=utf-8

- 2枚目の画像のResponseタブの部分には、レスポンスボディに含まれるHTMLの内容が記載されている。

- フォームの送信内容（メッセージボディ）について

・https://tech-essentials.work/mypage/profile/edit で更新ボタンを押した時

Chromeの開発者ツールのネットワークタブで、profileファイルを開くと、Headersタブ内にGeneral, Response Headers, Request Headersの下にForm Data という項目があり、そこでフォームに入力した内容を確認することができる。

![image](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/415cba5b-a627-4f24-a269-39a02a41e41b.png)


また、この時のリクエストメソッドはPOSTで、ステータスコードは320 Foundとなっている（302 Foundは、リクエストされたリソースが一時的に Location で示された URL へ移動したことを示す。（参照 : https://developer.mozilla.org/ja/docs/Web/HTTP/Status/302 ））

![image](https://tech-essentials-production.s3.ap-northeast-1.amazonaws.com/af7691a3-8fa9-4595-97b4-5c737335b455.png)

### 参考にした書籍やwebページ
- [フォームデータの送信 - ウェブ開発を学ぶ | MDN](https://developer.mozilla.org/ja/docs/Learn/Forms/Sending_and_retrieving_form_data)
- [webを支える技術](https://www.amazon.co.jp/Webを支える技術-HTTP、URI、HTML、そしてREST-WEB-PRESS-plus/dp/4774142042/ref=sr_1_1?adgrpid=52846719243&dchild=1&gclid=CjwKCAjw-ZCKBhBkEiwAM4qfF1LQZOm4N8JVHASfXRQBuWNMh4tqsKBf-MAOyDyuQS5Tv7fYQC9LABoCQq8QAvD_BwE&hvadid=338588425160&hvdev=c&hvlocphy=1028852&hvnetw=g&hvqmt=e&hvrand=3993001199732908843&hvtargid=kwd-333749887651&hydadcr=16039_11170862&jp-ad-ap=0&keywords=webを支える技術&qid=1631896989&sr=8-1)
- [paizaラーニングWeb技術入門編](https://paiza.jp/works/web_tech/primer/beginner-webtech1)
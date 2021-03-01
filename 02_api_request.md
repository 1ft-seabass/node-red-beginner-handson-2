# 猫画像 API からデータを取得してみよう

この章では 猫画像 API からデータを取得してみようということで進めていきます。

![image](https://i.gyazo.com/364cbe715eccee492a9480e772535318.png)

Node-RED パレットを見てみると、ネットワークというカテゴリに実に様々なノードが並んでます。

今回はその中でも、たくさんの API につなぐことができる HTTP プロトコルに対応した http request ノードを使ってみましょう。

![image](https://i.gyazo.com/36ef8dd43559913b54a2a5712c3ee376.png)

## API の世界をのぞいてみる

API とは、 Application Programming Interface の頭文字です。データを提供したり、何かしらの機能を提供するソフトウェアの一部を、私たちのような外部の人間が扱えるように公開されている窓口です。

たとえば、WEBアプリケーションなどインターネットの様々な仕組みを構築するとき、 API からデータを取得して加工して表示することが多いです。データや機能を借り受けるということですね。

![image](https://i.gyazo.com/fdad42d082b6fc15b0be1030af528dd6.png)

API は、データだけでなく、さらに踏み込んでAI・IoT・チャットなど様々な技術を手軽に利用することもできます。

![image](https://i.gyazo.com/382d040478d18d9e3d7f15188c2cb3f3.png)

API につなげるためには HTTP という通信のルール（プロトコル）を理解するのことが大切です。

* データを取りに行くとき（問い合わせ）
    * HTTPリクエスト
    * HTTPステータス
* データを受け取るとき（データ応答）
    * HTTPヘッダー

さらにデータをJSONデータという形で受け取ります。受け取ったAPIに合わせて、JSONデータの中身を理解して、必要なデータを取り出しします。

## フリーのAPIのリストコレクション public-apis

今回はフリーのAPIのリストコレクション [public\-apis](https://github.com/public-apis/public-apis) をご紹介します。

![image](https://i.gyazo.com/ae74f03f1dc5a89503579497a476530a.png)

英語ですが、実に 100 以上の API があり見ているだけで楽しいです。

とはいえ、本来であれば、各 API の仕様に合わせて HTTP リクエストを正しくアクセスするかを読み解く必要があります。

* HTTP リクエスト方法
* HTTP ヘッダーや必要なパラメータ
* 受け取った HTTP レスポンスがどのようなデータ構造になっているか
* などなど

![image](https://i.gyazo.com/09bc50e2b861135f3ab3c829b906f79d.png)

今回は、私が別の機会に書いた [すぐにAPIを体験！public\-apis 100以上のJavaScript axiosサンプル集](https://protoout.studio/posts/public-apis-api-get) を使っていきます。

## [実践]: Node-RED で http request する仕組みを作ってみる

![image](https://i.gyazo.com/baedc63f4a27f63eb33277aeb49481eb.png)

まず、パレットから http request ノードをワークスペースにドラッグアンドドロップしましょう。

![image](https://i.gyazo.com/c408d12ed4e8cac8f21f908895ec1ba6.png)

http request ノードをワークスペースに配置しました。

![image](https://i.gyazo.com/247a57dbff5c98d92558dc25d8c8a7bb.png)

inject ノードをワークスペースに配置して、動かすきっかけを指示できるようにします。

![image](https://i.gyazo.com/c7fa03567191282fe7dda707b2755b90.png)

debug ノードをワークスペースに配置して http request ノードが、どこかのURLにアクセスして取得したデータをデバッグタブに表示できるようにします。

![image](https://i.gyazo.com/79d9e7553396c09fb9d2daf96a726d20.png)

inject ノード → http request ノード → debug ノード とつなぎます。

これで「何かの API につなぐ」仕組みはひとまず出来上がりました。

## 今回は猫画像をつなげてみます

さきほどの フリーのAPIのリストコレクション [public\-apis](https://github.com/public-apis/public-apis) の中から、かわいい猫画像をランダムに返答してくれる Random Cat というAPIを使ってみましょう。

![image](https://i.gyazo.com/dca6cd0f1ae97e469bbbfb81e19085fc.png)

https://aws.random.cat/ こちらです。

HTTP リクエストの仕様として読み解くと以下のようになります。

* HTTP リクエスト方法
  * GET メソッド
* アクセスする API の URL
  * https://aws.random.cat/meow

これのみです。追加で設定すべきパラメータもなく、これは、GET リクエストのためブラウザからアクセスでき、とても扱いやすいAPIです。

## [実践]: Node-RED の http request ノードに設定してみる

![image](https://i.gyazo.com/5eedcc78773990656fca5301fbc7acf4.png)

さきほどのワークスペースに戻って http request ノードをダブルクリックして詳細の設定をしましょう。

![image](https://i.gyazo.com/01180438ef52a518cf184d32394cbd86.png)

このような設定項目があります。HTTP リクエストをするために様々な設定があります。

![image](https://i.gyazo.com/ace03d6fefff738b0df25119c855e122.png)

今回は

| 項目名 | 変更する内容 |
| --- | --- |
| メソッド | GET |
| URL   | https://aws.random.cat/meow |


で設定します。

完了をクリックします。

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

## [実践]: 動かしてみる

![image](https://i.gyazo.com/d02727178bb7fb092a36642e2eb0d9ee.png)

それでは inject ノードの ![image](https://i.gyazo.com/917722feaa1444c55f1a19ece002757d.png) のボタンをクリックしましょう。

![image](https://i.gyazo.com/43f320285579e73bfd807acb52699805.png)

サイドバーのデバッグタブには、猫画像APIからリクエストが表示されます。

## [実践]: 返答データを文字列からJSONに変更する

![image](https://i.gyazo.com/e0354e1cefe80b1ebf9bc905cc081d2c.png)

現状では取得したデータは文字列です。今後扱いやすいようにJSONに変更しましょう。

文字列のままだと頑張って読んで探して取り出す必要がありますが、JSONデータで受け取れると入れ子構造で目的のデータが取り出しやすくなります。

それでは、http request ノードをダブルクリックをしてプロパティ編集画面を表示します。

![image](https://i.gyazo.com/7ad33f4cec6e1be03622557bd9af245e.png)

http request ノードの設定の `出力形式` をUTF文字列から `JSONオブジェクト` 変更します。

![image](https://i.gyazo.com/d51b5faaace99acf648a267bb27b91c9.png)

すると、文字列ができるかぎり JSON に変換されます。

[![Image from Gyazo](https://i.gyazo.com/419a64aecb7753fa7cfe646155910dab.gif)](https://gyazo.com/419a64aecb7753fa7cfe646155910dab)

このように、今回はシンプルな構造ですが、JSONの中身をツリー構造でのぞくことでデータの内容が把握しやすくなります。

![image](https://i.gyazo.com/d51b5faaace99acf648a267bb27b91c9.png)

ためしに、fileにある猫画像URLをコピーして

![image](https://i.gyazo.com/3b27b584458761dd13dd38b7db6ada17.png)

ブラウザのアドレスバーにペーストしてアクセスしてみましょう。

![image](https://i.gyazo.com/511188999f8b0f418dfbe413ece02d2c.jpg)

無事、猫画像が見れていますね！次の章で Node-RED の中で表示してみましょう！

## この章のまとめ

![image](https://i.gyazo.com/9d560ea43e326e7f26b905d1771e60a2.png)

猫画像 API を元に http request ノードを使って外部からデータを取得する流れをお伝えしました。

## 次の章

[猫画像 API のデータを表示してみよう](03_cat_image_output.md)

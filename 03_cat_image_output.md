# 猫画像 API のデータを表示してみよう

この章では 猫画像 API からデータを表示してみようということで進めていきます。

![image](https://i.gyazo.com/3e745d3dd6f370d87da4697631895978.png)

前章で猫画像の入っているJSONデータを取得することができました。

![image](https://i.gyazo.com/64a83596c5e6d858400e497702166a5d.png)

猫画像の入っているJSONデータから猫画像URLだけ取り出して、Node-REDで画像が見えるようにしていきましょう。

このようにAPIの仕様をつかんで、JSONデータの中身を理解して、必要なデータを取り出せるようになると、さまざまなAPIを自分好みに使えたり、他のAPIとも連携できるようになるのでやってみましょう。

## [実践]: change ノードをフローに準備する

![image](https://i.gyazo.com/82db4f456538ca13751287e836c5858e.png)

Node-RED で、データを取り出すノードは change ノードを使います。「データを目的に合わせて変更 change する」というニュアンスです。

![image](https://i.gyazo.com/7b9b8d738f1df5b92e13a11af0f208de.png)

前章のフローを見てみます。

![image](https://i.gyazo.com/9476d893f0c004c16333c85d3dd4810b.png)

change ノードを入れるために http request ノードと debug ノード の間をのばします。

![image](https://i.gyazo.com/85c9db5856173b48a573a4e6c3330902.png)

パレットから change ノードをドラッグして移動します http request ノードと debug ノード の間のラインにのせると、点線になって反応するのでドロップします。

![image](https://i.gyazo.com/c76bd85c0989945180f26b1497cad6e2.png)

これで change ノードが入りました。

## change ノードでデータを取り出すには？

![image](https://i.gyazo.com/5d4495a528da6b9f46abeb89bc501975.png)

http request ノードで受け取った JSON データから猫画像URLだけ取り出してみましょう。いまは payload という Node-RED がやりとりするデータの入れ物の中に file という入れ物がさらにあって猫画像URLが入っています。

![image](https://i.gyazo.com/8eef2a2898474f8527a9b31bc0ecb4ed.png)

change ノードをダブルクリックして詳細の設定をしていきます。

![image](https://i.gyazo.com/564640c0879fc40f3701fec41d46717d.png)

* 上部
  * msg.payload に対して、下部の `対象の値` で `値の代入` を行う（値を置き換える）という意味
* 下部
  * 対象の値に関する処理。いまは ![image](https://i.gyazo.com/584c21fbc8937843039506d40730c102.png) なので文字列を示します。

![image](https://i.gyazo.com/ab06ed6c169441e7b352eda995841297.png)

もし、このように、対象の値を文字列ウォンバットに設定した場合は、以下のような動きになります。

![image](https://i.gyazo.com/4688bf65e7d603cfd66a03685e15ec0f.png)

msg.payload が対象の値を文字列ウォンバットが変更されて、次のノードに渡されます。

![image](https://i.gyazo.com/2ab7b67d4d24bb683e338804aee7dabc.png)

文字列以外にも、様々な値を設定することができます。

![image](https://i.gyazo.com/f0b3b508c8e86c8331d48ee7ff18ff93.png)

ということで、今回は `JSONata式` という値を使います。

JSONata は、JSONデータから抽出したり、加工・操作・計算などを手軽に行える軽量クエリおよび変換言語です。

* JSONata参考文献
  * [JSONata の言語ガイドを訳してみた \- Qiita](https://qiita.com/yamachan360/items/91805334890d18ef6e23)
    * 日本語の解説。分かりやすいです。
  * [JSONata Documentation · JSONata](https://docs.jsonata.org/overview.html)
    * 公式のドキュメントです（英語）

といいつつ、今回は JSONata の記述のなかでも「ドットを使ってデータを取り出す」ことだけをするので、深く知らなくても大丈夫です。気になる方は、あとでぜひ試してみてください。

![image](https://i.gyazo.com/393d1d6dc8bca9f14ccff1fbbd2425a8.png)

JSONata はドットを使って JSON データの中身を取り出すことができます。まるで現実の住所をたどるような形で、今回の値で言うと payload を起点に猫画像の中に入っている file をいう値を取り出すことができます。

payload の中の file という値ということで `payload.file` となります。

## [実践]: change ノードでデータを取り出す

それでは、実際に設定していきましょう。まず、change ノードをダブルクリックしてプロパティの編集画面を開きます。

「ルール」を下記のように変更します。

- 「対象の値」の種類を「JSONate式」（英語では expression）に変更する
- 「対象の値」の値（入力欄）に `payload.file` を入力する

![image](https://i.gyazo.com/c90a6a04c6331cf86752db74d30fb177.png)

この指定により、`payload.file` に指定されている猫画像を指定することができます。

![image](https://i.gyazo.com/dcc16d4d8fa6f944b138e2c65b081d64.png)

完了をクリックして、

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

![image](https://i.gyazo.com/3591400ff5996388ab620ca1246e0455.png)

デプロイが終わったら inject ノードをクリックして動かしてみましょう。

![image](https://i.gyazo.com/6b8ba6dbfd0c7a5b962cf818163d611d.png)

デバッグタブを見てみると、無事 file の中身が取り出されて猫画像の URL だけが取り出すことができました。

## [実践]: 画像を表示するノード node-red-contrib-image-output ノードを追加

さて、画像を表示するノードですが、標準のノード群には存在していません。

ですが、大丈夫です。

Node-RED は標準のノード群に [フロー検索（英語）](https://flows.nodered.org/) で探して、追加することができます。

* 参考文献
  * [パレットにノードを追加する : Node\-RED日本ユーザ会](https://nodered.jp/docs/user-guide/runtime/adding-nodes)

今回は画像を表示するノード node-red-contrib-image-output ノードを追加してみましょう。

![image](https://i.gyazo.com/9ee49941997b91b8876283c500568cc8.png)

右上のメニューを開きます。

![image](https://i.gyazo.com/d900f4ec51729cd943c4dfd7cbd4a4e0.png)

パレットの管理をクリック。

![image](https://i.gyazo.com/e2925fe3a3d0d91f090781899084dd36.png)

現在のノードが表示されるので、

![image](https://i.gyazo.com/fb7c3f5177824037c5a14905f6d0bb94.png)

ノードを追加のタブをクリックします。

![image](https://i.gyazo.com/417aebe2e3b97171e91678537f8b6246.png)

ノードを追加機能が表示されます。

![image](https://i.gyazo.com/fe0db25502255250ea5923575af72034.png)

ノードを検索と書かれているエリアで検索できるので、

![image](https://i.gyazo.com/fb02568bbd1e757da835b91938c3d861.png)

node-red-contrib-image-output と入力して検索すると、 node-red-contrib-image-output が出てきます。

![image](https://i.gyazo.com/e0828f744d38cc58f8daf935f7871e96.png)

ノード追加をクリックします。

![image](https://i.gyazo.com/090675c4500e96afbbe138ab41077311.png)

ダイアログが下りてくるので追加ボタンをクリックします。

![image](https://i.gyazo.com/45e03adcb75df2e225b22d8fa821624f.png)

しばらく待っているとインストールが完了します。

## [実践]: node-red-contrib-image-output ノードをフローに加える

node-red-contrib-image-output ノードを加えて、画像を表示しましょう。

![image](https://i.gyazo.com/59b74b1ebb67ba1850a42b72f10cbfdf.png)

node-red-contrib-image-output ノードは、このように前のノードから送られてきたデータ msg.payload の値を画像URLとしてデータを取得してきてワークスペースに表示します。

![image](https://i.gyazo.com/7dca041d65021ecda96e379761a952b1.png)

先ほどのフローに戻ります。

![image](https://i.gyazo.com/040f0c8a76affc161eb03ea025a0dac0.png)

パレットから node-red-contrib-image-output ノードを探します。 image という名前でいるはずです。

![image](https://i.gyazo.com/fc7906e7a199e9d557d2631cf7dfbbaa.png)

ドラッグアンドドロップして追加します。ちょうど change ノードの下あたりが、おさまりがいいです。

![image](https://i.gyazo.com/49c0105e37a9c5fb852ec7b5addff300.png)

node-red-contrib-image-output ノードが配置できました。

![image](https://i.gyazo.com/3dcd5101056263bebebdab0841a63bde.png)

change ノードから node-red-contrib-image-output ノードにつないでみましょう。

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

![image](https://i.gyazo.com/54c694e451fe8e2379442f35acf1af9d.png)

inject ノードをクリックして猫画像が node-red-contrib-image-output ノードの下部に表示されるか試してみましょう。

画像が表示されるはずです！

## [実践]: 画像を大きくしたいとき

![image](https://i.gyazo.com/018e05bc6e6c32f14a96e32d2405ca8c.png)

画像を大きくしたいときは node-red-contrib-image-output ノードをダブルクリックして Width の値を変更してみましょう。最初は 160 pxと小さめです。

![image](https://i.gyazo.com/7850ab3c70fe4d9b81856ef2cf61f5e8.jpg)

たとえば 480 と変更してデプロイしてみると大きな猫画像を試すことができます！

## この章のまとめ

![image](https://i.gyazo.com/9d560ea43e326e7f26b905d1771e60a2.png)

APIから取得したデータから猫画像データを取り出して Node-RED のワークスペースに表示することをお伝えしました。

## 次の章

* [チャットにデータを送ってみよう](04_chat_output.md)


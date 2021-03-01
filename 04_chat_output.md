# チャットにデータを送ってみよう

この章ではチャットにデータを送ってみましょう。

![image](https://i.gyazo.com/5b539505df764578d97b1aff7551eec8.png)

猫画像 API の仕組みでAPIとやり取りができるようになったので、 Discord というチャットツールで特定のチャンネルにメッセージを送るAPIがあるので使ってみます。

## Discord とは

![image](https://i.gyazo.com/dbecfb11772f8eaa1a6ec18e76fff525.png)

> [Discord \(ソフトウェア\) \- Wikipedia](https://ja.wikipedia.org/wiki/Discord_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))
>
> Discord（ディスコード）は、Windows・macOS・Linux・Android・iOS・Webブラウザで動作する、ビデオ通話・音声通話・VoIPフリーソフトウェアである。アメリカ合衆国で開発されており、2019年5月時点でユーザー数が2億5000万人に達している。

とのことで、当初はゲーム実況やゲーム時のコミュニケーションに使われていますが、当初よりボットやメッセージといった仕組みがプログラムからも作りやすく、最近では、開発に使う方がいたりと、なかなか扱いやすいチャットツールです。

チャットツールというと、LINE ・ Slack ・ Microsoft Teams ・ チャットワークといったツールもありますね。自由にメッセージが送れると、いろいろなことができます。

たとえば、

* 自分用のチャットルームで必要なニュースやSNSでの情報を集めて把握しやすくする。
* 仕事用のチャットルームで、今日1日の自分業務を朝にリマインドしていいスタートを切る。

といったことができます。

## Discord でメッセージを送れる API をしらべる

![image](https://i.gyazo.com/4e8decdb4ec141573ac0aefb03cddb48.png))

[DiscordのWebhookドキュメント](https://discord.com/developers/docs/resources/webhook#execute-webhook) の Webhook 実行については、このように書かれています。

読み解くと、

* `POST /webhooks/{webhook.id}/{webhook.token}`
  * POST メソッドでリクエストしてほしい
  * `/webhooks/{webhook.id}/{webhook.token}` 各チャンネルで特有のURLがある（後述）
* 送るデータはJSONで content というオブジェクトの中に文字を入れればメッセージとして送れる
  * 例えば、こんな感じ。 `{"content":"やっほー！"}`

といった API のルールが書かれています。


## 今回のメッセージを送るチャンネル

![image](https://i.gyazo.com/851c2a1d3b5ba2daf01810a890ed0025.png)

私が持っている Discord サーバーの一つにチャンネルを一つ作りました。

あらかじめ、API の送り先を発行してあります。各ワークショップで振り分けていますので、いま入っているワークショップの方を使いましょう。

田中ワークショップルームの人

```
https://discord.com/api/webhooks/815829613052952577/KcOkTYSJ5hHPP5GTd3rKaFvn63FYZ_Yn3T64eSNGdOJaWqRkGDgi1P0wLa9UbtgWEL5v
```

横井ワークショップルームの人

```
https://discord.com/api/webhooks/815837652988461097/19QNd6-8MdF_G6cDigdaiGKxI2Y-FfWHJZA8FXkvPfkwx_fFsdRHbs46zRa8lERb320S
```

こちらです↑

今回はこちらのURLに、POSTリクエストで送るデータはJSONで content というオブジェクトの中に文字を入れてメッセージを送ります。

## [実践]: Katacoda での Node-RED の準備

![image](https://i.gyazo.com/2de10931d575e9e705572040afb51d37.png)

一旦、Katacodaをリロードして再起動しましょう。

https://www.katacoda.com/kazuhitoyokoi/scenarios/node-red

もしも閉じてしまった方はこちらから。再度、起動しましょう。

![image](https://i.gyazo.com/239c0aebb18423e4682942122c26206a.png)

このように教材がはじまるので START SENARIO をクリックします。

![image](https://i.gyazo.com/1a31474455106f387e226d5be80e94dd.png)

起動して、しばらく待ちます。

![image](https://i.gyazo.com/7567bd2523e69ba0ce1fa9827fe27039.png)

おおよそ、40 秒 ～ 1 分待っていると起動するのでお待ちください。

## Node-RED のフローを作ってみる

![image](https://i.gyazo.com/9a0be632d5aa24d4b229db4b14f38ebf.png)

このように inject ノードで、メッセージの中身のデータを作って、http request ノードで前述した Discord の API URL に送るように設定して、debug ノードで送られた結果を受信する仕組みです。

それでははじめましょう！

![image](https://i.gyazo.com/3db27fed8052a008086a742ea65b5116.png)

はじめに、パレットから inject ノードをワークスペースにドラッグアンドドロップしましょう。

![image](https://i.gyazo.com/292dc57f4c424a2ce8078cd47c88a8b1.png)

次に、パレットから http request ノードをワークスペースにドラッグアンドドロップしましょう。

![image](https://i.gyazo.com/53f50b82d7fa95cbe2deff41223a9810.png)

最後に、パレットから debug ノードをワークスペースにドラッグアンドドロップしましょう。

![image](https://i.gyazo.com/284196220e03ebd9784433b02ba384f2.png)

各ノードを inject ノード → http request ノード → debug ノード の順につなぎます。これで、フローは完成です。

## [実践]: inject ノードで、送るメッセージの中身のデータを作る

![image](https://i.gyazo.com/b0df92b33ddc8b9b65c2eaf56c5b6297.png)

inject ノードをダブルクリックして詳細を設定します。

![image](https://i.gyazo.com/296c3407f36c3e89bcb54c10cb7be8d6.png)

デフォルトでは `msg.payload` が日時（タイムスタンプ）になっています。

![image](https://i.gyazo.com/2f6a8b151e728bf667ec9915b4f24642.png)

送るデータのタイプを、今回の送るデータタイプ JSON と合わせて JSON に設定しましょう。

![image](https://i.gyazo.com/2a9c84a886e556cea76e4e7f08f969b5.png)

このように送るデータを `{"content":"やっほー！～～～だよ！"}` というJSONデータで設定します。`～～～だよ！` の `～～～` の部分には、皆さんのお名前に書き換えてみてください。

設定出来たら、完了ボタンをクリックして設定を完了します。

![image](https://i.gyazo.com/9d747d753252057d3c6260be3f906ed1.png)

送るデータは実際にはこのようなイメージです。

JSON データは msg の中の payload の中にある content というオブジェクトの中に、今回送りたい文字を入れています。

## [実践]: http request ノードで前述した Discord の API URL に送るように設定

![image](https://i.gyazo.com/4a24882968e410ad4cd7bb6ebacdfa01.png)

http request ノードをダブルクリックして詳細を設定します。

![image](https://i.gyazo.com/30c0851aff9d86201a5d4756c2da7678.png)

詳細設定はこのようになっています。

![image](https://i.gyazo.com/55948c3fcb19c1de27252a63a647dc43.png)

メソッドの項目を POST に変更して、今回の Discord API の仕様に合わせます。

![image](https://i.gyazo.com/6c3d0e4183aabfa1f05e182ecf82aa77.png)

URL の項目に今回のチャンネルへメッセージが送れる API URL の

```
＜当日だけ開かれたDiscordのURLを記述していました＞
```

を設定します。

設定出来たら、完了ボタンをクリックして設定を完了します。

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

## 試してみる

![image](https://i.gyazo.com/0d013fd82eaf3732f9a721f84bc74c49.png)

inject ノードのボタンをクリックして、データを送ってみましょう！

![image](https://i.gyazo.com/99059658d1e231e42f8eed63197c81b3.png)

こちらの Discord チャンネルに、このようにデータが送られるはずです！

![image](https://i.gyazo.com/37508100205348a600a7978891ab50f5.png)

debug ノードは、データがちゃんと送られるときは `""` という空文字がもどってきます。

## トラブルシューティング

![image](https://i.gyazo.com/68b88b9c9c605da251bd2b240b1b4b70.png)

うまくデータが送られず debug ノードに上記のようにデータがきた場合は、APIのURLがコピーペーストがうまくいかず欠けてしまったときに、起きやすいエラーです。

![image](https://i.gyazo.com/d67810480bd01e001e37e217b63b083e.png)

こちらは送る JSON データがうまく設定できてないときにエラーがおきます。例えば `{"content":"やっほー！～～～だよ！}` のデータは `だよ！` のあとのダブルクォーテーションがなくなっていることで、送るメッセージ部分が文字列にならずエラーが起きています。

このあたりは、JSON データの書き方を読んでみると理解が深まります。

* 参考文献
  * [JSON入門 \- とほほのWWW入門](http://www.tohoho-web.com/ex/json.html#string)

## エクストラ1：画像を送ってみよう

inject ノードで送る JSON データを変更してみましょう。

```json
{"content":"画像を送るよ! https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Vombatus_ursinus_-Maria_Island_National_Park.jpg/1280px-Vombatus_ursinus_-Maria_Island_National_Park.jpg"}
```

こちらのデータを送ってみると、

![image](https://i.gyazo.com/b233491133bbdb0ceb42cca80810f923.jpg)

このように、画像URLがあっていれば Discord で画像を見ることができます。今回の画像は、[ウォンバット \- Wikipedia](https://ja.wikipedia.org/wiki/%E3%82%A6%E3%82%A9%E3%83%B3%E3%83%90%E3%83%83%E3%83%88) から引用。

好きな画像を送ってみましょう！

## エクストラ2：絵文字を送ってみよう

inject ノードで送る JSON データを変更してみましょう。

```json
{"content":"ホッとするー！！！！:relaxed::relaxed::relaxed::relaxed::relaxed:"}
```

こちらのデータを送ってみると、

![image](https://i.gyazo.com/99b2906db03c4c152d4417b67c59781f.png)

このように `:relaxed:` という部分が絵文字となって投稿できます。

[Qiita/Github/Slack/Discord 絵文字一覧 \- Qiita](https://qiita.com/yamadashy/items/ae673f2bae8f1525b6af) 

こちらを参考に絵文字でメッセージを装飾してみましょう～。

## この章のまとめ

![image](https://i.gyazo.com/9d560ea43e326e7f26b905d1771e60a2.png)

Discordの今回設定した API URL にデータを送ってメッセージをチャンネルに表示することができました。

## 次の章

* [READMEにもどる](README.md)



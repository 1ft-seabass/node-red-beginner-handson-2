# Node-REDを動かしてみよう

## Node-RED とは

![image](https://i.gyazo.com/07c703e8af39a7a5aaf5e660f663b576.png)

[Node\-RED日本ユーザ会](https://nodered.jp/)

Node-RED は Node.js で動く仕組みです。Node-RED はサーバーとフロントエンドの両方を作れる仕組みです。GUI（ビジュアルで見えるUI）によって、APIを取得する仕組みであったり、dashboard のように表示も作れます。

[フロー検索](https://flows.nodered.org/)で他の人の作った仕組みやノード再利用でき、プロトタイプするうえでも小さく素早く進める側面を備えています。

### STEM 教育の目線の Node-RED

![image](https://i.gyazo.com/ad71e11b0d71e69b38be011a9d9cc448.png)

Node-RED はローコードプログラミングな手軽なアプローチで STEM 教育で言及されるような科学・技術・工学・数学といった要素に触れることができます。

たとえば、IoT（Internet of Things）のような現実世界をセンサーで取得したり（科学）、何かを動かしたり（工学）あるいは、インターネット上の様々なデータ API を取得して（技術）データに応じて計算したり分析したりできます（数学）。

## Katacoda について

![image](https://i.gyazo.com/3d73fc84a538e4cffe4a15fb30218abc.png)

一定時間の使い切りの環境で自由に技術で遊ぶ事ができる WEB サービスです。環境構築をブラウザでだけでできて「まず試して体験」に注力できます。制限時間は **おおよそ1時間** くらい。

![image](https://i.gyazo.com/2de10931d575e9e705572040afb51d37.png)

ですので今日は、おおよそ 30 ～ 40 分で 1 つのハンズオンで行い、合計 2 つのハンズオンを行います。途中で 1 つめのハンズオンが終わったら Katacoda をリロードして起動しなおします。

## [実践]: Katacoda での Node-RED の準備

教材はこちらです。

https://www.katacoda.com/kazuhitoyokoi/scenarios/node-red

まず、自分のアカウントでログインしてアクセスしてみましょう。

![image](https://i.gyazo.com/239c0aebb18423e4682942122c26206a.png)

このように教材がはじまるので START SENARIO をクリックします。

![image](https://i.gyazo.com/1a31474455106f387e226d5be80e94dd.png)

> This is a playground environment for the Node-RED flow editor.
> 
> After the message, "Connecting to Port 1880" is displayed for about 40 seconds, your browser will automatically load Node-RED flow editor. On the playground, you can install nodes, develop your flow, and deploy the flow. Because it is a temporary environment on Katacoda, the Node-RED instance will be removed automatically after about 50 minutes.
> 
> Open new tab: https://~~~~~~~~~~~~~~~~.environments.katacoda.com
>
> これは、Node-REDフローエディタの遊び場環境です。 
> 
> "Connecting to Port 1880"「ポート1880に接続しています」というメッセージが約40秒間表示された後、ブラウザは自動的にNode-REDフローエディタをロードします。 遊び場では、ノードをインストールし、フローを開発し、フローをデプロイできます。 Katacodaの一時的な環境であるため、Node-REDインスタンスは約50分後に自動的に削除されます。 

とのことで、こちらは起動して待ちます。

![image](https://i.gyazo.com/7567bd2523e69ba0ce1fa9827fe27039.png)

おおよそ、40 秒 ～ 1 分待っていると起動するのでお待ちください。

## 動かしてみよう

[はじめてのフロー : Node\-RED日本ユーザ会](https://nodered.jp/docs/tutorials/first-flow)

こちら元に進めます。

ウォームアップしてみましょう。

![image](https://i.gyazo.com/b94d2d8a7a58febf76226db658c7d5dc.png)

このようなシンプル仕組みをつくります。

## ノードについて

![image](https://i.gyazo.com/4209abfa226dca0d1a4c1d3421768bbe.png)

まず ノード（Node）はNode-REDを構成する基本的な構成要素です。処理をする機能のかたまりです。

![image](https://i.gyazo.com/ac72b467278872701170501f629731ef.png)

ノードはフロー中の前方のノードからメッセージを受け取るか、外部イベントを受け取ることで動き出します。ノードはメッセージまたはイベントを処理し、 フロー中の次のノードにメッセージを送ります。左から右に処理されていきます。

![image](https://i.gyazo.com/b2e38a11e61da1ad55ff387493b71891.png)

メッセージはJSONデータで構成され、`msg` という一番上のオブジェクトにぶら下がっている `payload` というオブジェクトの中で、各ノードで処理された内容がバケツリレーのようにやり取りされていきます。

![image](https://i.gyazo.com/7a554e64647323ace99a930de52bfe67.png)

こんな感じです。

![image](https://i.gyazo.com/20007903edfd97e9aabddeedd5d6d8d5.png)

今回は inject というノードでボタンクリックをきっかけにメッセージを送り、 debug ノードという送られてきたメッセージをサイドバーのデバッグタブに表示するノードに送ります。

## 画面紹介

![image](https://i.gyazo.com/4e4f325615d23c2a56929fc767ce4327.png)

パレットはインストール済みで利用可能なすべてのノードが含まれます。ノードが置かれているエリアです。

![image](https://i.gyazo.com/47f080539655f431df2bc6afbf2eb845.png)

ワークスペースはパレットからノードを配置してフロー（データの流れ）を作るエリアです。

![image](https://i.gyazo.com/2b44b8d4535ed54a2ce46629fec8f96f.png)

サイドバーは、エディタ内に多くの便利なツールを提供するエリアです。

* ノードについてのさらなる情報
* ヘルプを確認するパネル
* デバッグメッセージを確認するパネル
* フローの設定ノードを確認するパネル

などがあります。

## [実践]: inject ノードと debug ノードをつなげていく

![image](https://i.gyazo.com/69d9424ea7db4779794c1d39e1d0a44f.png)

inject ノードをワークスペースにドラックアンドドロップします。

![image](https://i.gyazo.com/4ab5cd15ee540f8b2181cafc29cf9377.png)

inject ノードの横にdebugノードをドラックアンドドロップします。

![image](https://i.gyazo.com/b8eb34fb3296018ddae614e01bd47a50.png)

inject ノードと debug ノードをつなぎます。つなぐものはワイヤーといいます。

![image](https://i.gyazo.com/58de57346d51b7620c32562f9c8690bf.png)

デプロイボタンをクリックすると今作ったものが反映されます。

![image](https://i.gyazo.com/6d69e6990487e06533edba753d67904e.png)

## [実践]: 動かしてみる

![image](https://i.gyazo.com/486f98add3229d4cc880359bf1b3b643.png)

debugノードでデータがきてるか確認します。

![image](https://i.gyazo.com/6efbc1b0671669a3e5201e23e7298216.png)

debugノードのデータはサイドバーのデバッグタブをクリックすると見れます。

![image](https://i.gyazo.com/f99e3989db06dd84900022d8be76cb75.png)

injectノードの横のボタンを押すとdebugノードにデータが送られます。今回はinjectノードは日付（タイムスタンプ）を送っています。さきほどのデバッグタブでdebugノードが受け付けたデータを確認できます。

## [実践]: injectノードで送るデータを変更

injectノードをダブルクリックしてデータを変更しましょう。

![image](https://i.gyazo.com/05dc870ae85c44d1be74d97b1a474b41.png)

ノードはダブルクリックすると細かな設定ができます。

![image](https://i.gyazo.com/6416484adf11e2410f0e5e1042da7f53.png)

ペイロードがデータの内容です。数値に変更しましょう。

![image](https://i.gyazo.com/290cce9c52a9736289eb3317a8f18f36.png)

50に設定して完了をクリックします。

![image](https://i.gyazo.com/6d69e6990487e06533edba753d67904e.png)

デプロイボタンをクリックすると今作ったものが反映されます。

![image](https://i.gyazo.com/5ce3b9ec73285db932270eabfab4ac63.png)

動かして、inject ノードから送られるデータが 50 の数値になっているか確認します。

## この章のまとめ

![image](https://i.gyazo.com/9d560ea43e326e7f26b905d1771e60a2.png)

inject ノードと debug ノードのフローづくりを通じて Node-RED のファーストステップをお伝えしました。

## 次の章へ

* [猫画像 API からデータを取得してみよう](02_api_request.md)

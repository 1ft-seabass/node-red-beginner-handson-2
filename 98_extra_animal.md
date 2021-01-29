# エクストラ：ほかの動物画像APIをつないで画像表示してみよう

![image](https://i.gyazo.com/39c556b01dc1ed140a22606812df37fe.png)

猫画像が無事表示できました！

![image](https://i.gyazo.com/a54d6d373dd535b013a63a7844f65a58.png)

このように API は便利なものですが、

* イイ感じのAPIを見つける
* APIドキュメントで使い方を理解する
* APIからデータを受け取る
* データを取り出す
* データを使いやすく加工・計算・結合

といったが流れが必要にはなりますが、いきなりは大変なものです。いろいろなAPIをためして慣れていきましょう。

![image](https://i.gyazo.com/59ea49809c9cbc8c44c8baf2e4ef5197.png)

今日ご紹介したフリーのAPIのリストコレクション [public\-apis](https://github.com/public-apis/public-apis) は、面白くて使いやすい API が揃っていて見つけやすいですし、APIドキュメントも案内されているものも多く取っつきやすくしてくれていますのでおススメです。

![image](https://i.gyazo.com/09bc50e2b861135f3ab3c829b906f79d.png)

さらに、私が別の機会に書いた [すぐにAPIを体験！public\-apis 100以上のJavaScript axiosサンプル集](https://protoout.studio/posts/public-apis-api-get) を参考にしていただくと、そのなかでも今回使った Random Cat APIのように、すぐ試せるAPIがありますので、こちらも見てみてください。

この章では、2つほどほかの動物画像APIをつないで画像表示して和んでいきましょう。

## 柴犬画像API shibe.online につなげる

![image](https://i.gyazo.com/2715472e9939cb52cb676ce62254c594.png)

さきほどのフローをそのまま使います。

![image](https://i.gyazo.com/32ff4480a359ef26ad963ca5dc8380d0.png)

[public\-apis](https://github.com/public-apis/public-apis) から、同じ Animal 欄にある [shibe\.online \- the shiba inu api](https://shibe.online/) をつないで見ましょう。

![image](https://i.gyazo.com/7bf787b7aa8c9c3156168ac2d97cf263.png)

仕様は、GETリクエストで `http://shibe.online/api/shibes?count=[1-100]&urls=[true/false]&httpsUrls=[true/false]` といったURLでつなぎに行くと、柴犬画像が取得できるようです。

たとえばこんなURL http://shibe.online/api/shibes?count=3&urls=true&httpsUrls=true でアクセスしてみると、

![image](https://i.gyazo.com/8ad0bffc35d4f9a185fe1756560f92e1.png)

このように柴犬が画像URLが count=3 にしたので 3 個きました。

### http request ノードを変更

![image](https://i.gyazo.com/732068bddb9055646908395f5fa11a94.png)

http request ノードは、

* URL
  * `http://shibe.online/api/shibes?count=3&urls=true&httpsUrls=true` 

に変更します。

完了ボタンをクリックして、設定を完了します。

### debug ノードを準備する

ちゃんとデータが取れているか確かめるために http request ノードの後に debug ノードを入れてテストしてみましょう。

![image](https://i.gyazo.com/1d035b472a671a5bdf511ed356e7ef2d.png)

http request ノードと change ノードの間のつながりを削除しておきます。

![image](https://i.gyazo.com/323897f6f1117d90e55cb9d373952311.png)

http request ノードの後に、このように新しく debug ノードを加えてつなぎます。

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

![image](https://i.gyazo.com/5e031f94beebe181cce037bbf2343fcd.png)

inject ノードをクリックしてAPIにつないでみましょう。

![image](https://i.gyazo.com/0864603f1e35f74f31f9c87b9f4513f1.png)

柴犬の画像が無事届いていることが確認できます。このように途中途中で debug ノードを入れて検証する手法は Node-RED でよく使いますので、ご自分の開発時も思い出してぜひ使ってみてください。

### もう一度 change ノードにつなげる

検証が済んだので、もう一度 change ノードにつなげます。

![image](https://i.gyazo.com/31cb9abf7500608df61e7c9ecf715f61.png)

http request ノードと change ノードの間のつながりを再接続します。

### change ノードを変更

```json
[
"https://cdn.shibe.online/shibes/58b00681af055dcee1d3767b2968534e6de096ea.jpg",
"https://cdn.shibe.online/shibes/e89791fa1a9185984d0c5cd94ab1a0920ed818df.jpg",
"https://cdn.shibe.online/shibes/021010b053ed3c0cf7fd6aa42d9c7ed89701eda9.jpg"
]
```

たとえば、このようなデータを受け取っていることが分かりました。

![image](https://i.gyazo.com/ee34aa289d3b102cd13a56528eb9e8ee.png)

msg.payload の中に、配列（データの入る箱のようなもの）で、犬画像URLが3つ入っています。

![image](https://i.gyazo.com/ab36ee93aa38e6aa5f12b24c8a9cf4e0.png)

配列のデータは、数え方が 0 からはじまりますので change ノードに反映していきます。

![image](https://i.gyazo.com/61d2f06862173be4b23fda47a349cdc9.png)

change ノードは、

* 対象の値を
  * `payload[0]`

に変更します。

![image](https://i.gyazo.com/1b16892da77d329b1d7aab8d3aa27a3d.png)

このように `[` と `]` でくくった数字で payload の中の配列から取り出しています。（今回は 0 番目を取得）

完了ボタンをクリックして、設定を完了します。

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

![image](https://i.gyazo.com/48257c78501beaa31c251bdd47906a51.jpg)

inject ノードをクリックすると柴犬画像は表示されます！

![image](https://i.gyazo.com/71b364bd2ff2a50a32ab8948c8b648f9.png)

犬画像が無事表示できました！

## キツネ画像API RandomFox につなげる

![image](https://i.gyazo.com/6371c7fb080172532ef68c15899164f0.png)

もう一度、[public\-apis](https://github.com/public-apis/public-apis) に戻って今度は [Random Fox](https://randomfox.ca/floof/) というAPIにつなげてみましょう。

![image](https://i.gyazo.com/39a19d330338b69b0e77ed51f08c521f.png)

https://randomfox.ca/floof/ にアクセスすると、このように、image という値に狐画像が入っているようです。

### http request ノードを変更

![image](https://i.gyazo.com/cbc2d87e3b732fc67c3f39ebffcace25.png)

http request ノードは、

* URL
  * `https://randomfox.ca/floof/` 

に変更したら完了ボタンをクリックして、設定を完了します。

### change ノードを変更

![image](https://i.gyazo.com/ce59da80a83d251da4a3fda12573acf3.png)

change ノードは、

* 対象の値を
  * `payload.image`

に変更したら完了ボタンをクリックして、設定を完了します。

### 動かしてみる

![image](https://i.gyazo.com/2bdb7bcee8bd0219254ac90c8fe8e91f.png)

デプロイをクリックして Node-RED に設定を反映させます。

![image](https://i.gyazo.com/afe9927ec236a64f963ccbe30f29e923.png)

inject ノードをクリックするとキツネ画像は表示されます！

![image](https://i.gyazo.com/aa9bb51b75bef8760ffa918a3aa152b7.jpg)

キツネ画像が無事表示できました！

![image](https://i.gyazo.com/daa5747b5c91744d8460cd18ff7d1f83.png)

## この章のまとめ

![image](https://i.gyazo.com/9d560ea43e326e7f26b905d1771e60a2.png)

画像を表示するフローを元に、他の似たような動物画像 API につないで、新しい API を読み取ってデータを取り出したり、ドキュメントを見て読み解いたりする流れをお伝えしました。

## 次の章

[READMEに戻る](README.md)
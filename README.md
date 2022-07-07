# memo-app
  学習のための自主制作Webアプリケーション「NOTE」の開発ディレクトリです。( 制作者 🪴 ayuyama25 )

  Vueでフロントエンドを実装しています。

<div style="text-align: center;">

![ロゴ：NOTE](./src/assets/logo-note.png)

👉　https://memoapp-via-vue.netlify.app/

</div>

## 🌱 概要
  「NOTE」は、作成したメモに自分の好きなデコレーションを施し投稿できるメモアプリです。

  「好みのデザインにより近いメモを取れる」という趣味嗜好を叶えることで、より楽しく創作的なメモ体験を提供します。

  昔からあるスクラップブックやコラージュノートに着想を得て、好きなものを好きなように書き残せるという価値の再現に努めました。

## 🌞 機能一覧
  * 新規メモ投稿（テキスト、レート、背景テーマを設定）
  * メモの再編集、削除  
  * タイムライン表示
  * 表示順序の変更（投稿日時基準 or レート基準）
  * デフォルトのデザインの設定、変更

## 📗 使い方
<div style="text-align: center;">

![デモンストレーション](https://user-images.githubusercontent.com/89821806/177673218-00d4399d-d774-4292-b840-43bffbd39f54.gif)
</div>

  ***新規投稿***

  ロゴ画像付近の「Create New Memo!」ボタンを押すと新規メモの作成画面が開きます。テキストを入力して「Go!」を押すことで、作成したメモが投稿されます。

  ***編集・削除***

  タイムライン上のメモカードには、「Edit」ボタンと「x」ボタンが用意されています。「Edit」を押すと編集用、「x」では削除用のモーダルが開きます。

  ***レート・背景テーマ設定***

  メモの作成画面では、星の数で表現されるレートと背景デザインを個別に選択できます。それぞれに何も選択しない場合、レートは「0」、背景デザインはデフォルトが適用されます。

  ***タイムライン表示***

  メモカードが表示される順番は上から「投稿日時が近い順」あるいは「自ら設定したレートが高い順」のどちらかを選択でき、並べ替えることができます。

  ***デフォルトのデザイン設定***

  デフォルトで適用されるデザインは、設定画面から変更できます。

## 💡 使用言語・技術

<div style="padding: 1rem 2rem;">

 内容 | 言語・技術 
 -|- 
 フロントエンド | **Vue.js**@3.2.36 ( Vue/cli@5.0.4 ) , javascript 
 ビルド / デプロイ | **Netlify** 

  <div style="color: skyblue;">

  （バックエンドの学習後）データベースを構築してアカウント機能・メモの保管機能を追加予定です。現段階では、投稿されたメモはjavascriptオブジェクトとして格納されるため、ブラウザを更新されるとすべて初期化してしまいます。

  </div>
</div>

## 👍 特徴

**[ 機能 ]**

  データベースを持たず、メモオブジェクトに固有のidを持たせてnotes配列に格納し、v-forディレクティブで表示させています。
  削除や編集の際も、idを使って対象となるオブジェクトを特定します。

* id付与
  ~~~javascript
  // NewMemo.vue
  /* id付与 */
  props: {
    cards: Array
  },
  data(){
    return {
      idSerch: new Map(),
    }
  },
  methods: {
    getNewId(cardsData) {
      if (this.cards.length == 0 ) {
        return 0
      } else {
        this.idSerch = cardsData.map((card) => card.id)
        return Math.max(...this.idSerch) + 1
      }
    },
  },
  ~~~

* 削除
  ~~~javascript
  //HostPage.vue
  //templateタグ内
  <memo-cards @deletedId="getDeleted"></memo-cards>
  //scriptタグ内
  data() {
    return {
      notes: [
        {
        title: '',
        description: '',
        rating: 0,
        timestamp: new Date(),
        id: '0',
        themeColor: 'Default',
        },
      ],
      changeCard: {},
      targetIndex: '',
    }
  },
  methods: {
    /* 削除対象idからnotes配列の[index]を検索して削除実行 */
    getDeleted(value) {
      this.changeCard = value
      this.targetIndex = this.notes.map((card) => (card)).findIndex((card) => card.id === this.changeCard )
      this.notes.splice([this.targetIndex],1)
      this.targetIndex = null
      this.changeCard = null
      return this.notes
    },
  }
  ~~~

**[ 非機能 ]**

  「楽しく創作的なメモ体験」というコンセプトを反映しつつ操作性を高めるために、カラフルな色使いを採用し、ボタン周辺の動きを多く実装しました。

  レートの星付けや背景デザインの選択などでは、動的にCSSを付与できるように設定しています。
  
  * レートの星付け
  ~~~javascript
  //StarMemo.vue
  //templateタグ内
  <span v-for="(item, index) in starList" :key="index" @change="changingRate(item.value)">
    <label :class="item.color"><input type="radio" name="stars" v-model="starsOfRate" :value="item.value">★</label>
  </span>

  // scriptタグ内
  data() {
    return {
      starsOfRate: null,
      starList: [
        {value: 1, name: '1star', color: ''},
        {value: 2, name: '2star', color: ''},
        {value: 3, name: '3star', color: ''},
        {value: 4, name: '4star', color: ''},
        {value: 5, name: '5star', color: ''},
      ] 
    }
  },
  methods: {
    /* 選択したレートに応じて色をつける */
    colorStars(value) {
      for (let i=0; i<this.starList.length; i++) {
        this.starList[i].color = ''
      } 
      for (let i=0; i<value; i++) {
        this.starList[i].color = 'coloring-star'
      } return
    },
  }
  // style scapedタグ内（CSS）
  /* 選択したスターに付与する */
  .coloring-star{
    color: #c8ed7d;
  }
  ~~~

## 🌵 構成

<!-- 
### ▶︎ システム構成
 -->

### ▶︎ ディレクトリ構成

~~~javascript
  ├──node_modules
  ├──public
  │   ├index.html   // ←表示用HTML
  ├──src
  │   ├assets       // ←ロゴ画像を格納
  │   └components
  │     ├DeleteMemo.vue    // ←削除用ポップアップ画面
  │     ├EditMemo.vue      // ←編集画面
  │     ├MemoCards.vue     // ←表示部分(タイムライン)
  │     ├NewMemo.vue       // ←新規作成画面
  │     ├SetTheme.vue      // ←テーマ選択用の部品
  │     ├SetupPage.vue     // ←設定画面
  │     └StarMemo.vue      // ←レート選択用の部品
  ├──HostPage.vue  // ←ルートコンポーネント
  ├──main.js       // ←メインファイル
  ├──package.json
  ├──README.md
~~~

### ▶︎ データベース（テーブル）
  HostPage.vueのdata内にあるnotesオブジェクトが作成後のメモデータを保持しています。

~~~javascript
notes: [
  {
    title: '',    // タイトル
    description: '',  // 詳細
    rating: 0,    // 重要度のレート、星の数
    timestamp: new Date(),  // 初回投稿日時
    id: '0',    // メモのID番号
    themeColor: 'Default',  // 背景テーマ設定
  }, 
]
~~~

## 📝 使用またはインストール

### ▶︎ 試用URL

<div style="text-align: center; padding: 1rem;">

  👉 https://memoapp-via-vue.netlify.app/

</div>

### ▶︎ インストールする場合
```
Gitリポジトリのコピー
$ git clone https://github.com/ayuyama25/memo-app.git

ローカル環境でのデプロイ
$ cd memoapp
$ npm install
$ vue serve
  👉 http://localhost:8080
```

実行環境

  * Node.js@v16.13.0
  * npm@8.1.0
  * @vue/cli@5.0.4
  * webpack@5.72.1

## 🫰 規約・免責
* 公序良俗に反する利用は、いかなる場合であっても禁止します。
* 当ソフトの利用にあたって何らかの不具合やトラブルが生じたとしても、制作者側は一切の責任を負いません。

## 🔖 更新履歴
<div style="padding: 0 2rem;">

2022/6/16 公開 (ver.0.1.0)

2022/6/22 タイムライン表示のバグを修正（ver.0.1.1）

2022/6/28 テーマ編集機能を追加（ver.0.2.0）

2022/6/30 テーマ編集機能のバグを修正（ver.0.2.1）

2022/7/1  デザインを修正（ver.1.0.0）

<!--  TODO 確認、整備 -->
</div>

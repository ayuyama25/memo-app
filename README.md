# memo-app
  学習のための自主制作アプリ「NOTE」の開発ディレクトリです。

  NOTEは、作成したメモに自分の好きなデコレーションを施し投稿できるWebアプリケーションです。

<div style="text-align: center; padding: 1rem;">

![ロゴ：NOTE](./src/assets/logo-note.png)

👉　https://memoapp-via-vue.netlify.app/

( 制作者 🪴 ayuyama25 )

</div>

## 🌞 機能一覧
  * 新規メモ投稿
  * メモの再編集、削除  
  * タイムライン表示
  * 表示順序の変更（投稿日時基準 or レート基準）
  * デフォルトのデザインの設定、変更

<div style="text-align: center; padding: 1rem;">
<!--   ＜デモンストレーション＞ -->
<!-- TODO: 使い方の追加 -->

</div>

## 💡 使用言語・技術

<div style="padding: 0 2rem;">

| | |
|-|-|
| フロントエンド | **Vue.js** , javascript |
| バックエンド | なし [^補足] |
| デプロイサーバー | **Netilfy** 

[^補足]: （バックエンドの学習後、データベースを構築してアカウント機能・メモの保管機能を追加予定です。今のところ投稿されたメモはjavascriptオブジェクトとして格納されるため、ブラウザを更新されるとすべて初期化してしまいます。）

</div>

## 🌵 構成

### ▶︎ システム構成


### ▶︎ リポジトリ構成
  main.jsがメインファイルです。
<!-- HostPage.vueがappをマウントしている・・？？ -->
<!-- TODO 確認 -->

### ▶︎ データベース（テーブル）
  現状はHostPage.vueのdata内にあるnotesオブジェクトが作成後のメモデータを保持しています。
```
notes: [
  {
    title: '',    // タイトル
    description: '',  // 詳細
    rating: 0,    // 重要度のレート
    timestamp: new Date(),  // 投稿日時
    id: '0',    // メモのID番号
    themeColor: 'Default',  // デザイン設定
  }, 
]
```

## 📝 使用またはインストール

（配布形態：フリーウェア）

### ▶︎ テスト使用URL

<div style="text-align: center; padding: 1rem;">

  👉 https://memoapp-via-vue.netlify.app/

</div>

### ▶︎ インストールする場合
```
インストール
$ git clone ...

ローカル環境でのデプロイ
$ cd memoapp
$ vue serve
  👉 http://localhost:8080
```
<!-- コマンド要確認、修正 -->

## 🫰 規約・免責
* 素材データ等の流用、転載、二次配布等を禁止します。
* 公序良俗に反する利用は、いかなる場合であっても禁止します。
* 当ソフトの利用にあたって何らかの不具合やトラブルが生じたとしても、制作者側は一切の責任を負いません。自己責任でご利用ください。

## 🔖 更新履歴
<div style="padding: 0 2rem;">

2022/6/16 公開 (ver.0.1.0)

2022/6/22 タイムライン表示のバグを修正（ver.0.1.1）

2022/6/28 テーマ編集機能を追加（ver.0.2.0）

2022/6/30 テーマ編集機能のバグを修正（ver.0.2.1）

2022/7/1  デザインを修正（ver.1.0.0）

<!--  TODO 確認、整備 -->
</div>

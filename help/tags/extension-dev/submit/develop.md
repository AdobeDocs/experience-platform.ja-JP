---
title: 拡張機能の開発
description: このドキュメントでは、タグ拡張機能開発プロセスの一般的な概要と、詳細なプロセスに関するその他のドキュメントへのリンクを示します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 96%

---

# 拡張機能の開発

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

タグ拡張機能は、独自の要件を持つ（小規模の）製品として考える必要があります。 Adobe Experience Platform のユーザーが拡張機能をどのように使用するかを把握すると、拡張機能で提供する必要があるイベント、条件、アクション、データ要素の各タイプに基づいて機能を分類しやすくなります。

この知識により、拡張機能で提供するコンポーネントを計画できます。

## ガイド

計画を立てることで、以下のガイドが拡張機能の開発プロセスを理解するのに役立ちます。

* 左側のナビゲーションの&#x200B;**拡張機能の開発**&#x200B;の下にある[はじめる前に](../getting-started.md)とその他のドキュメントは、拡張機能を理解するための参考資料です。 これには、拡張機能の機能、ユーザー情報の保存および拡張機能と Adobe Experience Platform との間での受け渡し方法、コードのライブラリへのバンドル方法、拡張機能コードが実行時にブラウザーで解釈および使用される方法に関する詳細が含まれます。
* [拡張機能チュートリアルビデオ](https://youtu.be/rxjtC9o4rl0)から始めるとよいでしょう。
* YouTube プレイリストの「[拡張機能の概要](https://www.youtube.com/playlist?list=PLOdw8u2F8CIgynzKrPEwCPuDxzHW1WP5m)」では、拡張機能パッケージの作成プロセスを順を追って説明しています。
* 記事「[JSON スキーマの理解](https://spacetelescope.github.io/understanding-json-schema/index.html#)」を参照してください。
* 「[JSON Lint/Validator](http://jsonlint.com/)」を参照してください。。
* [JSON Viewer](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) Chrome 拡張機能を使用して、JSON および JSONP をハイライト表示および印刷してください。
* [jsonschema.net](https://jsonschema.net/#/editor) エディターは、オブジェクトから JSON スキーマを作成するのに役立ちます。。
* [JSON スキーマバリデータ](http://www.jsonschemavalidator.net/) は、オンラインのインタラクティブな JSON スキーマ検証ツールです。

## ツール

拡張機能パッケージの開発に役立つ npm ツールも多数用意されています。

* [タグ拡張機能基礎ツール](https://www.npmjs.com/package/@adobe/reactor-scaffold) を使用すると、ローカルマシン上にスタータープロジェクトを簡単に作成できます。
* [タグ拡張機能サンドボックス](https://www.npmjs.com/package/@adobe/reactor-sandbox)は、ローカルマシン上の拡張機能のビューやモジュールを検証するのに役立ちます。
* [タグ拡張機能パッケージャー](https://www.npmjs.com/package/@adobe/reactor-packager)は、タグ拡張機能を zip ファイルにパッケージ化するためのコマンドラインユーティリティです。
* [タグ拡張機能アップローダ](https://www.npmjs.com/package/@adobe/reactor-uploader)は、テクニカルアカウントの資格情報を入力し、拡張機能パッケージをタグにアップロードするのに役立つ、インタラクティブなコマンドラインツールです。
* [タグ拡張機能リリーサー](https://www.npmjs.com/package/@adobe/reactor-releaser)は、拡張機能をプライベートで利用できるようにするインタラクティブなコマンドラインツールです。

## 拡張機能の例

GitHub には、スタータープロジェクトとしてレビューまたは使用できる拡張機能の例があります。

* [Hello World サンプル拡張機能](https://github.com/adobe/reactor-helloworld-extension)
* [Facebook サンプル拡張機能](https://github.com/Adobe-Marketing-Cloud-Activation/extension-facebookpixel)
* [Typekit サンプル拡張機能](https://github.com/jeffchasin/extension-typekit)
* [Pinterest サンプル拡張機能](https://github.com/jeffchasin/extension-pinterest)

## Slack ワークスペース

Slack コミュニティワークスペースへのアクセスをリクエストできます。このワークスペースでは、拡張機能の作成者は、この[リクエストフォーム](http://join.launchdevelopers.chat)を使用して相互にサポートできます。

**注意**：この Slack ワークスペースにはアドビシステムズのメンバーも参加していますが、このコミュニティリソースはアドビのサポートや管理の対象ではありません。

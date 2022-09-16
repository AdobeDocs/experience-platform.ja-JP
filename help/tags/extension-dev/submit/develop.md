---
title: 拡張機能の開発
description: このドキュメントでは、タグ拡張機能開発プロセスの一般的な概要と、詳細なプロセスに関するその他のドキュメントへのリンクを示します。
exl-id: fb2f7275-a5da-4a41-b915-822c71c02e5c
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 100%

---

# 拡張機能の開発

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

拡張機能は、独自の要件を持つ（小規模の）製品として考える必要があります。 Adobe Experience Platform のユーザーが拡張機能をどのように使用するかを把握すると、拡張機能で提供する必要があるイベント、条件、アクション、データ要素の各タイプに基づいて機能を分類しやすくなります。

この知識があれば、拡張機能で提供するコンポーネントを計画できます。

## ガイド

計画を立てることで、以下のガイドが拡張機能の開発プロセスを理解するのに役立ちます。

* 左側のナビゲーションの [はじめる前に](../getting-started.md) と **拡張機能の開発** の下にあるその他のドキュメントは、拡張機能を理解するための参考資料です。 これには、拡張機能ができること、拡張機能と Adobe Experience Platform 間でのユーザー情報の保存および受け渡し方法、コードのライブラリへのバンドル方法、拡張機能コードが実行時にブラウザーで解釈および使用される方法に関する詳細が含まれます。
* [拡張機能チュートリアルビデオ](https://youtu.be/rxjtC9o4rl0) は、仕様を開始する際に非常に役立ちます。
* YouTube プレイリストの「[拡張機能の概要](https://www.youtube.com/playlist?list=PLOdw8u2F8CIgynzKrPEwCPuDxzHW1WP5m)」では、拡張機能パッケージの作成プロセスを順を追って説明しています。
* 記事「[JSON スキーマの理解](https://spacetelescope.github.io/understanding-json-schema/index.html#)」を参照してください。
* 「[JSON Lint/Validator](https://jsonlint.com/)」を参照してください。。
* [JSON Viewer](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) Chrome 拡張機能を使用して、JSON および JSONP をハイライト表示および印刷してください。
* [jsonschema.net](https://jsonschema.net/#/editor) エディターは、オブジェクトから JSON スキーマを作成するのに役立ちます。
* [JSON スキーマバリデータ](https://www.jsonschemavalidator.net) は、オンラインのインタラクティブな JSON スキーマ検証ツールです。

## ツール

拡張機能パッケージの開発に役立つ npm ツールも多数用意されています。

* [タグ拡張スキャフォールドツール](https://www.npmjs.com/package/@adobe/reactor-scaffold) を使用すると、ローカルマシン上にスタータープロジェクトを簡単に作成できます。
* [タグ拡張サンドボックス](https://www.npmjs.com/package/@adobe/reactor-sandbox) は、ローカルマシン上の拡張機能のビューとモジュールを検証するのに役立ちます。
* [タグ拡張パッケージャー](https://www.npmjs.com/package/@adobe/reactor-packager) は、タグ拡張機能を zip ファイルにパッケージ化するためのコマンドラインユーティリティです。
* [タグ拡張アップローダ](https://www.npmjs.com/package/@adobe/reactor-uploader)は、テクニカルアカウントの資格情報を入力し、拡張機能パッケージをタグにアップロードするのに役立つインタラクティブなコマンドラインツールです。
* [タグ拡張リリース](https://www.npmjs.com/package/@adobe/reactor-releaser)は、拡張機能をプライベートで利用できるようにするインタラクティブなコマンドラインツールです。

## 拡張機能の例

GitHub には、スタータープロジェクトとしてレビューまたは使用できる拡張機能の例があります。

* [Hello World サンプル拡張機能](https://github.com/adobe/reactor-helloworld-extension)
* [Facebook サンプル拡張機能](https://github.com/Adobe-Marketing-Cloud-Activation/extension-facebookpixel)
* [Typekit サンプル拡張機能](https://github.com/jeffchasin/extension-typekit)
* [Pinterest サンプル拡張機能](https://github.com/jeffchasin/extension-pinterest)

## Slack ワークスペース

Slack コミュニティワークスペースへのアクセスをリクエストできます。このワークスペースでは、拡張機能の作成者はこの[リクエストフォーム](https://docs.google.com/forms/d/e/1FAIpQLScq1m63YkDrRpvPLhzUqtfoleWiDDTTXZsSivIXRfFdlSMzpQ/viewform)を使用して相互にサポートできます。

**注意**：この Slack ワークスペースにはアドビのメンバーも所属していますが、このワークスペースは、アドビが後援や管理をおこなっていないコミュニティリソースです。

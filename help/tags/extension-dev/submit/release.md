---
title: 拡張機能のリリース
description: Adobe Experience Platform でタグ拡張機能を非公開または公開でリリースする方法について説明します。
exl-id: a5eb6902-4b0f-4717-a431-a290c50fb5a6
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 64%

---

# 拡張機能のリリース

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

テストとドキュメント化が完了すると、拡張機能をリリースする準備が整います。現在、2 種類のリリースを実行できます。

- **非公開リリース**：完成した拡張機能は、社内のすべてのプロパティで使用できますが、Adobe Experience Platform の他の会社では使用できません。
- **公開リリース**：完成した拡張機能は、すべての Adobe Experience Platform ユーザー向けの公開 Marketplace で利用できます。

>[!NOTE]
>
>拡張機能をリリースすると、変更を加えることができなくなり、リリースを停止することはできません。リリース後、バグの修正と機能の追加をおこなうには、新しいバージョンの拡張機能パッケージを `POST`し、その新しいバージョンで前述のテストとリリースの手順に従います。

拡張機能を公開する前に、まずプライベート拡張機能としてリリースする必要があります。

## 非公開リリース

拡張機能を非公開でリリースする最も簡単な方法は、[tag extension releaser](https://www.npmjs.com/package/@adobe/reactor-releaser) を使用することです。

```bash
npx @adobe/reactor-releaser
```

`npx`：npm パッケージをダウンロードして実行できます。実際にはこのパッケージをコンピューターにインストールする必要はありません。これは、リリーサーを実行する最も簡単な方法です。

>[!NOTE]
> デフォルトでは、リリーサーは、サーバー間 Oauth フローについてAdobe I/Oの資格情報が必要であることを前提としています。 従来の `jwt-auth` 資格情報
> > 2025 年 1 月 1 日（PT）に廃止されるまで `npx @adobe/reactor-releaser@v3.1.3` を実行して使用できます。 必要なパラメーター
> > `jwt-auth` のバージョンを実行するには、[ こちら ](https://github.com/adobe/reactor-releaser/tree/9ea66aa2c683fe7da0cca50ff5c9b9372f183bb5) を参照してください。

リリーサーでは、いくつかの情報のみを入力する必要があります。 `clientId` と `clientSecret` は、Adobe I/O コンソールから取得できます。 I/O コンソールの[統合ページ](https://console.adobe.io/integrations)に移動します。ドロップダウンから正しい組織を選択し、適切な統合を見つけて「**[!UICONTROL View]**」を選択します。

- あなたの `clientId` は何ですか。 これを I/O コンソールからコピーして貼り付けます。
- あなたの `clientSecret` は何ですか。 これを I/O コンソールからコピーして貼り付けます。

リリーサーは、拡張機能マニフェストから `name` フィールドと `platform` フィールドを読み取り、開発可用性で一致する拡張機能パッケージの API をクエリします。
次に、リリース担当者は、プライベートで利用できるようにする正しい拡張機能パッケージが見つかったことを確認するように求めます。

API を直接使用して、非公開で拡張機能をリリースする場合は、 [拡張機能パッケージを非公開でリリース](/help/tags/api/endpoints/extension-packages.md#private-release) するための呼び出し例の詳細を API ドキュメントで参照してください。

## 公開リリース

非公開リリースが完了したら、公開するよう Adobe に依頼できます。これにより、拡張機能が公開カタログで使用できるようになります。データ収集ユーザーは誰でも、拡張機能を任意のプロパティにインストールできます。

リリースプロセスを開始するには、 [公開リリースリクエストフォーム](https://www.feedbackprogram.adobe.com/c/r/DCExtensionReleaseRequest) に記入してください。

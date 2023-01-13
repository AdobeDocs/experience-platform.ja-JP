---
title: 拡張機能のリリース
description: Adobe Experience Platform でタグ拡張機能を非公開または公開でリリースする方法について説明します。
exl-id: a5eb6902-4b0f-4717-a431-a290c50fb5a6
source-git-commit: 8862a911f09d47c3a2260faba045f3c79826b52c
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 98%

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

拡張機能を公開してリリースするには、拡張機能をプライベート拡張機能としてリリースする必要があります。

## 非公開リリース

非公開で拡張機能をリリースする最も簡単な方法は、[tag extension releaser](https://www.npmjs.com/package/@adobe/reactor-releaser) を使用することです。詳しい手順については、ドキュメントを参照してください。

API を直接使用して、非公開で拡張機能をリリースする場合は、 [拡張機能パッケージを非公開でリリース](https://developer.adobelaunch.com/api/reference/1.0/extension_packages/release_private/) するための呼び出し例の詳細を API ドキュメントで参照してください。

## 公開リリース

非公開リリースが完了したら、公開するよう Adobe に依頼できます。これにより、拡張機能が公開カタログで使用できるようになります。データ収集ユーザーは誰でも、拡張機能を任意のプロパティにインストールできます。

リリースプロセスを開始するには、 [公開リリースリクエストフォーム](https://experiencecloudpanel.adobe.com/c/r/DCExtensionReleaseRequest) に記入してください。

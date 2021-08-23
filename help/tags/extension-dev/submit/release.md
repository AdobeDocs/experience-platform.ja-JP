---
title: 拡張機能のリリース
description: Adobe Experience Platform でタグ拡張機能を非公開または公開でリリースする方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 94%

---

# 拡張機能のリリース

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

テストとドキュメント化が完了すると、拡張機能をリリースする準備が整います。現在、2 種類のリリースを実行できます。

- **非公開リリース**：完成した拡張機能は、会社内のすべてのプロパティで使用できますが、Adobe Experience Platform の他の会社では使用できません。
- **公開リリース**：完成した拡張機能は、すべての Adobe Experience Platform ユーザー向けの公開 Marketplace で利用できます。

>[!NOTE]
>
>拡張機能をリリースすると、変更やリリースの解除はできなくなります。リリース後、バグの修正と機能の追加をおこなうには、新しいバージョンの拡張機能パッケージを `POST` し、その新しいバージョンで前述のテストとリリースの手順に従います。

拡張機能を公開してリリースするには、拡張機能をプライベート拡張機能としてリリースする必要があります。

## 非公開リリース

拡張機能を非公開でリリースする最も簡単な方法は、[タグ拡張機能リリーサー](https://www.npmjs.com/package/@adobe/reactor-releaser)を使用することです。詳しい手順については、ドキュメントを参照してください。

API を直接使用して拡張機能を非公開としてリリースする場合は、[拡張機能パッケージを非公開でリリースする](https://developer.adobelaunch.com/api/reference/1.0/extension_packages/release_private/)ための呼び出し例の詳細を API ドキュメントで参照してください。

## 公開リリース

非公開リリースが完了したら、公開するよう Adobe に依頼できます。これにより、拡張機能が公開カタログで使用できるようになります。データ収集 UI ユーザーは、誰でも拡張機能を任意のプロパティにインストールできます。

リリースプロセスを開始するには、[公開リリースリクエストフォーム](https://adobe.allegiancetech.com/cgi-bin/qwebcorporate.dll?idx=7DRB5U)に記入してください。

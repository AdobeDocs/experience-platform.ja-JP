---
title: オーサリングのベストプラクティス
description: 宛先のドキュメントページをオーサリングする際に従う必要があるルールとヒントを説明します。これにより、Adobe Experience Platformのドキュメント品質標準を確実に満たすことができます。
source-git-commit: 35e5c388e9572a3b27ec4bce55e766725936eda4
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 2%

---

# オーサリングのベストプラクティス

## 概要 {#overview}

このページでは、次の場合に従う必要があるルールについて説明します [宛先ドキュメントのオーサリング](./documentation-instructions.md) ページを開き、Adobe Experience Platformのドキュメント品質標準を満たしていることを確認します。

## 一般的なガイダンス {#general-guidance}

* を [テンプレート](./self-service-template.md) 宛先のドキュメントについては、『Adobe投稿者ガイド』を参照してください。 [リンク](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en), [テーブル](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en#tables)、 [サポートされる markdown 構文](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en), [記述指針](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=en)など。
* 製品ドキュメントには、観測と推定を含めないでください。
* Experience Platformドキュメントでは、Adobeライターは **太字書式** 次のように、ユーザーインターフェイスコントロールを参照するには、次の手順を実行します。
   * に移動します。 **[!UICONTROL 接続]** > **[!UICONTROL 宛先]**&#x200B;をクリックし、 **[!UICONTROL カタログ]** タブをクリックします。 ユーザーインターフェイスコントロールが [宛先のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=en#select-destination).

## リンク {#linking}

提供されているドキュメントテンプレートに従ってください。テンプレート内の既存のリンクは編集しないでください。 新しいリンクを含める場合は、 [ドキュメント内でのリンクの使用](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=en) （コントリビューターガイド）。

## ブランディングのガイドライン {#branding}

* AEP は、公開用語として承認されていません。 最初の使用時にAdobe Experience Platformを使用し、次にExperience Platformを使用し、次に Platform を使用してください。
   * **次を使用しない**:AEP から宛先にデータを書き出す前に、次の前提条件を読み、満たしていることを確認してください。
   * **用途**:Adobe Experience Platformから宛先にデータを書き出す前に、次の前提条件を読み、満たしていることを確認してください。

## 画像とスクリーンショット {#images-and-screenshots}

* 詳しくは、 [画像へのリンク方法](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en#images)（コントリビューターガイドを参照）。
* スクリーンショットを使用する場合は、スクリーンショットに Platform UI の画面全体が取り込まれていることを確認してください。
* ページ上の特定のコントロールまたはラベルをハイライト表示するために画像をマークアップする場合は、Experience Platformドキュメントチームが使用するマークアップスタイルに従ってください。 プロファイルベースが [このスクリーンショット](/help/destinations/catalog/cloud-storage/amazon-s3.md#export-type-frequency).
* 以下を使用してください： `png` 画像の形式設定
* 番号付きスクリーンショットはファイル名として使用しないでください。 画像ファイル名は説明的である必要があります。
   * **次を使用しない**: `1.png`, `2.png`, `3.png`
   * **使用方法**: `yourdestination-authentication-details.png`, `yourdestination-destination-details.png`
* ドキュメントに追加する画像に代替テキストを使用し、代替テキストに適切な文法を使用してください。
   * **次を使用しない**:宛先接続の詳細
   * **用途**:宛先接続の詳細を示す、Platform UI の画像が入力されました。

## プロセス {#process}

* この [ドキュメントテンプレート](./self-service-template.md) は、パートナーのフィードバックに基づいて、まれに更新されます。 宛先のオーサリングドキュメントを開始する前に、 [テンプレートの最新バージョン](/help/destinations/destination-sdk/docs-framework/assets/yourdestination-template.zip).
* ドキュメントを作成し、フォークのブランチからドキュメントプル要求 (PR) を作成します *本枝以外の*. でのオーサリング時に、レビュー用の送信先を参照してください。 [GitHub インターフェイス](./use-github-interface-to-create-documentation.md#submit-review) または [ローカル環境](./work-in-local-environment.md#submit-review).
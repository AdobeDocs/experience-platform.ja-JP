---
title: オーサリングのベストプラクティス
description: Adobe Experience Platform ドキュメント品質基準を確実に満たすために、宛先ドキュメントページをオーサリングする際に従うべきルールおよびヒントについて説明します。
exl-id: b12059f1-6635-41cd-acc5-6ff471111164
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 89%

---

# オーサリングのベストプラクティス

## 概要 {#overview}

このページでは、Adobe Experience Platform ドキュメント品質基準を確実に満たすために、[宛先ドキュメントページをオーサリング](./documentation-instructions.md)する際に従うべきルールについて説明します。

## 一般的なガイダンス {#general-guidance}

* 宛先ドキュメント用に[テンプレート](./self-service-template.md)に入力する場合、[リンク](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html)、[テーブル](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html#tables)、[サポートされるマークダウン構文](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html)、[ライティングガイダンス](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html)などについて詳しくは、アドビコントリビューターガイドを参照してください。
* 製品ドキュメントに所見や推定を含めないでください。
* Experience Platform ドキュメントでは、アドビのライターは、ユーザーインターフェイスコントロールを参照するために、以下のように、**太字書式**&#x200B;を使用しています。
   * **[!UICONTROL 接続]**／**[!UICONTROL 宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。ユーザーインターフェイスコントロールの表記方法の例を表示するには、[宛先チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html#select-destination)を参照してください。

## ライティングスタイル

>[!IMPORTANT]
>
>宛先ドキュメントページのオーサリングを開始する前に、[アドビドキュメントのライティングガイダンス](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html)を参照してください。

* 文を短くして、要点を早く伝えます。文が 20 単語を超える長さだったり、複数のコンマを使用している場合は、別の文に分割することを検討してください。20 単語を超える長さの文は、読者にとって特に読みにくい可能性があります。
* 過剰に丁寧な表現をしないようにします。テクニカルドキュメントでは、「お願いいたします」や「...していただく」などの使用は避けます。

## リンク {#linking}

提供されるドキュメントテンプレートに従い、テンプレート内の既存のリンクを編集しないようにします。新しいリンクを含める場合は、コントリビューターガイドの[ドキュメントでのリンクの使用](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html)を参照してください。

## ブランディングガイドライン {#branding}

* AEP は、承認された、一般向けの用語ではありません。最初の使用時には Adobe Experience Platform を使用し、次に Experience Platform、その次に Platform を使用してください。
   * **使用不可**：AEP から宛先にデータを書き出す前に、以下の前提条件を参照し、必ず完了するようにしてください。
   * **使用可**：Adobe Experience Platform から宛先にデータを書き出す前に、以下の前提条件を参照し、必ず完了するようにしてください。

## 画像とスクリーンショット {#images-and-screenshots}

* [画像へのリンク方法](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html#images)について詳しくは、コントリビューターガイドを参照してください。
* スクリーンショットを使用する場合、スクリーンショットが Platform UI 画面全体をキャプチャしていることを確認してください。
* ページ上の特定のコントロールやラベルをハイライト表示するために画像をマークアップする場合、Experience Platform ドキュメントチームによって使用されるマークアップスタイルに従ってください。[このスクリーンショット](/help/destinations/catalog/cloud-storage/amazon-s3.md#export-type-frequency)で「Profile-based」がどのようにハイライト表示されているかを確認してください。
* `png` 形式の画像を使用してください。
* 番号が付けられたスクリーンショットをファイル名として使用しないでください。画像ファイル名は、説明的である必要があります。
   * **使用不可**：`1.png`、`2.png`、`3.png`
   * **使用可**：`yourdestination-authentication-details.png`、`yourdestination-destination-details.png`
* ドキュメントに追加するすべての画像に alt テキストを使用し、alt テキストに適した文法を使用してください。
   * **使用不可**：宛先接続の詳細
   * **使用可**：宛先接続の詳細が入力されていることを表す、Platform UI の画像。

## プロセス {#process}

* [ドキュメントテンプレート](./self-service-template.md)は、パートナーのフィードバックに基づいて、まれに更新されます。宛先用のドキュメントのオーサリングを開始する前に、[テンプレートの最新バージョン](../assets/docs-framework/yourdestination-template.zip)をダウンロードしていることを確認してください。
* ドキュメントをオーサリングして、*メインブランチ以外*&#x200B;のフォーク内のブランチからドキュメントのプルリクエスト（PR）を作成します。[GitHub インターフェイス](./use-github-interface-to-create-documentation.md#submit-review)や[ローカル環境](./work-in-local-environment.md#submit-review)でオーサリングする場合は、レビューのための宛先の送信に関する節を参照してください。

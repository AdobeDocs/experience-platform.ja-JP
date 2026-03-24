---
title: オーサリングのベストプラクティス
description: Adobe Experience Platform ドキュメント品質基準を確実に満たすために、宛先ドキュメントページをオーサリングする際に従うべきルールおよびヒントについて説明します。
exl-id: b12059f1-6635-41cd-acc5-6ff471111164
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 81%

---

# オーサリングのベストプラクティス

## 概要 {#overview}

このページでは、宛先ドキュメント [&#x200B; ページを](./documentation-instructions.md) オーサリングする際に従う必要があるルールについて説明し、[!DNL Adobe Experience Platform] ドキュメントの品質基準を満たしていることを確認します。

## 一般的なガイダンス {#general-guidance}

* 宛先ドキュメント用に[テンプレート](./self-service-template.md)に入力する場合、[リンク](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=ja)、[テーブル](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja#tables)、[サポートされるマークダウン構文](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja)、[ライティングガイダンス](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=ja)などについて詳しくは、アドビコントリビューターガイドを参照してください。
* 製品ドキュメントに所見や推定を含めないでください。
* Experience Platform ドキュメントでは、アドビのライターは、ユーザーインターフェイスコントロールを参照するために、以下のように、**太字書式**&#x200B;を使用しています。
   * **[!UICONTROL Connections]** > **[!UICONTROL Destinations]**&#x200B;に移動し、「**[!UICONTROL Catalog]**」タブを選択します。 ユーザーインターフェイスコントロールの表記方法の例を表示するには、[宛先チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja#select-destination)を参照してください。

## 書き込みのスタイル {#writing-style}

>[!IMPORTANT]
>
>宛先ドキュメントページのオーサリングを開始する前に、[アドビドキュメントのライティングガイダンス](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=ja)を参照してください。

* 文を短くして、要点を早く伝えます。文が 20 単語を超える長さだったり、複数のコンマを使用している場合は、別の文に分割することを検討してください。20 単語を超える長さの文は、読者にとって特に読みにくい可能性があります。
* 過剰に丁寧な表現をしないようにします。テクニカルドキュメントでは、「お願いいたします」や「...していただく」などの使用は避けます。

## リンク {#linking}

提供されるドキュメントテンプレートに従い、テンプレート内の既存のリンクを編集しないようにします。新しいリンクを含める場合は、コントリビューターガイドの[ドキュメントでのリンクの使用](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/linking.html?lang=ja)を参照してください。

## ブランディングガイドライン {#branding}

* AEP は、承認された、一般向けの用語ではありません。[!DNL Adobe Experience Platform]を最初に使用し、次にExperience Platform、次にExperience Platformを使用してください。
   * **使用不可**：AEP から宛先にデータを書き出す前に、以下の前提条件を参照し、必ず完了するようにしてください。
   * **使用**: [!DNL Adobe Experience Platform]からYourDestinationにデータを書き出す前に、これらの前提条件を読んで完了してください。

## 画像とスクリーンショット {#images-and-screenshots}

* [画像へのリンク方法](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja#images)について詳しくは、コントリビューターガイドを参照してください。
* スクリーンショットを使用する場合は、スクリーンショットがExperience Platform UI画面全体をキャプチャしていることを確認します。
* ページ上の特定のコントロールやラベルをハイライト表示するために画像をマークアップする場合、Experience Platform ドキュメントチームによって使用されるマークアップスタイルに従ってください。[このスクリーンショット](/help/destinations/catalog/cloud-storage/amazon-s3.md#export-type-frequency)で「Profile-based」がどのようにハイライト表示されているかを確認してください。
* `png` 形式の画像を使用してください。
* 番号が付けられたスクリーンショットをファイル名として使用しないでください。画像ファイル名は、説明的である必要があります。
   * **使用不可**：`1.png`、`2.png`、`3.png`
   * **使用可**：`yourdestination-authentication-details.png`、`yourdestination-destination-details.png`
* ドキュメントに追加するすべての画像に alt テキストを使用し、alt テキストに適した文法を使用してください。
   * **使用不可**：宛先接続の詳細
   * **使用**：宛先の接続の詳細が入力されたExperience Platform UIの画像。

## プロセス {#process}

* [ドキュメントテンプレート](./self-service-template.md)は、パートナーのフィードバックに基づいて、まれに更新されます。宛先用のドキュメントのオーサリングを開始する前に、[テンプレートの最新バージョン](../assets/docs-framework/yourdestination-template.zip)をダウンロードしていることを確認してください。
* ドキュメントをオーサリングして、*メインブランチ以外*&#x200B;のフォーク内のブランチからドキュメントのプルリクエスト（PR）を作成します。[GitHub インターフェイス](./use-github-interface-to-create-documentation.md#submit-review)や[ローカル環境](./work-in-local-environment.md#submit-review)でオーサリングする場合は、レビューのための宛先の送信に関する節を参照してください。

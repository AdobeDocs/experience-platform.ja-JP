---
title: Adobe Experience Platform の宛先のドキュメント化
description: Adobe Experience Platformで宛先のドキュメントページを作成する手順
exl-id: 6cc9c758-44bb-463b-941a-06b1a22ee8f3
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 7%

---

# 宛先を[!DNL Adobe Experience Platform]に文書化します

>[!IMPORTANT]
>
>ここに記載されているプロセスは、製品で構成された（パブリックな）宛先を送信するパートナーにのみ必要です。 自分で使用するプライベート宛先を作成する場合は、宛先のドキュメントを作成して公開する必要はありません。

## 概要 {#overview}

[!DNL Adobe Experience Platform]へようこそ、ここにあなたがいらっしゃって嬉しいです！
宛先を文書化することは、[!DNL Adobe Experience Platform]でライブ設定する前の最後の手順です。

このドキュメントセクションには、以下が含まれます。

* 新しい宛先のドキュメントページを作成するための手順ごとの手順
* あなたの目的地のために記入するためのテンプレート。
* [Markdown](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja)の使用に関する一般的な手順
* [Adobe Markdown フレーバーに関する具体的な手順](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja#custom-markdown-extensions) （Adobe Markdown フレーバーは通常のMarkdownとよく似ています）。
* Experience Platform ドキュメントの品質基準を満たす、宛先ページのドキュメントページの作成に役立つ[&#x200B; ベストプラクティスページ &#x200B;](./authoring-best-practices.md)です。

## 前提条件 {#prerequisites}

この記事の手順に従って、宛先のドキュメントを作成するには、次の項目が必要です。

* **GitHub アカウント**。 まだアカウントをお持ちでない場合は、[GitHub](https://github.com/)にサインアップしてください。
* **GitHub Desktop**。 ローカル環境[で](./work-in-local-environment.md) ドキュメントを作成することを選択した場合は、[GitHub Desktop](https://desktop.github.com/)を使用する必要があります。
* Adobeとの統合は、[!DNL Adobe Experience Platform]のステージング環境にデプロイされた宛先のテスト段階である必要があります。

## [!DNL Adobe Experience Platform]で宛先のドキュメントを作成するための概要レベルの手順 {#high-level-instructions}

上位レベルでは、宛先のドキュメントを作成するには、[&#x200B; ドキュメントリポジトリの](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja#fork-the-repository)分岐[!DNL Adobe Experience Platform]を作成し、新しいブランチで[提供されたドキュメントテンプレート &#x200B;](./self-service-template.md)を編集する必要があります。 Adobeから提供されるテンプレートを使用して、新しい宛先ページを作成します。 準備ができたら、プルリクエスト（PR）を開きます。 これをおこなう手順は、さらに次のとおりです。[新しい宛先ページを作成する手順](./documentation-instructions.md#steps-to-create-docs-page)。

<!--

* In the table of contents (TOC.md) `/help/rtcdp/TOC.md`, add a link to your new destination page. Place it within the category where your destination resides in the Adobe Experience Platform user interface (for example: mobile, social, advertising). 
* In the overview page for the respective category, add a link to your new destination page. For example, for cloud storage destinations, you would add a link to [this page](https://docs.adobe.com/content/help/ja-JP/experience-platform/rtcdp/destinations/destinations-cat/cloud-storage/cloud-storage-destinations.html). 

-->

## ドキュメントテンプレート {#documentation-template}

ドキュメントページの作成を支援するために、Adobeでは[&#x200B; ドキュメントテンプレート &#x200B;](./self-service-template.md)が事前入力されています。 以下では、テンプレートを編集してプルリクエストを開く方法を説明します。 Adobe ドキュメントチームは、新しい宛先のドキュメントを確認して公開します。

[ここでテンプレートをダウンロードし](../assets/docs-framework/yourdestination-template.zip)、ファイルを解凍して`yourdestination.md` ファイルを抽出します。

テンプレートを使用してドキュメントページを作成する方法については、以下を参照してください。

## 新しい宛先ページを作成する手順 {#steps-to-create-docs-page}

GitHub web インターフェイスまたはローカル環境を使用して、[!DNL Adobe Experience Platform]に新しい宛先のドキュメントを作成できます。 両方のオプションの手順については、以下のリンクを参照してください。

* [GitHub web インターフェイスを使用した宛先ドキュメントページの作成](./use-github-interface-to-create-documentation.md)
* [ローカル環境でのテキストエディターを使用した宛先ドキュメントページの作成](./work-in-local-environment.md)

## ベストプラクティス {#best-practices}

宛先ドキュメント ページを作成する前と作成中に、[&#x200B; オーサリングのベストプラクティス &#x200B;](/help/destinations/destination-sdk/docs-framework/authoring-best-practices.md)を確認します。 また、Adobe ドキュメントチームがドキュメントのオーサリング時に使用する書き込みのヒントについては、[Adobe ドキュメントの書き込みガイダンス &#x200B;](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=ja)を参照してください。
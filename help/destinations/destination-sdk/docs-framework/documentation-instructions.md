---
title: Adobe Experience Platform の宛先のドキュメント化
description: Adobe Experience Platformで宛先のドキュメントページを作成する手順を説明します。
exl-id: 6cc9c758-44bb-463b-941a-06b1a22ee8f3
source-git-commit: 3c871471802e905b58acf8cd25db6788f49b9e43
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 8%

---

# Adobe Experience Platform の宛先のドキュメント化

## 概要 {#overview}

Adobe Experience Platformへようこそ、ここに来てくれて嬉しい！
宛先のドキュメント化は、Adobe Experience Platformでライブに設定する前の最後の手順です。

このドキュメントの節には、以下が含まれます。

* 新しい宛先のドキュメントページを作成する手順を説明します。
* 宛先に入力するためのテンプレート。
* [Markdown の一般的な使用手順](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en);
* [Markdown のフレーバーAdobeの具体的な説明](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=en#custom-markdown-extensions) (Adobeの Markdown の風味は、通常の Markdown と非常に似ています )。
* A [ベストプラクティスページ](./authoring-best-practices.md) を使用すると、宛先ページのドキュメントページを作成し、Experience Platformドキュメントの品質基準を満たすのに役立ちます。

## 前提条件 {#prerequisites}

この記事の手順に従って宛先のドキュメントを作成するには、次の項目が必要です。

* **GitHub アカウント**. 新規登録 [GitHub](https://github.com/) まだアカウントをお持ちでない場合は、をクリックします。
* **GitHub Desktop**. 次を選択した場合： [ローカル環境でドキュメントを作成します。](./work-in-local-environment.md)を使用する場合、 [GitHub Desktop](https://desktop.github.com/).
* Adobeとの統合は、Adobe Experience Platformのステージング環境にデプロイされた宛先とのテストフェーズにある必要があります。

## Adobe Experience Platformで宛先のドキュメントを作成する手順の概要 {#high-level-instructions}

宛先のドキュメントを作成するには、概要レベルで、次の手順を実行する必要があります [分岐を作成する](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=en#fork-the-repository) Adobe Experience Platformドキュメントリポジトリの [提供されたドキュメントテンプレート](./self-service-template.md) 新しい分岐に Adobeが提供するテンプレートを使用して、新しい宛先ページを作成します。 準備が整ったら、プルリクエスト (PR) を開きます。 これをおこなう手順は、以下で説明します。 [新しい宛先ページを作成する手順](./documentation-instructions.md#steps-to-create-docs-page).

<!--

* In the table of contents (TOC.md) `/help/rtcdp/TOC.md`, add a link to your new destination page. Place it within the category where your destination resides in the Adobe Experience Platform user interface (for example: mobile, social, advertising). 
* In the overview page for the respective category, add a link to your new destination page. For example, for cloud storage destinations, you would add a link to [this page](https://docs.adobe.com/content/help/en/experience-platform/rtcdp/destinations/destinations-cat/cloud-storage/cloud-storage-destinations.html). 

-->

## ドキュメントテンプレート {#documentation-template}

ドキュメントページの作成を支援するために、Adobeは、 [ドキュメントテンプレート](./self-service-template.md) あなたのために。 以下では、テンプレートを編集し、プル要求を開く方法について説明します。 Adobeドキュメントチームが、新しい宛先のドキュメントを確認して公開します。

[ここでテンプレートをダウンロード](assets/yourdestination-template.zip) を展開し、抽出するファイルを解凍します。 `yourdestination.md` ファイル。

テンプレートを使用してドキュメントページを作成する手順は、以下のとおりです。

## 新しい宛先ページを作成する手順 {#steps-to-create-docs-page}

GitHub Web インターフェイスまたはローカル環境を使用して、Adobe Experience Platformでの新しい宛先に関するドキュメントを作成できます。 両方のオプションの手順については、次のリンクを参照してください。

* [GitHub web インターフェイスを使用した宛先ドキュメントページの作成](./use-github-interface-to-create-documentation.md)
* [ローカル環境でのテキストエディターを使用した宛先ドキュメントページの作成](./work-in-local-environment.md)

## ベストプラクティス {#best-practices}

以下を確認します。 [オーサリングのベストプラクティス](/help/destinations/destination-sdk/docs-framework/authoring-best-practices.md) 宛先ドキュメントページを作成する前と前 必ず [Adobe文書の記述ガイダンス](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=en) ドキュメントのオーサリング時にAdobeドキュメントチームが使用する、その他の記述に関するヒント
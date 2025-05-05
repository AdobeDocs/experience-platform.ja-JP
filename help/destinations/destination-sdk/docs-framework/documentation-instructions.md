---
title: Adobe Experience Platform の宛先のドキュメント化
description: Adobe Experience Platformで宛先用のドキュメントページを作成するための手順
exl-id: 6cc9c758-44bb-463b-941a-06b1a22ee8f3
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 8%

---

# Adobe Experience Platform の宛先のドキュメント化

>[!IMPORTANT]
>
>ここで説明するプロセスは、パートナーが製品化（公開）された宛先を送信する場合にのみ必要です。 自分で使用するためにプライベート宛先を作成している場合は、宛先用のドキュメントを作成して公開する必要はありません。

## 概要 {#overview}

Adobe Experience Platformへようこそ、ここにあなたがいることを嬉しく思います！
宛先のドキュメント化は、Adobe Experience Platformでライブに設定する前の最後の手順です。

このドキュメントの節では、次の内容について説明します。

* 新しい宛先のドキュメントページを作成するためのステップバイステップの手順。
* 宛先に入力するためのテンプレート。
* [Markdown の使用に関する一般的な説明 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja)
* [Adobeマークダウンフレーバーの具体的な手順 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/markdown.html?lang=ja#custom-markdown-extensions) （Adobeマークダウンフレーバーは、通常のマークダウンと非常によく似ています）。
* Experience Platformドキュメント品質基準を満たす宛先ページのドキュメントページを作成するのに役立つ [ ベストプラクティスページ ](./authoring-best-practices.md)。

## 前提条件 {#prerequisites}

この記事の手順に従って宛先のドキュメントを作成するには、次の項目が必要です。

* **GitHub アカウント**。 まだアカウントをお持ちでない場合は、[GitHub](https://github.com/) に新規登録してください。
* **GitHub デスクトップ**。 [ ローカル環境でドキュメントを作成する ](./work-in-local-environment.md) を選択した場合は、[GitHub デスクトップ ](https://desktop.github.com/) を使用する必要があります。
* Adobeとの統合は、Adobe Experience Platformのステージング環境にデプロイされた宛先でテスト段階にある必要があります。

## Adobe Experience Platformで宛先に関するドキュメントを作成するための高度な手順 {#high-level-instructions}

大まかに言えば、宛先用にドキュメントを作成するには、Adobe Experience Platform ドキュメントリポジトリの [ 分岐を作成 ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/setup/local-repo.html?lang=ja#fork-the-repository) し、新しいブランチで [ 提供されたドキュメントテンプレート ](./self-service-template.md) を編集する必要があります。 Adobeが提供するテンプレートを使用して、新しい出力先ページを作成します。 準備ができたら、プルリクエスト（PR）を開きます。 これを行う手順は、さらに下の [ 新しい宛先ページを作成する手順 ](./documentation-instructions.md#steps-to-create-docs-page) にあります。

<!--

* In the table of contents (TOC.md) `/help/rtcdp/TOC.md`, add a link to your new destination page. Place it within the category where your destination resides in the Adobe Experience Platform user interface (for example: mobile, social, advertising). 
* In the overview page for the respective category, add a link to your new destination page. For example, for cloud storage destinations, you would add a link to [this page](https://docs.adobe.com/content/help/ja-JP/experience-platform/rtcdp/destinations/destinations-cat/cloud-storage/cloud-storage-destinations.html). 

-->

## ドキュメントテンプレート {#documentation-template}

ドキュメントページの作成を支援するために、Adobeから [ ドキュメントテンプレート ](./self-service-template.md) が事前入力されました。 テンプレートを編集してプルリクエストを開く手順については、後述します。 Adobeドキュメントチームが、新しい宛先用のドキュメントを確認し、公開します。

[ テンプレートをここからダウンロード ](../assets/docs-framework/yourdestination-template.zip) し、ファイルを解凍して `yourdestination.md` ファイルを抽出します。

テンプレートを使用してドキュメントページを作成する手順は、後述します。

## 新しい宛先ページを作成する手順 {#steps-to-create-docs-page}

GitHub web インターフェイスまたはローカル環境を使用して、Adobe Experience Platformの新しい宛先に関するドキュメントを作成できます。 以下のリンクに、両方のオプションの手順が記載されています。

* [GitHub web インターフェイスを使用した宛先ドキュメントページの作成](./use-github-interface-to-create-documentation.md)
* [ローカル環境でのテキストエディターを使用した宛先ドキュメントページの作成](./work-in-local-environment.md)

## ベストプラクティス {#best-practices}

宛先ドキュメントページを作成する前と作成中に、[ オーサリングのベストプラクティス ](/help/destinations/destination-sdk/docs-framework/authoring-best-practices.md) を確認してください。 Adobeドキュメントチームがドキュメントのオーサリング時に使用するその他の記述のヒントについては、[Adobeドキュメントのライティングガイダンス ](https://experienceleague.adobe.com/docs/contributor/contributor-guide/writing-essentials/general-writing-guidance.html?lang=ja) も必ずお読みください。
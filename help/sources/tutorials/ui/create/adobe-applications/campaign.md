---
keywords: Experience Platform;ホーム;人気のトピック;ソース;コネクタ;ソースコネクタ;Campaign;Campaign Managed Services
title: Platform UI を使用して Adobe Campaign Managed Services ソース接続を作成
description: Platform の UI を使用して Adobe Experience Platform を Adobe Campaign Managed Services に接続する方法について説明します。
hide: true
hidefromtoc: true
source-git-commit: 57a34b10e40dee80638392477d49c21e107c491f
workflow-type: ht
source-wordcount: '300'
ht-degree: 100%

---


# Platform UI を使用して Adobe Campaign Managed Services ソース接続を作成

このチュートリアルでは、ソースコネクタを作成して Adobe Campaign Managed Services のデータを Adobe Experience Platform に取り込む手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Platform を使用すると、様々なソースからデータを取り込みながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## Adobe Campaign Managed Services の Platform への接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

**[!UICONTROL アドビアプリケーション]**&#x200B;カテゴリ内で、「**[!UICONTROL Adobe Campaign Managed Services]**」、「**[!UICONTROL Add data]**」の順に選択します。

### データの選択 {#select-data}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_instance"
>title="ACC インスタンス"
>abstract="使用する Adobe Campaign Classic 環境の名前。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_mapping"
>title="ターゲットマッピング"
>abstract="ターゲットマッピングは、Campaign によってメッセージの配信に使用される技術的なオブジェクトで、配信の送信に必要なすべての技術的な設定（アドレス、電話番号、オプトインインジケーター、追加の識別子など）が含まれます。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_schema"
>title="スキーマ名"
>abstract="Adobe Campaign データベースで定義されたエンティティの名前。"
>text="Learn more in documentation"

[!UICONTROL データの選択]手順が表示され、[!UICONTROL Adobe Campaign インスタンス]、[!UICONTROL ターゲットマッピング]、および[!UICONTROL スキーマ名]の値を設定するためのインターフェイスが利用できます。

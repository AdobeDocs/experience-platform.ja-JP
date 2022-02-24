---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；campaign;campaign managed services
title: Platform UI を使用したAdobe Campaign Managed Servicesソース接続の作成
description: Platform UI を使用してAdobe Experience PlatformをAdobe Campaign Managed Servicesに接続する方法について説明します。
hide: true
hidefromtoc: true
source-git-commit: 24d7a549e83245fc363bd76f26ba58130e980c6c
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 4%

---


# Platform UI を使用したAdobe Campaign Managed Servicesソース接続の作成

このチュートリアルでは、Adobe Campaign Managed ServicesデータをAdobe Experience Platformに取り込むソースコネクタを作成する手順を説明します。

## はじめに

このガイドでは、 Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Platform を使用すると、様々なソースからデータを取り込みながら、 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md):Platform は、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## Adobe Campaign Managed Servicesを Platform に接続

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 また、検索バーを使用して、表示するソースを絞り込むこともできます。

以下 **[!UICONTROL Adobe]** カテゴリ、選択 **[!UICONTROL Adobe Campaign Managed Services]** 次に、 **[!UICONTROL データを追加]**.

### データを選択

この [!UICONTROL データを選択] 手順が表示され、 [!UICONTROL Adobe Campaignインスタンス], [!UICONTROL ターゲットマッピング]、および [!UICONTROL スキーマ名].

#### キャンペーンインスタンスを選択 {#select-campaign-instance}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_instance"
>title="キャンペーンインスタンスを選択"
>abstract="使用するAdobe Campaign Classic環境の名前。"
>text="Learn more in documentation"

#### キャンペーンマッピングを選択 {#select-campaign-mapping}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_mapping"
>title="キャンペーンターゲットマッピングを選択"
>abstract="ターゲットマッピングは、メッセージの配信に使用される技術的なオブジェクトで、配信の送信に必要なすべての技術的な設定（アドレス、電話番号、オプトイン指標、追加の識別子など）が含まれます。"
>text="Learn more in documentation"

#### キャンペーンスキーマを選択 {#select-campaign-schema}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_schema"
>title="キャンペーンスキーマ名を選択"
>abstract="Adobe Campaignデータベースで定義されたエンティティの名前。"
>text="Learn more in documentation"

---
title: Audience Manager の拡張アクティベーション
description: Audience Managerの拡張されたアクティベーションを通じて、Audience Managerオーディエンスをソーシャルおよび広告の宛先に対してアクティブ化する方法を説明します。
exl-id: 1f209578-a688-40b8-8f13-dab0d4380b3b
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 6%

---

# Audience Manager の拡張アクティベーション

Adobe Experience Platform上に構築されたAudience Manager拡張アクティベーションは、既存の [Audience Manager](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/aam-home) ユーザーが [Facebook[、[Google広告 ](../destinations/catalog/social/overview.md) などの、Real-Time CDPから提供される ](../destinations/catalog/social/facebook.md) ソーシャル ](../destinations/catalog/advertising/overview.md) 宛先および ](../destinations/catalog/advertising/google-ads-destination.md) 広告 [ 宛先プラットフォームにオーディエンスをアクティブ化するのに役立ちます。

>[!IMPORTANT]
>
>[!DNL Audience Manager Expanded Activation] は、既存の [!DNL Audience Manager] ユーザーのみが使用できます。

## 用語 {#terminology}

Audience Managerの拡張アクティベーションは、Adobe Experience Platformの概念とコンポーネントを使用します。 拡張アクティベーションのワークフローと使用するコンポーネントをより深く理解するには、次の概念に関する基本的な知識を持っている必要があります。

* [ オーディエンス ](../segmentation/ui/overview.md)：オーディエンスとは、類似の行動や特徴を共有する人々の集まりです。 このユーザーグループは、セグメント定義またはオーディエンス構成（Adobe Experience Platformで生成されたオーディエンス）を使用して Platform によって生成することも、カスタムアップロードなどの外部ソース（外部で生成されたオーディエンス）から生成することもできます。 展開されたアクティベーションでは、Audience Managerセグメント（オーディエンス）は [ カスタムアップロード ](../segmentation/ui/audience-portal.md#import-audience) としてインポートされます。
* [Source コネクタ ](../sources/home.md): Source コネクタ（ソースとも呼ばれます）を使用すると、Experience Platformユーザーが複数のソースからデータを簡単に取り込むことができ、Experience Platformサービスを使用したデータの構造化、ラベル付け、拡張が可能になります。 データは、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、さまざまなソースから取り込むことができます。
* [ 宛先コネクタ ](../destinations/home.md)：宛先は、オーディエンスがアクティブ化されて配信される任意のエンドポイント（Adobeアプリケーション、広告プラットフォーム、クラウドストレージサービス、マーケティングサービスなど）を表します。 [!DNL Expanded Activation] では、[advertising](../destinations/catalog/advertising/overview.md) および [social](../destinations/catalog/social/overview.md) 宛先コネクタに対するオーディエンスのアクティベーションをサポートしています。

## 前提条件 {#prerequisites}

拡張アクティベーションを通じてオーディエンスをアクティブ化する前に、以下に説明する前提条件を満たしていることを確認してください。

### ユーザーと役割の要件 {#permission-requirements}

[!DNL Expanded Activation] を使用する前に、Admin Consoleからユーザーアカウントを作成して、[!DNL Expanded Activation] ロールに割り当てる必要があります。 これを行う方法について詳しくは、[ 管理 ](administration.md) ページを参照してください。

### オーディエンス要件 {#audience-requirements}

[!DNL Expanded Activation] を通じてオーディエンスをアクティブ化するには、Audience Managerオーディエンスが **ハッシュ化されたメールアドレス** に基づいていることを確認します。 これを確認するには、Audience Managerの使用状況に応じて、次の 2 つの方法があります。

* [People-based Destinations](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) のAudience Manager機能を使用している場合は、既にハッシュ化されたメールアドレスをAudience Managerで取り込んでいます。 この場合、追加の手順は必要ありません。 [ 拡張されたアクティベーションによるオーディエンスのアクティブ化 ](activate-audiences.md) にスキップできます。
* _2_ People-based Destinations のAudience Manager](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) 機能を使用していない場合は、Audience Managerで新しいデータソースを作成し、それを使用してハッシュ化されたメールアドレスを保存する必要があります。 [これを行う方法については、[ ハッシュ化されたメールワークフローのデータソースの設定 ](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/data-sources/create-data-source-hashed-emails) に関するドキュメントを参照してください。 ハッシュ化されたメールアドレスをAudience Managerデータソースに取り込んだら、[ 拡張されたアクティベーションによるオーディエンスのアクティブ化 ](activate-audiences.md) に関するドキュメントを参照してください。

## 次の手順 {#next-steps}

ユースケースとオーディエンスを使用する利点をより深く理解できたので、まず [ アカウントの設定 ](administration.md) 次に [ オーディ [!DNL Expanded Activation] ンスのアクティブ化 ](activate-audiences.md) を行います。

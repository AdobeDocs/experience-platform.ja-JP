---
title: Audience Manager の拡張アクティベーション
description: Audience Manager Expanded Activation を通じて、Audience Manager オーディエンスをソーシャルおよび広告の宛先に対してアクティブ化する方法について説明します。
exl-id: 1f209578-a688-40b8-8f13-dab0d4380b3b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 6%

---

# Audience Manager の拡張アクティベーション

Adobe Experience Platform上に構築されたAudience Manager Expanded Activation は、既存の [Audience Manager](https://experienceleague.adobe.com/ja/docs/audience-manager/user-guide/aam-home) ユーザーが [Facebook](../destinations/catalog/social/overview.md)、[&#128279;](../destinations/catalog/social/facebook.md)Google Ads[&#x200B; などのReal-Time CDPから &#x200B;](../destinations/catalog/advertising/overview.md) ソーシャル  および [&#x200B; 広告 &#x200B;](../destinations/catalog/advertising/google-ads-destination.md) 宛先プラットフォームにオーディエンスをアクティブ化するのに役立ちます。

>[!IMPORTANT]
>
>[!DNL Audience Manager Expanded Activation] は、既存の [!DNL Audience Manager] ユーザーのみが使用できます。

## 用語 {#terminology}

Audience Manager Expanded Activation は、Adobe Experience Platformの概念とコンポーネントを使用します。 拡張アクティベーションのワークフローと使用するコンポーネントをより深く理解するには、次の概念に関する基本的な知識を持っている必要があります。

* [&#x200B; オーディエンス &#x200B;](../segmentation/ui/overview.md)：オーディエンスとは、類似の行動や特徴を共有する人々の集まりです。 このユーザーグループは、セグメント定義またはオーディエンス構成（Adobe Experience Platformで生成されたオーディエンス）を使用してExperience Platformで生成することも、カスタムアップロードなどの外部ソース（外部で生成されたオーディエンス）から生成することもできます。 拡張されたアクティベーションでは、Audience Manager セグメント（オーディエンス）は [&#x200B; カスタムアップロード &#x200B;](../segmentation/ui/audience-portal.md#import-audience) として読み込まれます。
* [Source コネクタ &#x200B;](../sources/home.md): Source コネクタ（ソースとも呼ばれます）を使用すると、Experience Platformのユーザーが複数のソースからデータを簡単に取り込むことができ、Experience Platform サービスを使用したデータの構造化、ラベル付け、拡張が可能になります。 データは、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、さまざまなソースから取り込むことができます。
* [&#x200B; 宛先コネクタ &#x200B;](../destinations/home.md)：宛先は、オーディエンスがアクティブ化されて配信される任意のエンドポイント（Adobe アプリケーション、広告プラットフォーム、クラウドストレージサービス、マーケティングサービスなど）を表します。 [!DNL Expanded Activation] では、[advertising](../destinations/catalog/advertising/overview.md) および [social](../destinations/catalog/social/overview.md) 宛先コネクタに対するオーディエンスのアクティベーションをサポートしています。

## 前提条件 {#prerequisites}

拡張アクティベーションを通じてオーディエンスをアクティブ化する前に、以下に説明する前提条件を満たしていることを確認してください。

### ユーザーと役割の要件 {#permission-requirements}

[!DNL Expanded Activation] を使用する前に、Admin Consoleからユーザーアカウントを作成して、[!DNL Expanded Activation] ロールに割り当てる必要があります。 これを行う方法について詳しくは、[&#x200B; 管理 &#x200B;](administration.md) ページを参照してください。

### オーディエンス要件 {#audience-requirements}

[!DNL Expanded Activation] を通じてオーディエンスをアクティブ化するには、Audience Manager オーディエンスが **ハッシュ化されたメールアドレス** に基づいていることを確認します。 これを確認するには、Audience Managerの使用状況に応じて、次の 2 つの方法があります。

* [Audience Manager People-based Destinations](https://experienceleague.adobe.com/ja/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) 機能を使用している場合は、既にAudience Managerでハッシュ化されたメールアドレスを取り込んでいます。 この場合、追加の手順は必要ありません。 [&#x200B; 拡張されたアクティベーションによるオーディエンスのアクティブ化 &#x200B;](activate-audiences.md) にスキップできます。
* _2&rbrace;Audience Manager People-based Destinations_ 機能を使用していない [&#128279;](https://experienceleague.adobe.com/ja/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) 場合は、Audience Managerで新しいデータソースを作成し、それを使用してハッシュ化されたメールアドレスを保存する必要があります。 これを行う方法については、[&#x200B; ハッシュ化されたメールワークフローのデータソースの設定 &#x200B;](https://experienceleague.adobe.com/ja/docs/audience-manager/user-guide/features/data-sources/create-data-source-hashed-emails) に関するドキュメントを参照してください。 ハッシュ化されたメールアドレスをAudience Manager データソースに取り込んだら、[&#x200B; 拡張されたアクティベーションを通じたオーディエンスのアクティブ化 &#x200B;](activate-audiences.md) に関するドキュメントを参照してください。

## 次の手順 {#next-steps}

ユースケースとオーディエンスを使用する利点をより深く理解できたので、まず [&#x200B; アカウントの設定 &#x200B;](administration.md) 次に [&#x200B; オーディ [!DNL Expanded Activation] ンスのアクティブ化 &#x200B;](activate-audiences.md) を行います。

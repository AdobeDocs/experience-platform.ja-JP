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

Adobe Experience Platform上に構築されたAudience Manager拡張アクティベーションは、既存のものを支援 [Audience Manager](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/aam-home) ユーザーは、次の目的でオーディエンスをアクティブ化します [ソーシャル](../destinations/catalog/social/overview.md) および [広告](../destinations/catalog/advertising/overview.md) Real-Time CDPからの出力先プラットフォーム（例：） [Facebook](../destinations/catalog/social/facebook.md), [Google広告](../destinations/catalog/advertising/google-ads-destination.md)、など。

>[!IMPORTANT]
>
>[!DNL Audience Manager Expanded Activation] 既存のユーザーのみが使用できます [!DNL Audience Manager] ユーザー。

## 用語 {#terminology}

Audience Managerの拡張アクティベーションは、Adobe Experience Platformの概念とコンポーネントを使用します。 拡張アクティベーションのワークフローと使用するコンポーネントをより深く理解するには、次の概念に関する基本的な知識を持っている必要があります。

* [オーディエンス](../segmentation/ui/overview.md)：オーディエンスは、類似の行動や特性を共有する人々の集まりです。 このユーザーグループは、セグメント定義またはオーディエンス構成（Adobe Experience Platformで生成されたオーディエンス）を使用して Platform によって生成することも、カスタムアップロードなどの外部ソース（外部で生成されたオーディエンス）から生成することもできます。 拡張されたアクティベーションでは、Audience Managerセグメント（オーディエンス）は次のように読み込まれます。 [カスタムアップロード](../segmentation/ui/audience-portal.md#import-audience).
* [Source コネクタ](../sources/home.md):Source コネクタ（ソースとも呼ばれます）を使用すると、Experience Platformユーザーが複数のソースからデータを簡単に取り込むことができ、Experience Platformサービスを使用したデータの構造化、ラベル付け、拡張が可能になります。 データは、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、さまざまなソースから取り込むことができます。
* [宛先コネクタ](../destinations/home.md)：宛先は、オーディエンスがアクティブ化されて配信される任意のエンドポイント（Adobeアプリケーション、広告プラットフォーム、クラウドストレージサービス、マーケティングサービスなど）を表します。 [!DNL Expanded Activation] は、に対するオーディエンスのアクティブ化をサポートします [広告](../destinations/catalog/advertising/overview.md) および [ソーシャル](../destinations/catalog/social/overview.md) 宛先コネクタ。

## 前提条件 {#prerequisites}

拡張アクティベーションを通じてオーディエンスをアクティブ化する前に、以下に説明する前提条件を満たしていることを確認してください。

### ユーザーと役割の要件 {#permission-requirements}

使用する前に [!DNL Expanded Activation]の場合は、Admin Consoleからユーザーアカウントを作成し、それに割り当てる必要があります。 [!DNL Expanded Activation] 役割。 を参照してください。 [管理](administration.md) この方法に関する詳細な手順については、このページを参照してください。

### オーディエンス要件 {#audience-requirements}

を使用してオーディエンスをアクティブ化するには [!DNL Expanded Activation]を使用する場合は、Audience Managerオーディエンスがに基づいていることを確認します **ハッシュ化されたメールアドレス**. これを確認するには、Audience Managerの使用状況に応じて、次の 2 つの方法があります。

* を使用する場合 [People-based Destinations のAudience Manager](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) 機能を使用すると、ハッシュ化されたメールアドレスをAudience Managerで既に取り込むことができます。 この場合、追加の手順は必要ありません。 次にスキップできます： [拡張されたアクティベーションによるオーディエンスのアクティブ化](activate-audiences.md).
* 次の場合： _ではない_ の使用 [People-based Destinations のAudience Manager](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/destinations/people-based/people-based-destinations-overview) 機能を使用するには、Audience Managerで新しいデータソースを作成し、それを使用してハッシュ化されたメールアドレスを保存する必要があります。 のドキュメントを参照してください。 [ハッシュ化されたメールワークフロー用のデータソースの設定](https://experienceleague.adobe.com/en/docs/audience-manager/user-guide/features/data-sources/create-data-source-hashed-emails) その方法については、こちらを参照してください。 ハッシュ化されたメールアドレスをAudience Managerデータソースに取り込んだら、次のドキュメントを参照してください [拡張されたアクティベーションによるオーディエンスのアクティブ化](activate-audiences.md).

## 次の手順 {#next-steps}

これで、の使用例とメリットについてより深く理解できました [!DNL Expanded Activation]、開始 [アカウントの設定](administration.md) その後 [オーディエンスのアクティベート](activate-audiences.md).

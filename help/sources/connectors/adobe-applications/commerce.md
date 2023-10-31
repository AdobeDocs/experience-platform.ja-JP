---
title: Adobe Commerce Source Connector
description: Adobe Commerceソースを使用してコマースデータをExperience Platformに取り込む方法を説明します。
last-substantial-update: 2023-06-21T00:00:00Z
exl-id: 8313e3d5-5c3d-448c-883c-b9386dbbb2f5
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 2%

---

# Adobe Commerce

Adobe Commerceはアジャイルな B2B および B2C コマースプラットフォームで、オンラインと物理的なスペースにまたがる顧客中心のデジタルコマースエクスペリエンスを通じて、商人やブランドが収益を促進できます。

Adobe Experience Platform Sources は、Adobe Commerceの統合をサポートして、マーチャントがストアフロントおよびバックオフィスのデータをExperience PlatformEdge Network に送信できるようにし、Adobe AnalyticsやAdobe Targetなどの他のAdobe Experience Cloud製品で [!DNL Commerce] データ。

* **ストアフロントイベント**：買い物客のインタラクションのキャプチャ（例： ） `View Page`, `View Product`、および `Add to Cart`. B2B 商人の場合、店頭イベントも [購買依頼リスト](<https://experienceleague.adobe.com/docs/commerce-admin/b2b/requisition-lists/requisition-lists.html>).
* **バックオフィスイベント**：オーダーのステータスに関する情報（オーダーが発行された、取り消された、返金された、出荷された、完了したかなど）を取得します。

>[!NOTE]
>
>Adobe Commerceで取り込まれたデータには、個人を特定できる情報 (PII) は含まれていません。 Cookie ID や IP アドレスなどのすべてのユーザー識別子は厳密に匿名化されます。

## 前提条件

Adobe CommerceをExperience Platformに接続するには、次が必要です。

* Adobe Commerce 2.4.3 以降。
* 有効なAdobe ID ID および組織 ID。
* へのアクセス [Adobeクライアントデータレイヤー拡張機能](../../../tags/extensions/client/client-data-layer/overview.md). この拡張機能は、ストアフロントイベントデータを収集するために必要です。
* 他のAdobeDX 製品の使用権限

## オンボーディング手順

Adobe Commerceソースアカウントを完全にオンボーディングするには、以下に示す手順に従い、対応するドキュメントに進みます。

* [Experience Platformコネクタ拡張機能のインストール](https://experienceleague.adobe.com/docs/commerce-merchant-services/experience-platform-connector/fundamentals/install.html) Adobe Commerceの コネクタ拡張機能は、次の場所からダウンロードできます。 [AdobeMarketplace](https://commercemarketplace.adobe.com/magento-experience-platform-connector.html).
* Connector 拡張機能が正常にインストールされたら、Experience CloudのAdobeアカウントにログインし、 [組織 ID を確認する](https://experienceleague.adobe.com/docs/core-services/interface/administration/organizations.html?lang=en#concept_EA8AEE5B02CF46ACBDAD6A8508646255). この ID は、プロビジョニングされたExperience Cloud会社に関連付けられます。 24 文字の英数字の形式で、必須の文字が含まれています `@AdobeOrg`.
* 次に、コマース固有のフィールドグループを使用して、Experience Data Model(XDM) スキーマを作成または更新します。 コマース固有のフィールドグループを XDM スキーマに追加する方法について詳しくは、 [XDM スキーマへのフィールドグループの追加](https://experienceleague.adobe.com/docs/commerce-merchant-services/experience-platform-connector/fundamentals/update-xdm.html?lang=ja).
* スキーマを設定したら、新しいスキーマに基づいてデータセットを作成する必要があります。 このデータセットには、 [!DNL Commerce] 送信するデータ。 のデータセットを作成する方法の詳細な手順については、 [!DNL Commerce] データについては、 [データをExperience Platformに送信中](https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/experience-cloud/platform.html?lang=en#create-a-dataset).
* 次に、データストリームを作成し、コマース固有のフィールドグループを含む XDM スキーマを選択します。 データストリームの詳細については、 [データストリームの概要](https://experienceleague.adobe.com/docs/experience-platform/datastreams/overview.html?lang=ja).
* 次に、Adobe Commerceインスタンスを [Commerce Services コネクタ](https://experienceleague.adobe.com/docs/commerce-merchant-services/user-guides/integration-services/saas.html). これにより、コマースインスタンスを SaaS(Software as a Service) としてデプロイできます。
* 前述のすべての設定が完了したら、を使用して Commerce Services コネクタとExperience Platformコネクタの両方を設定することで、Experience Platformに接続できるようになりました。 [!DNL Commerce Admin]. この最後の手順の詳細については、 [コマースデータのExperience Platformへの接続](https://experienceleague.adobe.com/docs/commerce-merchant-services/experience-platform-connector/fundamentals/connect-data.html).

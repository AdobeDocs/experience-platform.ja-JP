---
title: Adobe Commerce Source コネクタ
description: Adobe Commerce ソースを使用してコマースデータをExperience Platformに取り込む方法を説明します。
last-substantial-update: 2023-12-13T00:00:00Z
exl-id: 8313e3d5-5c3d-448c-883c-b9386dbbb2f5
source-git-commit: 506f33e03dea5d5879808bccb82948209714926a
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Adobe Commerce

Adobe Commerceは、アジャイルな B2B および B2C コマースプラットフォームであり、オンラインおよび物理スペースをまたいで顧客中心のデジタルコマースエクスペリエンスを通じて、マーチャントおよびブランドの収益を促進できます。

Adobe Experience Platform Sources では、Adobe Commerceとの統合をサポートしています。これにより、マーチャントはストアフロントおよびバックオフィスのデータをExperience PlatformEdge Networkに送信でき、Adobe AnalyticsやAdobe Targetなどの他のAdobe Experience Cloud製品でも [!DNL Commerce] のデータを使用できます。

* **ストアフロントイベント**：買い物客のインタラクション（`View Page`、`View Product`、`Add to Cart` など）をキャプチャします。 B2B マーチャントの場合、ストアフロントイベントは [&#x200B; 購買依頼リスト &#x200B;](<https://experienceleague.adobe.com/docs/commerce-admin/b2b/requisition-lists/requisition-lists.html?lang=ja>) もキャプチャします。
* **バックオフィスイベント**：注文のステータスに関する情報（注文の成否、キャンセル、返金、発送、完了など）を取得します。

>[!NOTE]
>
>Adobe Commerceでキャプチャされたデータには、個人を特定できる情報（PII）は含まれません。 Cookie ID や IP アドレスなどのすべてのユーザー識別子は、厳密に匿名化されます。

## 前提条件

Adobe CommerceをExperience Platformに接続するには、次の条件を満たす必要があります。

* Adobe Commerce 2.4.3 以降。
* 有効なAdobe IDと組織 ID。
* [Adobeクライアントデータレイヤー拡張機能 &#x200B;](../../../tags/extensions/client/client-data-layer/overview.md) にアクセスします。 この拡張機能は、ストアフロントイベントデータを収集するために必要です。
* 他のAdobeDX 製品の使用権限。

## オンボーディング手順

Adobe Commerce ソースアカウントを完全にオンボードするには、対応するドキュメントと共に、以下に説明する手順に従います。

* Adobe Commerce用に [&#x200B; 拡張機能をイ  [!DNL Data Connection]  ストール &#x200B;](https://experienceleague.adobe.com/docs/commerce-merchant-services/data-connection/fundamentals/install.html?lang=ja) します。 [Adobeの Marketplace](https://commercemarketplace.adobe.com/magento-experience-platform-connector.html) からコネクタ拡張機能をダウンロードできます。
* コネクタ拡張機能が正常にインストールされたら、Experience CloudでAdobeアカウントにログインし、[&#x200B; 組織 ID を確認 &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/administration/organizations.html?lang=ja#concept_EA8AEE5B02CF46ACBDAD6A8508646255) します。 この ID は、プロビジョニングされているExperience Cloud会社に関連付けられています。 24 文字の英数字の文字列の形式で、必須の文 `@AdobeOrg` が含まれています。
* 次に、Commerce固有のフィールドグループを使用して、エクスペリエンスデータモデル（XDM）スキーマを作成または更新します。 XDM スキーマにCommerce固有のフィールドグループを追加する方法の手順について詳しくは、[XDM スキーマへのフィールドグループの追加 &#x200B;](https://experienceleague.adobe.com/docs/commerce-merchant-services/data-connection/fundamentals/update-xdm.html?lang=ja) に関するガイドを参照してください。
* スキーマを設定したら、新しいスキーマに基づいてデータセットを作成する必要があります。 このデータセットには、送信した [!DNL Commerce] データが含まれます。 データのデータセットを作成する手順について詳しくは、[Experience Platformへのデ [!DNL Commerce] タの送信 &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/implement-mobile-sdk/experience-cloud/platform.html?lang=ja#create-a-dataset) に関するガイドを参照してください。
* 次に、データストリームを作成し、Commerce固有のフィールドグループを含む XDM スキーマを選択します。 データストリームについて詳しくは、[&#x200B; データストリームの概要 &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/datastreams/overview.html?lang=ja) を参照してください。
* 次に、Adobe Commerce インスタンスを [Commerce サービスコネクタ &#x200B;](https://experienceleague.adobe.com/docs/commerce-merchant-services/user-guides/integration-services/saas.html?lang=ja) に接続する必要があります。 これにより、Commerce インスタンスを SaaS （Software as a Service）としてデプロイできます。
* 前述のすべての設定が完了したら、[!DNL Commerce Admin] を使用してCommerce サービスコネクタと [!DNL Data Connection] 拡張機能の両方を設定することで、Experience Platformに接続できるようになります。 この最後の手順について詳しくは、[Commerce データのExperience Platformへの接続 &#x200B;](https://experienceleague.adobe.com/docs/commerce-merchant-services/data-connection/fundamentals/connect-data.html?lang=ja) に関するガイドを参照してください。

---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceとExperience Cloudの適用
topic-legacy: overview
description: このドキュメントでは、プライバシー関連の操作用に様々なExperience Cloudアプリケーションを設定する方法について説明します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: f193787ac27e30c69d25418656ae9c59c89622dc
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 50%

---

# [!DNL Privacy Service] およびアプリケ [!DNL Experience Cloud] ーション

Adobe Experience Platform [!DNL Privacy Service]は、複数のAdobe Experience Cloudアプリケーションのプライバシーリクエストをサポートするように設計されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントでは、プライバシー関連の操作用にアプリケーションを設定する方法を説明した[!DNL Experience Cloud]アプリケーションドキュメントのリファレンスを提供します。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Service](#integrated)と統合されたアプリケーション：にアクセス、削除またはオプトアウトのリクエストを送信できるアプリケーシ [!DNL Privacy Service]ョン。
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

プライバシーリクエストの形式設定方法と、それらのリクエストでサポートされる値については、[!DNL Experience Cloud]アプリケーションのドキュメントを参照してください。

## [!DNL Privacy Service]と統合されたアプリケーション {#integrated}

[!DNL Privacy Service]と統合されている[!DNL Experience Cloud]アプリケーションのリストを次に示します。このリストには、互換性のある[!DNL Privacy Service]機能や、詳細なドキュメントへのリンクが含まれます。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
| --- | :---: | :---: | --- |
| Adobe Advertising Cloud | ✓ | ✓ | <ul><li>[GDPR向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPAのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPAの販売オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/an-gdpr-overview.html?lang=ja)</li><li>[!DNL Analytics] は、[プライバシーレポート変数](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/consent-variables.html)を使用してオプトアウトリクエストを処理します。</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/consents.md)</li></ul> |
| Adobe顧客属性(CRS) | ✓ | なし | <ul><li>[GDPR向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html)</li><li>[CCPAのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[リアルタイム顧客プロファイルのためのアクセス / 削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] は、オーデ [ィエンスセグメントのオプトアウトリクエストを許可します](../segmentation/consents.md)。</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>[!DNL Primetime] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## セルフサービスアプリケーション {#self-serve}

以下は、[!DNL Privacy Service]と統合されておらず、プライバシーに関する懸念事項を内部で管理する必要がある[!DNL Experience Cloud]アプリケーションのリストです。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://docs.campaign.adobe.com/doc/AC/getting_started/JA/ACC_GDPR.html) | Adobe Campaign Classic の GDPR 機能の概要。 |
| [Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-4/managing/using/gdpr-compliance.html) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://experienceleague.adobe.com/docs/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Adobe Experience Platform Launch](https://docs.adobelaunch.com/client-side-information/deploy-javascript-tags-to-opt-in-to-launch) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |

{style=&quot;table-layout:auto&quot;}

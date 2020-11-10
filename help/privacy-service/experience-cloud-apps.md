---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Privacy Service と Experience Cloud アプリケーション
topic: overview
translation-type: tm+mt
source-git-commit: 4cd7b9d3ca542c2fba83d066197b92775c053729
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 62%

---


# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリケーション

Adobe Experience Platform [!DNL Privacy Service] is built to support privacy requests for several Adobe Experience Cloud applications. 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

This document serves as a reference for [!DNL Experience Cloud] application documentation that outlines how to configure that application for privacy-related operations. データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Serviceと統合されたアプリケーション](#integrated):アクセス、削除またはオプトアウトの要求を送信できるアプリケーション [!DNL Privacy Service]。
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

Please review the documentation for your [!DNL Experience Cloud] applications to learn how to format your privacy requests, and which values are supported for those requests.

## Applications integrated with [!DNL Privacy Service] {#integrated}

The following is a list of [!DNL Experience Cloud] applications that are integrated with [!DNL Privacy Service], including the [!DNL Privacy Service] capabilities they are compatible with, and links to documentation for more information.

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
--- | :---: | :---: | ---
| Adobe Advertising Cloud | ✓ | ✓ | <ul><li>[GDPR向けドキュメントへのアクセス/削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPAのドキュメントへのアクセスと削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPAのオプトアウト販売ドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/en/analytics/admin/data-governance/an-gdpr-overview.html)</li><li>[!DNL Analytics] は、[プライバシーレポート変数](https://docs.adobe.com/content/help/ja-JP/analytics/admin/data-governance/consent-variables.html)を使用してオプトアウトリクエストを処理します。</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/honoring-opt-outs.md)</li></ul> |
| Adobe顧客属性(CRS) | ✓ | なし | <ul><li>[GDPR向けドキュメントへのアクセス/削除](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/gdpr.html)</li><li>[CCPAのドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/ccpa.html)</li><li>顧客属性にはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[リアルタイム顧客プロファイルのためのアクセス / 削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] は、オーディエンスセグメントの [オプトアウト要求に従います](../segmentation/honoring-opt-outs.md)。</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>[!DNL Primetime] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/ja-JP/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |


## セルフサービスアプリケーション {#self-serve}

The following is a list of [!DNL Experience Cloud] applications that are not integrated with [!DNL Privacy Service] and must manage their privacy concerns internally. 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://docs.campaign.adobe.com/doc/AC/getting_started/JA/ACC_GDPR.html) | Adobe Campaign Classic の GDPR 機能の概要。 |
| [Adobe Dynamic Tag Manager](https://docs.adobe.com/content/help/ja-JP/dtm/using/tools/opt-in.html) | 同意が得られるまでアドビタグが実行されないようにする手順。 |
| [Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-4/managing/using/gdpr-compliance.html) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://docs.adobe.com/content/help/en/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Adobe Experience Platform Launch](https://docs.adobelaunch.com/client-side-information/deploy-javascript-tags-to-opt-in-to-launch) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |

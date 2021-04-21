---
keywords: Experience Platform；ホーム；人気の高いトピック
solution: Experience Platform
title: Privacy ServiceとExperience Cloudの申し込み
topic-legacy: overview
description: このドキュメントでは、プライバシーに関する操作のために様々なExperience Cloudアプリケーションを設定する方法に関するリファレンスを提供します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 58%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリケーション

Adobe Experience Platform[!DNL Privacy Service]は、複数のAdobe Experience Cloudアプリケーションに対するプライバシー要求をサポートするように設計されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントは、プライバシーに関する操作のためのアプリケーションの設定方法を概説した[!DNL Experience Cloud]アプリケーションドキュメントのリファレンスとして機能します。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Serviceと統合されたアプリケーション](#integrated):アクセス、削除またはオプトアウトの要求を送信できるアプリケーション [!DNL Privacy Service]。
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

プライバシーリクエストの書式設定方法、およびそれらのリクエストでサポートされる値については、[!DNL Experience Cloud]アプリケーションのドキュメントを参照してください。

## [!DNL Privacy Service] {#integrated}に統合されたアプリケーション

[!DNL Privacy Service]と統合される[!DNL Experience Cloud]アプリケーションのリストを次に示します。[!DNL Privacy Service]と互換性のある機能や、詳細についてのドキュメントへのリンクが含まれます。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
--- | :---: | :---: | ---
| Adobe Advertising Cloud | ✓ | kid | <ul><li>[GDPR向けドキュメントへのアクセス/削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPAのドキュメントへのアクセスと削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPAのオプトアウト販売ドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | kid | kid | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/en/analytics/admin/data-governance/an-gdpr-overview.html)</li><li>[!DNL Analytics] は、[プライバシーレポート変数](https://docs.adobe.com/content/help/en/analytics/admin/data-governance/consent-variables.html)を使用してオプトアウトリクエストを処理します。</li></ul> |
| Adobe Audience Manager | kid | kid | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | kid | kid | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/honoring-opt-outs.md)</li></ul> |
| Adobe顧客属性(CRS) | kid | なし | <ul><li>[GDPR向けドキュメントへのアクセス/削除](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/gdpr.html)</li><li>[CCPAのドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/ccpa.html)</li><li>顧客属性にはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |
| Adobe Experience Platform | kid | kid | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[リアルタイム顧客プロファイルのためのアクセス / 削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] は、オーディエンスセグメントの [オプトアウト要求に従います](../segmentation/honoring-opt-outs.md)。</li></ul> |
| Adobe Primetime Authentication | kid | なし | <ul><li>[アクセス / 削除に関するドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>[!DNL Primetime] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | kid | なし | <ul><li>[アクセス / 削除に関するドキュメント](https://docs.adobe.com/content/help/ja-JP/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |


## セルフサービスアプリケーション {#self-serve}

以下は、[!DNL Privacy Service]と統合されておらず、内部でプライバシー上の問題を管理する必要がある[!DNL Experience Cloud]アプリケーションのリストです。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://docs.campaign.adobe.com/doc/AC/getting_started/JA/ACC_GDPR.html) | Adobe Campaign Classic の GDPR 機能の概要。 |
| [Adobe Dynamic Tag Manager](https://docs.adobe.com/content/help/ja-JP/dtm/using/tools/opt-in.html) | 同意が得られるまでアドビタグが実行されないようにする手順。 |
| [Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-4/managing/using/gdpr-compliance.html) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://docs.adobe.com/content/help/en/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Adobe Experience Platform Launch](https://docs.adobelaunch.com/client-side-information/deploy-javascript-tags-to-opt-in-to-launch) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |

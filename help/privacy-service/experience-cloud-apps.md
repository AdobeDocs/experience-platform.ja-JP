---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーサービスおよびExperience Cloudアプリケーション
topic: overview
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# プライバシーサービスおよびExperience Cloudアプリケーション

Adobe Experience Platformプライバシーサービスは、複数のAdobe Experience Cloudアプリケーションのプライバシーリクエストをサポートするように設計されています。 各アプリケーションは、データの主題を識別するために、異なる製品値とIDをサポートしています。

このドキュメントは、プライバシー関連の操作用にアプリを設定する方法を説明したExperience Cloudアプリケーションドキュメントのリファレンスとして機能します。 データの形式設定やラベル付けの方法も含まれます。 2つのカテゴリのアプリケーションが対象です。

* [Applications integrated with Privacy Service](#integrated):プライバシーサービスにアクセス、削除、またはオプトアウトの要求を送信できるアプリケーション。
* [セルフサービスアプリケーション](#self-serve):プライバシー要求を内部で管理する必要があり、プライバシーサービスと直接通信できない申し込み。

プライバシーリクエストの形式設定方法と、それらのリクエストでサポートされている値については、Experience Cloudアプリケーションのドキュメントを参照してください。

## プライバシーサービスに統合された申し込み {#integrated}

次のリストは、プライバシーサービスと統合されているExperience Cloudアプリケーションを示しています。この機能には、互換性のあるプライバシーサービス機能や、詳細についてのドキュメントへのリンクが含まれています。

| アプリケーション | アクセス/削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
--- | :---: | :---: | ---
| Adobe Advertising Cloud | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/en/advertising-cloud/all/privacy/ad-cloud-gdpr.html) </li><li>Advertising Cloudは、アドビプライバシーセンターが提供する既存のグローバルオプトアウト機能を利用します。 詳しくは、データのプライバシー [リクエストの作成に関するガイド](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#opt-out-requests) を参照してください。</li></ul> |
| Adobe Analytics | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://marketing.adobe.com/resources/help/en_US/analytics/gdpr/index.html)</li><li>Analyticsは、プライバシーレポート変数を使用してオプトアウト [要求を処理します](https://docs.adobe.com/content/help/ja-JP/analytics/admin/data-governance/consent-variables.html)</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://marketing.adobe.com/resources/help/en_US/aam/aam-gdpr.html)</li><li>[オプトアウトのドキュメント](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.campaign.adobe.com/doc/standard/getting_started/en/ACS_GDPR.html)</li><li>[オプトアウトのドキュメント](../segmentation/honoring-opt-outs.md)</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | <ul><li>[Data Lakeのドキュメントへのアクセスと削除](../catalog/privacy.md)</li><li>[リアルタイム顧客プロファイルのドキュメントへのアクセス/削除](../profile/privacy.md)</li><li>Experience Platformは、オプトア [ウトリクエストに従います](../segmentation/honoring-opt-outs.md)。</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | <ul><li>[ドキュメントへのアクセスと削除](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>Primetimeにはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |
| Adobe Target | ✓ | なし | <ul><li>[ドキュメントへのアクセスと削除](https://marketing.adobe.com/resources/help/en_US/target/target/privacy-and-general-data-protection-regulation.html)</li><li>ターゲットにはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |

<!-- (To include once access/delete documentation is available)
Adobe Customer Attributes (CRS) | ✓ | N/A | <ul><li>Customer Attributes does not have the capability to transfer data, therefore opt-out-of-sale requests are not applicable.</li></ul>
-->

## セルフサービスアプリケーション {#self-serve}

以下は、Experience Cloudアプリケーションのリストで、プライバシーサービスと統合されておらず、プライバシーに関する問題を社内で管理する必要があります。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://docs.campaign.adobe.com/doc/AC/getting_started/JA/ACC_GDPR.html) | GDPR機能の概要をAdobe CampaignClassic。 |
| [Adobe Dynamic Tag Manager](https://marketing.adobe.com/resources/help/ja_JP/dtm/opt-in.html) | 同意が得られるまでAdobeタグが実行されないようにする手順です。 |
| [Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-4/managing/using/gdpr-compliance.html) | 顧客プライバシー管理者またはAEM管理者がGDPR要求を処理する方法の概要を示します。 |
| [Adobe Experience Manager Livefyre](https://marketing.adobe.com/resources/help/en_US/livefyre/c_gdpr_compliance.html) | Livefyreを使用してGDPRにアクセスし、リクエストを削除する手順。 |
| [Adobe Experience Platform Launch](https://docs.adobelaunch.com/client-side-information/deploy-javascript-tags-to-opt-in-to-launch) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを導入する方法。 |
| [Adobe Social](https://marketing.adobe.com/resources/help/en_US/social/c_gdpr-request.html) | GDPRリクエストフォームを使用して、Socialで収集されたデータにアクセスまたは削除する手順です。 |
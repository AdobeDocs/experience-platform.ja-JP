---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーサービスおよびExperience Cloudアプリケーション
topic: overview
translation-type: tm+mt
source-git-commit: 14cd3d17c7d9ba602d02925abddec9e0b246a8c8

---


# プライバシーサービスおよびExperience Cloudアプリケーション

Adobe Experience Platformプライバシーサービスは、複数のAdobe Experience Cloudアプリケーションのプライバシー要求をサポートするように設計されています。 各アプリケーションは、データサブジェクトを識別するために、異なる製品値とIDをサポートしています。

このドキュメントは、プライバシー関連の操作用にそのアプリケーションを設定する方法を概説したExperience Cloudアプリケーションドキュメントのリファレンスとして機能します。 データの形式設定やラベル付けの方法も含まれます。 対象となるアプリケーションのカテゴリは2つです。

* [Applications integrated with Privacy Service](#integrated): プライバシーサービスにアクセス、削除、またはオプトアウトの要求を送信できるアプリケーション。
* [セルフサービスアプリケーション](#self-serve): 内部でプライバシー要求を管理する必要があり、プライバシーサービスと直接通信できないアプリケーション。

プライバシーリクエストの書式設定方法およびサポートされる値については、Experience Cloudアプリケーションのドキュメントを参照してください。

## Privacy Serviceと統合されたアプリケーション {#integrated}

次のリストは、プライバシーサービスと統合されるExperience Cloudアプリケーションの機能です。この機能には、互換性のあるプライバシーサービス機能や、詳細についてのドキュメントへのリンクが含まれています。

| アプリケーション | アクセス/削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
--- | :---: | :---: | ---
| Adobe Advertising Cloud | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/en/advertising-cloud/all/privacy/ad-cloud-gdpr.html) </li><li>Advertising Cloudは、アドビプライバシーセンターが提供する既存のグローバルオプトアウト機能を利用します。 詳しくは、データのプライバシー [要求に関するガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#opt-out-requests) 。</li></ul> |
| Adobe Analytics | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/en/analytics/admin/data-governance/an-gdpr-overview.html)</li><li>Analyticsは、 [プライバシーレポート変数を使用してオプトアウト要求を処理します](https://docs.adobe.com/content/help/ja-JP/analytics/admin/data-governance/consent-variables.html)</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/ja-JP/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://docs.adobe.com/content/help/en/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | <ul><li>[ドキュメントへのアクセスと削除](https://docs.campaign.adobe.com/doc/standard/getting_started/en/ACS_GDPR.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/honoring-opt-outs.md)</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | <ul><li>[Data Lakeのドキュメントへのアクセスと削除](../catalog/privacy.md)</li><li>[リアルタイムのお客様プロファイル向けドキュメントへのアクセスと削除](../profile/privacy.md)</li><li>Experience Platformは、オーディエンスセグメントに対する [オプトアウト要求に従います](../segmentation/honoring-opt-outs.md)。</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | <ul><li>[ドキュメントへのアクセスと削除](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>Primetimeにはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |
| Adobe Target | ✓ | なし | <ul><li>[ドキュメントへのアクセスと削除](https://docs.adobe.com/content/help/ja-JP/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.translate.html)</li><li>ターゲットにはデータを転送する機能がないので、オプトアウトオブセールのリクエストは適用されません。</li></ul> |

<!-- (To include once access/delete documentation is available)
Adobe Customer Attributes (CRS) | ✓ | N/A | <ul><li>Customer Attributes does not have the capability to transfer data, therefore opt-out-of-sale requests are not applicable.</li></ul>
-->

## セルフサービスアプリケーション {#self-serve}

以下は、プライバシーサービスと統合されておらず、内部でプライバシーに関する問題を管理する必要があるExperience Cloudアプリケーションのリストです。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://docs.campaign.adobe.com/doc/AC/getting_started/JA/ACC_GDPR.html) | Adobe Campaign・クラシックのGDPR機能の概要 |
| [Adobe Dynamic Tag Manager](https://docs.adobe.com/content/help/en/dtm/using/tools/opt-in.html) | 同意が取得されるまでAdobeタグが実行されないようにする手順です。 |
| [Adobe Experience Manager](https://helpx.adobe.com/experience-manager/6-4/managing/using/gdpr-compliance.html) | 顧客のプライバシー管理者またはAEM管理者がGDPR要求を処理する方法の概要を示します。 |
| [Adobe Experience Manager Livefyre](https://docs.adobe.com/content/help/en/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyreを使用したGDPRへのアクセスおよび削除リクエストの手順です。 |
| [Adobe Experience Platform Launch](https://docs.adobelaunch.com/client-side-information/deploy-javascript-tags-to-opt-in-to-launch) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを導入する方法。 |

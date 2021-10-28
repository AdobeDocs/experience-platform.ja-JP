---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceとExperience Cloudの適用
topic-legacy: overview
description: このドキュメントでは、プライバシー関連の操作用に様々なExperience Cloudアプリケーションを設定する方法に関するリファレンスを提供します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: f0dc33dcd4803f157e411d8baf3b2d2f96cea5e1
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 47%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリ

Adobe Experience Platform [!DNL Privacy Service] は、複数のAdobe Experience Cloudアプリケーションのプライバシーリクエストをサポートするように設計されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントは、 [!DNL Experience Cloud] プライバシー関連の操作用にアプリケーションを設定する方法を説明するアプリケーションドキュメントです。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Service](#integrated):にアクセス、削除またはオプトアウトのリクエストを送信できるアプリケーション [!DNL Privacy Service].
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

詳しくは、 [!DNL Experience Cloud] アプリケーションを使用して、プライバシーリクエストの形式を設定する方法と、それらのリクエストでサポートされる値を確認してください。

## と統合されたアプリケーション [!DNL Privacy Service] {#integrated}

以下は、 [!DNL Experience Cloud] と統合されたアプリケーション [!DNL Privacy Service]( [!DNL Privacy Service] 互換性のある機能と、詳細情報に関するドキュメントへのリンクです。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | ドキュメントと考慮事項 |
| --- | :---: | :---: | --- |
| Adobe Advertising Cloud | ✓ | ✓ | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPA のオプトアウトオブセールに関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/an-gdpr-overview.html?lang=ja)</li><li>[!DNL Analytics] は、[プライバシーレポート変数](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/consent-variables.html?lang=ja)を使用してオプトアウトリクエストを処理します。</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/consents.md)</li></ul> |
| Adobe顧客属性 (CRS) | ✓ | なし | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[ID サービスのアクセス/削除に関するドキュメント](../identity-service/privacy.md)</li><li>[リアルタイム顧客プロファイルのためのアクセス / 削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] 名誉 [オーディエンスセグメントのオプトアウトリクエスト](../segmentation/consents.md).</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](http://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>[!DNL Primetime] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | ✓ | なし | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html?lang=ja)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |

{style=&quot;table-layout:auto&quot;}

## セルフサービスアプリケーション {#self-serve}

以下は、 [!DNL Experience Cloud] と統合されていないアプリケーション [!DNL Privacy Service] 内部でプライバシーに関する懸念を管理する必要があります。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/privacy/privacy-management.html?lang=ja) | Adobe Campaign Classic の GDPR 機能の概要。 |
| [Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-64/managing/data-protection/data-protection-and-privacy.html) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://experienceleague.adobe.com/docs/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Magento](https://devdocs.magento.com/compliance/industry-compliance.html) | Magento Commerceのインストールが、特定のプライバシー法の要件に従っていることを確認します。 |
| [Marketo](https://www.marketo.com/company/trust/gdpr/) | プライバシー規制がMarketoにどのように適用されるかを説明します。 |
| [Adobe Experience Platform のタグ](../tags/ui/client-side/consent.md) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |
| [Workfront](https://www.workfront.com/privacy-notice) | Workfrontが個人データを収集する方法、およびデータ主体がフォームを使用してプライバシーリクエストを送信する方法について説明します。 |

{style=&quot;table-layout:auto&quot;}

---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceとExperience Cloudの適用
topic-legacy: overview
description: このドキュメントでは、プライバシー関連の操作用に様々なExperience Cloudアプリケーションを設定する方法に関するリファレンスを提供します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 36%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリ

Adobe Experience Platform [!DNL Privacy Service] は、複数のAdobe Experience Cloudアプリケーションのプライバシーリクエストをサポートするように設計されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントは、 [!DNL Experience Cloud] プライバシー関連の操作用にアプリケーションを設定する方法を説明するアプリケーションドキュメントです。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Service](#integrated):にアクセス、削除またはオプトアウトのリクエストを送信できるアプリケーション [!DNL Privacy Service].
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

詳しくは、 [!DNL Experience Cloud] アプリケーションを使用して、プライバシーリクエストの形式を設定する方法と、それらのリクエストでサポートされる値を確認してください。

## と統合されたアプリケーション [!DNL Privacy Service] {#integrated}

以下は、 [!DNL Experience Cloud] と統合されたアプリケーション [!DNL Privacy Service]( [!DNL Privacy Service] 互換性のある機能、削除リクエストの処理プロトコル、詳細はドキュメントへのリンクを参照してください。

>[!NOTE]
>
>統合されたすべての製品は、30 日以内にプライバシーリクエストに応答します。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | 削除動作 | ドキュメントおよびその他の考慮事項 |
| --- | :---: | :---: | --- | --- |
| Adobe Advertising Cloud | ✓ | ✓ | データ主体の Cookie ID またはデバイス ID が、Cookie に関連付けられたすべてのコスト、クリック数および売上高データと共にシステムから削除されます。 | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPA のオプトアウトオブセールに関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | If `analyticsDeleteMethod` が省略された場合、またはに設定された場合 `anonymize` プライバシーリクエストをおこなう場合、特定のユーザー ID のコレクションによって参照されるすべてのデータは匿名になります。 If `analyticsDeleteMethod` が `purge`を指定した場合、すべてのデータが完全に削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/an-gdpr-overview.html?lang=ja)</li><li>[!DNL Analytics] は、[プライバシーレポート変数](https://experienceleague.adobe.com/docs/analytics/admin/data-governance/consent-variables.html?lang=ja)を使用してオプトアウトリクエストを処理します。</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | リクエストに含まれている特性識別子に関連付けられているAudience Managerとセグメントがすべて削除されます。 さらに、個人に対する各識別子は以降のデータ収集からオプトアウトされ、それぞれの ID マッピングは削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html)</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/consents.md)</li></ul> |
| Adobe顧客属性 (CRS) | ✓ | なし | データ主体の属性がシステムから削除されます。 | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | Experience Platform が Privacy Service から削除リクエストを受信すると、Platform は、Privacy Service に対し、リクエストを受信し、影響を受けるデータが削除用にマークされている旨の確認を送信します。プライバシージョブが完了すると、レコードはデータレイクまたはプロファイルストアから削除されます。 ジョブが完了する前に、データはソフト削除されるので、どの Platform サービスからもアクセスできません。 | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[ID サービスのアクセス/削除に関するドキュメント](../identity-service/privacy.md)</li><li>[リアルタイム顧客プロファイルのアクセス/削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] 名誉 [オーディエンスセグメントのオプトアウトリクエスト](../segmentation/consents.md).</li></ul> |
| Adobe Primetime Authentication | ✓ | なし | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>[!DNL Primetime] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | ✓ | なし | データ主体の ID に関連付けられているすべてのデータは、訪問者プロファイルから削除されます。 個人を特定しない、またはコンテンツデータなどとは無関係な集計データや匿名化データは、削除リクエストには適用されません。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html?lang=ja)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Marketo Engage | ✓ | なし | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/privacy-requests.html)</li><li>[!DNL Marketo] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |

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

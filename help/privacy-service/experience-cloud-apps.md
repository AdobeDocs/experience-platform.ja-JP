---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceとExperience Cloudの適用
description: このドキュメントでは、プライバシー関連の操作用に様々なExperience Cloudアプリケーションを設定する方法に関するリファレンスを提供します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 33%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリ

Adobe Experience Platform [!DNL Privacy Service] は、複数のAdobe Experience Cloudアプリケーションのプライバシーリクエストをサポートするように設計されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントは、 [!DNL Experience Cloud] プライバシー関連の操作用にアプリケーションを設定する方法を説明するアプリケーションドキュメントです。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Serviceと統合](#integrated)：アクセス、削除またはオプトアウトのリクエストをに送信できるアプリケーション。 [!DNL Privacy Service].
* [セルフサービスアプリケーション](#self-serve)[!DNL Privacy Service]：プライバシーリクエストを内部で管理する必要があり、 と直接通信できないアプリケーション。

次のドキュメントを確認してください： [!DNL Experience Cloud] アプリケーションを使用して、プライバシーリクエストの形式を設定する方法と、それらのリクエストでサポートされる値を確認してください。

## と統合されたアプリケーション [!DNL Privacy Service] {#integrated}

以下は、 [!DNL Experience Cloud] と統合されたアプリケーション [!DNL Privacy Service]( [!DNL Privacy Service] 互換性のある機能、削除リクエストの処理プロトコル、詳細はドキュメントへのリンクを参照してください。

>[!NOTE]
>
>統合されたすべての製品は、30 日以内にプライバシーリクエストに応答します。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | 削除動作 | ドキュメントおよびその他の考慮事項 |
| --- | :---: | :---: | --- | --- |
| Adobe Advertising Cloud | ✓ | ✓ | データ主体の Cookie ID またはデバイス ID が、Cookie に関連付けられたすべてのコスト、クリック数および売上高データと共にシステムから削除されます。 | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPA のオプトアウトオブセールに関するドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | Adobe Analytics では、計測データのプライバシー性と契約上の制限に従ってデータをラベルで分類するためのツールの提供を開始します。ラベルは、次の場合に重要な手順です。<ol><li>データ主体の識別。</li><li>アクセスリクエストの一部として返すデータの決定。</li><li>削除リクエストの一環として削除する必要があるデータフィールドの識別。</li></ol> | <ul><li>[プライバシーワークフロー](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/an-gdpr-workflow.html?lang=ja)</li><li>[Analytics のラベル付け](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/data-labels/gdpr-labels.html)</li><li>[Analytics オプトアウト](https://experienceleague.adobe.com/docs/analytics/components/dimensions/cm-opt-out.html)</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | リクエストに含まれている特性識別子に関連付けられているAudience Managerとセグメントがすべて削除されます。 さらに、個人に対する各識別子は以降のデータ収集からオプトアウトされ、それぞれの ID マッピングは削除されます。 | <ul><li>[アクセス](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#access-data) / [削除](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#delete-data) ドキュメント</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html#opt-out-calls)</li><li>[オプトアウトリクエスト](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#opt-out-requests)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/consents.md)</li></ul> |
| Adobe顧客属性 (CRS) | ✓ | なし | データ主体の属性がシステムから削除されます。 | <ul><li>[GDPR 向けのアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html?lang=ja)</li><li>[CCPA のアクセス/削除に関するドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html?lang=ja)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | Experience Platform が Privacy Service から削除リクエストを受信すると、リクエストが受信され、影響を受けるデータに削除マークが付けられた旨の確認を Platform が Privacy Service に送信します。プライバシージョブが完了すると、レコードはデータレイクまたはプロファイルストアから削除されます。 ジョブが完了する前に、データはソフト削除されるので、どの Platform サービスからもアクセスできません。 | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[ID サービスのアクセス/削除に関するドキュメント](../identity-service/privacy.md)</li><li>[リアルタイム顧客プロファイルのアクセス/削除に関するドキュメント](../profile/privacy.md)</li><li>[!DNL Experience Platform] 名誉 [オーディエンスセグメントのオプトアウトリクエスト](../segmentation/consents.md).</li></ul> |
| Adobe Pass 認証 | ✓ | なし | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>パスにはデータを転送する機能がないので、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Adobe Target | ✓ | なし | データ主体の ID に関連付けられているすべてのデータは、訪問者プロファイルから削除されます。 個人を特定しない、またはコンテンツデータなどとは無関係な集計データや匿名化データは、削除リクエストには適用されません。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html?lang=ja)</li><li>[!DNL Target] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |
| Marketo Engage | ✓ | なし | データ主体に保存されているデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/privacy-requests.html)</li><li>[!DNL Marketo] にはデータを転送する機能がないため、販売のオプトアウトリクエストは利用できません。</li></ul> |

{style="table-layout:auto"}

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

{style="table-layout:auto"}

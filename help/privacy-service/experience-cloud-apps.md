---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceおよびExperience Cloudアプリケーション
description: このドキュメントでは、プライバシー関連の操作に対して様々なExperience Cloudアプリケーションを設定する方法について説明します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: 46ca46460de9211c3e876454c986d030b964646e
workflow-type: tm+mt
source-wordcount: '822'
ht-degree: 26%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリケーション

Adobe Experience Platform [!DNL Privacy Service] は、複数のAdobe Experience Cloud アプリケーションのプライバシーリクエストをサポートするように構築されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントは、次の場合に参考になります [!DNL Experience Cloud] プライバシー関連操作用にアプリケーションを設定する方法の概要を説明するアプリケーションドキュメント。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Serviceと統合されたアプリケーション](#integrated)：アクセス、削除またはオプトアウト要求をに送信できるアプリケーション [!DNL Privacy Service].
* [セルフサービスアプリケーション](#self-serve)：プライバシーリクエストを内部で管理する必要があり、と通信できないアプリケーション [!DNL Privacy Service] 直接。

のドキュメントを確認してください [!DNL Experience Cloud] プライバシーリクエストのフォーマット方法と、それらのリクエストでサポートされている値を学ぶためのアプリケーション。

## と統合されたアプリケーション [!DNL Privacy Service] {#integrated}

以下は、 [!DNL Experience Cloud] と統合されたアプリケーション [!DNL Privacy Service]を含む [!DNL Privacy Service] と互換性のある機能、削除リクエストを処理するためのプロトコル、詳細についてはドキュメントへのリンクを示します。

>[!NOTE]
>
>すべての統合製品は、プライバシーリクエストに 30 日以内に応答します。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | 動作を削除 | ドキュメントとその他の考慮事項 |
| --- | :---: | :---: | --- | --- |
| Adobe Advertising Cloud | ✓ | ✓ | データ主体の cookie ID またはデバイス ID が、その cookie に関連付けられたすべてのコスト、クリック数、売上高のデータと共に、システムから削除されます。 | <ul><li>[GDPR のドキュメントへのアクセス/削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html)</li><li>[CCPA に関するドキュメントへのアクセス /削除](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html)</li><li>[CCPA の販売オプトアウトドキュメント](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html)</li></ul> |
| Adobe Analytics | ✓ | ✓ | Adobe Analyticsには、データの機密性と契約上の制限に従ってデータをラベル付けするためのツールが用意されています。 ラベルは、次の場合に重要な手順です。<ol><li>データ主体の識別。</li><li>アクセスリクエストの一部として返すデータの決定。</li><li>削除リクエストの一環として削除する必要があるデータフィールドを識別します。</li></ol> | <ul><li>[プライバシーワークフロー](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/an-gdpr-workflow.html)</li><li>[Analytics のラベル付け](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/data-labels/gdpr-labels.html)</li><li>[Analytics オプトアウト](https://experienceleague.adobe.com/docs/analytics/components/dimensions/cm-opt-out.html)</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | リクエストに含まれているAudience Manager ID に関連付けられているすべての特性とセグメントが削除されます。 また、個々の識別子は、更なるデータ収集からオプトアウトされ、各 ID マッピングが除去される。 | <ul><li>[アクセス](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#access-data) / [削除](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#delete-data) 詳細を見る</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html#opt-out-calls)</li><li>[オプトアウトリクエスト](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html#opt-out-requests)</li></ul> |
| Adobe Campaign Classic | ✓ | ✓ | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[プライバシーの管理](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/privacy/privacy-management.html?lang=ja).</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://helpx.adobe.com/jp/campaign/kb/campaign-privacy.html)</li><li>[オプトアウトに関するドキュメント](../segmentation/consents.md)</li></ul> |
| Adobe顧客属性（CRS） | ✓ | なし | データ主体の属性がシステムから削除されます。 | <ul><li>[GDPR のドキュメントへのアクセス/削除](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html?lang=ja)</li><li>[CCPA に関するドキュメントへのアクセス /削除](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html?lang=ja)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | Experience Platform が Privacy Service から削除リクエストを受信すると、リクエストが受信され、影響を受けるデータに削除マークが付けられた旨の確認を Platform が Privacy Service に送信します。プライバシージョブが完了すると、レコードはその後、データレイクまたはプロファイルストアから削除されます。 ジョブが完了する前に、データはソフト削除されるので、どの Platform サービスからもアクセスできません。 | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[ID サービスのドキュメントへのアクセス/削除](../identity-service/privacy.md)</li><li>[リアルタイム顧客プロファイルのドキュメントへのアクセス/削除](../profile/privacy.md)</li><li>[!DNL Experience Platform] オナーズ [オーディエンスセグメントのオプトアウトリクエスト](../segmentation/consents.md).</li></ul> |
| Adobe Journey Optimizer | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/en/docs/journey-optimizer/using/privacy/requests)</li></ul> |
| Adobe Pass 認証 | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>パスにはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Adobe Target | ✓ | なし | データ主体の ID に関連付けられているすべてのデータは、訪問者プロファイルから削除されます。 個人を識別しない、またはその他の無関係な集計データまたは匿名化データ（コンテンツデータなど）は、削除リクエストには適用されません。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html?lang=ja)</li><li>[!DNL Target] にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Marketo Engage | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/privacy-requests.html)</li><li>[!DNL Marketo] にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |

{style="table-layout:auto"}

## セルフサービスアプリケーション {#self-serve}

以下は、 [!DNL Experience Cloud] と統合されていないアプリケーション [!DNL Privacy Service] また、プライバシーに関する懸念を内部で管理する必要があります。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-64/managing/data-protection/data-protection-and-privacy.html) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://experienceleague.adobe.com/docs/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Magento](https://devdocs.magento.com/compliance/industry-compliance.html) | Magento Commerceのインストールが特定のプライバシー法律の要件に準拠していることを確認します。 |
| [Adobe Experience Platformのタグ](../tags/ui/client-side/consent.md) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |
| [Workfront](https://www.workfront.com/privacy-notice) | Workfrontが個人データを収集する方法と、データ主体がフォームを使用してプライバシーリクエストを送信する方法について説明します。 |

{style="table-layout:auto"}

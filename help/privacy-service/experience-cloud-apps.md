---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Privacy ServiceおよびExperience Cloud アプリケーション
description: このドキュメントでは、様々なExperience Cloud アプリケーションをプライバシー関連の業務用に設定する方法について説明します。
exl-id: da21c15f-0b99-4eb7-ac9a-f0fe5e3ba842
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 21%

---

# [!DNL Privacy Service] および [!DNL Experience Cloud] アプリケーション

Adobe Experience Platform [!DNL Privacy Service] は、複数のAdobe Experience Cloud アプリケーションのプライバシーリクエストをサポートするように構築されています。 各アプリケーションは、データ主体を識別するために、異なる製品値と ID をサポートしています。

このドキュメントでは、アプリケーションのドキュメント [!DNL Experience Cloud] 参照して、プライバシー関連の操作に合わせてアプリケーションを設定する方法の概要を説明します。 データの形式設定やラベル付けの方法も含まれます。次の 2 つのカテゴリのアプリケーションを対象としています。

* [Privacy Serviceと統合されたアプリケーション &#x200B;](#integrated)：アクセス、削除またはオプトアウトリクエストを [!DNL Privacy Service] に送信できるアプリケーション。
* [&#x200B; セルフサービスアプリケーション &#x200B;](#self-serve)：プライバシーリクエストを内部で管理する必要があり、[!DNL Privacy Service] ーザーと直接通信できないアプリケーション。

プライバシーリクエストの形式を設定する方法と、[!DNL Experience Cloud] れらのリクエストでサポートされている値については、使用しているプライバシーアプリケーションのドキュメントを参照してください。

## [!DNL Privacy Service] と統合されたアプリケーション {#integrated}

[!DNL Privacy Service] と統合されている [!DNL Experience Cloud] アプリケーションのリストを以下に示します。これらのアプリケーションには、互換性のある [!DNL Privacy Service] 機能、削除リクエストを処理するためのプロトコル、詳細を確認するためのドキュメントへのリンクが含まれています。

>[!NOTE]
>
>すべての統合製品は、プライバシーリクエストに 30 日以内に応答します。

| アプリケーション | アクセス / 削除 | 販売のオプトアウト | 動作を削除 | ドキュメントとその他の考慮事項 |
| --- | :---: | :---: | --- | --- |
| Adobe Advertising Cloud | ✓ | ✓ | データ主体の cookie ID またはデバイス ID が、その cookie に関連付けられたすべてのコスト、クリック数、売上高のデータと共に、システムから削除されます。 | <ul><li>[GDPR のドキュメントへのアクセス/削除 &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-gdpr.html?lang=ja)</li><li>[CCPA に関するドキュメントへのアクセス /削除 &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-access-delete.html?lang=ja)</li><li>[CCPA の販売オプトアウトドキュメント &#x200B;](https://experienceleague.adobe.com/docs/advertising-cloud/privacy/ad-cloud-ccpa-opt-out-of-sale.html?lang=ja)</li></ul> |
| Adobe Analytics | ✓ | ✓ | Adobe Analyticsには、データの機密性と契約上の制限に従ってデータをラベル付けするためのツールが用意されています。 ラベルは、次の場合に重要な手順です。<ol><li>データ主体の識別。</li><li>アクセスリクエストの一部として返すデータの決定。</li><li>削除リクエストの一環として削除する必要があるデータフィールドを識別します。</li></ol> | <ul><li>[&#x200B; プライバシーワークフロー &#x200B;](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/an-gdpr-workflow.html?lang=ja)</li><li>[Analytics のラベル付け &#x200B;](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/data-governance/data-labels/gdpr-labels.html?lang=ja)</li><li>[Analytics オプトアウト &#x200B;](https://experienceleague.adobe.com/docs/analytics/components/dimensions/cm-opt-out.html?lang=ja)</li></ul> |
| Adobe Audience Manager | ✓ | ✓ | リクエストに含まれているAudience Manager ID に関連付けられているすべての特性とセグメントが削除されます。 また、個々の識別子は、更なるデータ収集からオプトアウトされ、各 ID マッピングが除去される。 | <ul><li>[&#x200B; アクセス &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html?lang=ja#access-data)/[&#x200B; 削除 &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html?lang=ja#delete-data) ドキュメント</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/declared-ids.html?lang=ja#opt-out-calls)</li><li>[&#x200B; オプトアウトリクエスト &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/data-privacy-requests.html?lang=ja#opt-out-requests)</li></ul> |
| Adobe Campaign Classic | ✓ | ✓ | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[プライバシーの管理](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/privacy/privacy-management.html?lang=ja)</li></ul> |
| Adobe Campaign Standard | ✓ | ✓ | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/ja/docs/campaign-standard/using/getting-started/privacy/privacy-management#right-access-forgotten)</li><li>[オプトアウトに関するドキュメント](https://experienceleague.adobe.com/ja/docs/campaign-standard/using/profiles-and-audiences/understanding-opt-in-and-opt-out-processes/about-opt-in-and-opt-out-in-campaign)</li></ul> |
| Adobe顧客属性（CRS） | ✓ | なし | データ主体の属性がシステムから削除されます。 | <ul><li>[GDPR のドキュメントへのアクセス/削除 &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/gdpr.html?lang=ja)</li><li>[CCPA に関するドキュメントへのアクセス /削除 &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/ccpa.html?lang=ja)</li><li>顧客属性にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Adobe Experience Platform | ✓ | ✓ | Experience PlatformがPrivacy Serviceから削除リクエストを受信すると、Experience PlatformからPrivacy Serviceに、リクエストが受信され、影響を受けるデータに削除用のマークが付けられた旨の確認が送信されます。 プライバシージョブが完了すると、レコードはその後、データレイクまたはプロファイルストアから削除されます。 ジョブが完了すると、データはソフト削除されるので、どのExperience Platform サービスからもアクセスできません。 | <ul><li>[データレイクのためのアクセス / 削除に関するドキュメント](../catalog/privacy.md)</li><li>[ID サービスのドキュメントへのアクセス/削除 &#x200B;](../identity-service/privacy.md)</li><li>[&#x200B; リアルタイム顧客プロファイルのドキュメントへのアクセス/削除 &#x200B;](../profile/privacy.md)</li><li>[!DNL Experience Platform] は [&#x200B; オーディエンスセグメントのオプトアウトリクエスト &#x200B;](../segmentation/tutorials/consents.md) に従います。</li></ul> |
| Adobe Journey Optimizer | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/privacy/requests)</li></ul> |
| Adobe Pass 認証 | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://tve.helpdocsonline.com/how-to-make-a-privacy-request)</li><li>パスにはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Adobe Target | ✓ | なし | データ主体の ID に関連付けられているすべてのデータは、訪問者プロファイルから削除されます。 個人を識別しない、またはその他の無関係な集計データまたは匿名化データ（コンテンツデータなど）は、削除リクエストには適用されません。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/target/using/implement-target/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.html?lang=ja)</li><li>[!DNL Target] にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |
| Commerce（Personalization） | ✓ | なし | Privacy Service[!DNL Commerce]、マーケティング目的でCommerce SaaS サービスに保存されたデータを削除します。つまり、データ主体のプロファイルと注文は、キャンペーンやカスタマージャーニーで使用するためにAdobe マーケティングアプリケーションに送信されなくなりました。 ただし、マーチャントトランザクションのニーズに引き続き必要となる可能性があるため、Privacy Serviceは [!DNL Commerce] アプリケーション内のデータを削除しません。 マーチャントは、[!DNL Commerce] アプリケーション内のすべてのデータ削除/アクセスリクエストに対して責任を負います。 | <ul><li>[Commerceに関するドキュメントへのアクセスまたは削除 &#x200B;](https://experienceleague.adobe.com/ja/docs/commerce-merchant-services/data-connection/handle-privacy-request)</li></ul> |
| Marketo Engage | ✓ | なし | データ主体の保存されたデータがシステムから削除されます。 | <ul><li>[アクセス / 削除に関するドキュメント](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/miscellaneous/privacy-requests.html?lang=ja)</li><li>[!DNL Marketo] にはデータを転送する機能がないので、販売のオプトアウトリクエストは適用されません。</li></ul> |

{style="table-layout:auto"}

## セルフサービスアプリケーション {#self-serve}

以下は、[!DNL Privacy Service] と統合されておらず、プライバシーに関する懸念を内部で管理する必要がある [!DNL Experience Cloud] アプリケーションのリストです。 各アプリケーションのドキュメントへのリンクと、ドキュメントの内容に関する説明が記載されています。

| アプリケーション | ドキュメントの説明 |
| ------- | ----------- |
| [Adobe Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-64/managing/data-protection/data-protection-and-privacy.html?lang=ja) | 顧客プライバシー管理者または AEM 管理者が GDPR リクエストを処理する方法の概要。 |
| [Adobe Experience Manager Livefyre](https://experienceleague.adobe.com/docs/livefyre/using/settings-other/privacy-requests/c-gdpr-compliance.html?lang=ja) | Livefyre を使用して GDPR にアクセスしたり、リクエストを削除したりする手順。 |
| [Adobe Commerce](https://experienceleague.adobe.com/ja/docs/commerce-operations/security-and-compliance/overview) | Adobe Commerceのインストールが特定のプライバシー法の要件に準拠していることを確認します。 |
| [Adobe Experience Platformのタグ &#x200B;](../tags/ui/client-side/consent.md) | 拡張機能とルールビルダーを使用してオプトインおよびオプトアウトソリューションを定義する方法。 |
| [Workfront](https://www.workfront.com/privacy-notice) | Workfrontが個人データを収集する方法と、データ主体がフォームを使用してプライバシーリクエストを送信する方法について説明します。 |

{style="table-layout:auto"}

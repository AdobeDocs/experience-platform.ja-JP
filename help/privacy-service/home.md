---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platform Privacy Service
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 10%

---


# Adobe Experience Platform Privacy Serviceの概要

顧客体験をより良くするためには、顧客の個人データを収集して保存する必要があります。 このデータを使用する場合、顧客のプライバシーを理解し、尊重することが重要です。 新しい法規制や組織規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を持つようになります。

Adobe Experience Platform Privacy Serviceは、RESTful APIとユーザーインターフェイスを提供し、顧客からのこれらのデータリクエストを管理するのに役立ちます。 Privacy Serviceにより、個人または個人の顧客データにアクセスするリクエストをAdobe Experience Cloudアプリケーションから送信したり、削除したりできます。これにより、法的および組織のプライバシーに関する規制への準拠を自動化できます。

## なぜPrivacy Service?

Privacy Serviceは、顧客の個人データの管理に必要なビジネスの方法が根本的に変化したことに応えて開発されました。 Privacy Serviceの主な目的は、データのプライバシーに関する規制へのコンプライアンスを自動化することです。プライバシーに関する規制に違反した場合、大きな罰金が課され、ビジネスに関するデータの運用が妨げられる可能性があります。

### Privacy ServiceとGDPR

[](https://eugdpr.org/)一般データ保護規則（GDPR）は、EU 加盟国に対して、**アクセス権**&#x200B;や&#x200B;**忘れられる権利**&#x200B;など、新たなデータプライバシー権を導入しました。つまり、御社が個人データを収集した EU 市民は、いつでもデータのアクセスや削除を要求できます。30日以内にこれらの要求に従わないと、組織に対して100万ドル以上の罰金が科せられる場合があります。

Privacy Serviceは、GDPRに対するアクセスおよび削除の要求をサポートし、CCPA規制に基づく要求とは別に追跡します。 詳細は、 [GDPRのFAQ](gdpr/faq.md) と [用語](gdpr/terminology.md) ドキュメントを参照してください。

### Privacy ServiceとCCPA

The [California Consumer Privacy Act](https://www.caprivacy.org/about) (CCPA) enhances privacy rights and consumer protection for residents of California, United States. CCPAは、カリフォルニア州在住者に対し、個人データのアクセス権や削除権、個人データの販売・公開権（および相手）権、データを第三者に販売する権利など、新しいデータのプライバシー権オプトアウトを提供する。

Privacy Serviceは、CCPA規制のアクセスおよび削除要求をサポートし、GDPR要求とは別に追跡します。 また、Privacy Serviceは、オプトアウトオブセールをサポートするExperience Cloudアプリケーションに対するオプトアウト要求の送信もサポートしています。 詳しくは、 [CCPA FAQ](ccpa/faq.md) （CCPAに関するFAQ）を参照してください。

## Privacy Serviceを使用してプライバシージョブの要求を管理する方法

Privacy ServiceにはRESTful APIとユーザーインターフェイスが用意されており、プライベートデータへのアクセス/削除や、販売中止( **プライバシージョブ**)に対する顧客の要求を管理できます。 また、このサービスは、Experience Cloudアプリケーションに関するプライバシージョブの状態と結果を表示するための、中央の監査とログのメカニズムも提供します。

>[!NOTE]
>
>現在、オプトアウトリクエストはPrivacy ServiceAPIでのみサポートされています。

### APIの使用

[Privacy ServiceAPI](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) :RESTful API呼び出しを使用して、プライバシージョブを作成および管理できます。これにより、Experience Cloudアプリケーションのプライバシー規制への準拠をプログラム的にアプローチできます。 APIの使用方法に関する詳細な手順については、 [Privacy ServiceAPI開発者ガイドを参照してください](api/getting-started.md)。

### UIの使用

Privacy ServiceUIを使用すると、グラフィカルインターフェイスを使用してプライバシージョブを作成および監視できます。 UIには、すべてのアクティブなリクエストのステータスを視覚的に表す **ステータスレポート** Widgetが含まれており、組み込みのRequest Builderを使用するか、JSONファイルをアップロードして新しい **** リクエストを作成できます。 UIの使用について詳しくは、『 [Privacy Serviceユーザーガイド](ui/overview.md)』を参照してください。
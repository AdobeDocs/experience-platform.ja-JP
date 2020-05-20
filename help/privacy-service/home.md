---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformプライバシーサービス
topic: overview
translation-type: tm+mt
source-git-commit: 66fef5b98d2c21d1ac8b272e2c8557d26daa3364
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 10%

---


# Adobe Experience Platformプライバシーサービスの概要

顧客体験をより良くするためには、顧客の個人データを収集して保存する必要があります。 このデータを使用する場合、顧客のプライバシーを理解し、尊重することが重要です。 新しい法規制や組織規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり削除したりする権利を持つようになります。

Adobe Experience Platform Privacy Serviceは、RESTful APIとユーザーインターフェイスを備えており、お客様からのこれらのデータリクエストを管理するのに役立ちます。 プライバシーサービスを使用すると、Adobe Experience Cloudアプリケーションから個人または個人の顧客データにアクセスする要求を送信したり、データを削除したりできます。これにより、法律や組織のプライバシーに関する規制への準拠を自動化できます。

## プライバシーサービスを利用する理由

プライバシーサービスは、顧客の個人データを管理するためにビジネスに必要な方法が根本的に変化したことに対応して開発されました。 プライバシーサービスの主な目的は、データのプライバシーに関する規制への準拠を自動化することです。プライバシーに違反した場合、大きな罰金が課され、ビジネスに関するデータ操作が妨げられる可能性があります。

### プライバシー・サービスとGDPR

[](https://eugdpr.org/)一般データ保護規則（GDPR）は、EU 加盟国に対して、**アクセス権**&#x200B;や&#x200B;**忘れられる権利**&#x200B;など、新たなデータプライバシー権を導入しました。つまり、御社が個人データを収集した EU 市民は、いつでもデータのアクセスや削除を要求できます。30日以内にこれらの要求に従わないと、組織に対して100万ドル以上の罰金が科せられる場合があります。

プライバシー・サービスは、GDPRに対するアクセスおよび削除の要求をサポートし、CCPA規制に基づく要求とは別に追跡します。 詳細は、 [GDPRのFAQ](gdpr/faq.md) と [用語](gdpr/terminology.md) ドキュメントを参照してください。

### プライバシーサービスとCCPA

The [California Consumer Privacy Act](https://www.caprivacy.org/about) (CCPA) enhances privacy rights and consumer protection for residents of California, United States. CCPAは、カリフォルニア州在住者に対し、個人データのアクセス権や削除権、個人データの販売・公開権（および相手）権、データを第三者に販売する権利など、新しいデータのプライバシー権オプトアウトを提供する。

プライバシー・サービスは、CCPA規制に対するアクセスおよび削除の要求をサポートし、GDPR要求とは別に追跡します。 プライバシーサービスは、それらをサポートするExperience Cloudアプリケーションのオプトアウト要求の送信もサポートしています。 詳しくは、 [CCPA FAQ](ccpa/faq.md) （CCPAに関するFAQ）を参照してください。

## プライバシーサービスを使用してプライバシージョブの要求を管理する方法

プライバシーサービスは、RESTful APIとユーザーインターフェイスを提供し、プライベートデータへのアクセスと削除、または販売中止の要求( **プライバシージョブ**)を管理できます。 また、このサービスは監査とログの中央メカニズムも提供します。このメカニズムを使用すると、Experience Cloudアプリケーションに関するプライバシージョブのステータスと結果を表示できます。

>[!NOTE] オプトアウト要求は、現在、プライバシーサービスAPIでのみサポートされています。

### APIの使用

Privacy Service API [(](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) プライバシーサービスAPI)を使用すると、RESTful API呼び出しを使用してプライバシージョブを作成および管理でき、Experience Cloudアプリケーションのプライバシー規制への準拠をプログラム的にアプローチできます。 APIの使用方法に関する詳細な手順については、 [プライバシーサービスAPI開発者ガイドを参照してください](api/getting-started.md)。

### UIの使用

プライバシーサービスのUIを使用すると、グラフィカルインターフェイスを使用してプライバシージョブを作成および監視できます。 UIには、すべてのアクティブなリクエストのステータスを視覚的に表す **ステータスレポート** Widgetが含まれており、組み込みのRequest Builderを使用するか、JSONファイルをアップロードして新しい **** リクエストを作成できます。 UIの使用について詳しくは、 [プライバシーサービスユーザーガイドを参照してください](ui/overview.md)。
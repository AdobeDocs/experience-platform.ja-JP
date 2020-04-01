---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Adobe Experience Platformプライバシーサービス
topic: overview
translation-type: tm+mt
source-git-commit: 66fef5b98d2c21d1ac8b272e2c8557d26daa3364

---


# Adobe Experience Platformプライバシーサービスの概要

顧客体験をより良くするためには、顧客の個人データを収集し、保存する必要があります。 このデータを使用する場合、顧客のプライバシーを理解し、尊重することが重要です。 新しい法規制や組織の規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり、個人データを削除したりする権利が与えられます。

Adobe Experience Platform Privacy Serviceは、RESTful APIとユーザーインターフェイスを提供し、顧客からのこれらのデータリクエストを管理するのに役立ちます。 プライバシーサービスを使用すると、Adobe Experience Cloudアプリケーションから個人または個人の顧客データにアクセスする要求を送信したり、データを削除したりでき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

## プライバシーサービスを利用する理由

プライバシーサービスは、顧客の個人データを管理するために企業がどのように必要とされるかという基本的な変化に対応して開発されました。 プライバシーサービスの主な目的は、データのプライバシー規制への準拠を自動化することです。この規制に違反すると、重大な罰金が科され、ビジネスのデータ運用に支障を来す可能性があります。

### プライバシー・サービスとGDPR

[](https://eugdpr.org/)一般データ保護規則（GDPR）は、EU 加盟国に対して、**アクセス権**&#x200B;や&#x200B;**忘れられる権利**&#x200B;など、新たなデータプライバシー権を導入しました。つまり、御社が個人データを収集した EU 市民は、いつでもデータのアクセスや削除を要求できます。30日以内にこれらの要求に準拠しないと、組織に対して数百万ドルの罰金が科せられます。

プライバシー・サービスは、GDPRに対するアクセス要求と削除要求をサポートし、CCPA規則に基づく要求とは別に追跡を行います。 詳細は、 [GDPRのFAQと用語の](gdpr/faq.md)[ドキュメントを参照](gdpr/terminology.md) 。

### プライバシーサービスとCCPA

The [California Consumer Privacy Act](https://www.caprivacy.org/about) (CCPA) enhances privacy rights and consumer protection for residents of California, United States. CCPAは、自分の個人データにアクセスして削除する権利、自分の個人データが誰に売られたか（誰に）知る権利、第三者にデータを売り渡す権利など、カリフォルニア在住者に新しいデータのプライバシー権オプトアウトを提供する。

プライバシー・サービスは、CCPA規制のアクセスおよび削除要求をサポートし、GDPR要求とは別に追跡します。 また、プライバシーサービスでは、Experience Cloudアプリケーションの販売停止をサポートするオプトアウトリクエストの送信もサポートしています。 詳しくは、 [CCPAのFAQ](ccpa/faq.md) （よくある質問）を参照してください。

## プライバシーサービスを使用してプライバシージョブのリクエストを管理する方法

プライバシーサービスは、RESTful APIとユーザーインターフェイスを提供します。このAPIを使用すると、顧客のプライベートデータへのアクセス/削除や、販売中止のオプトアウト(プライバシージョブ ****)の要求を管理できます。 また、このサービスは、Experience Cloudアプリケーションに関するプライバシージョブのステータスと結果を表示できる、中央の監査およびログメカニズムも提供します。

>[!NOTE] オプトアウトリクエストは、現在、プライバシーサービスAPIでのみサポートされています。

### APIの使用

プライバ [シーサービスAPIを使用すると](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) 、RESTful API呼び出しを使用してプライバシージョブを作成および管理でき、Experience Cloudアプリケーションのプライバシー規制への準拠にプログラム的にアプローチできます。 APIの使用方法に関する詳細な手順については、プライバシーサービスAPI開発 [者ガイドを参照してください](api/getting-started.md)。

### UIの使用

プライバシーサービスUIを使用すると、グラフィカルインターフェイスを使用してプライバシージョブを作成および監視できます。 UIには、すべてのアクティブなリクエストのステータスを視覚的に表す **Status Report** Widgetが含まれており、組み込みの **** Request Builderを使用するか、JSONファイルをアップロードして新しいリクエストを作成できます。 UIの使用について詳しくは、プライバシーサービスのユーザーガ [イドを参照してください](ui/overview.md)。
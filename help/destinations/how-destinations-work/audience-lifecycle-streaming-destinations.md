---
title: Experience Platformとストリーミングの宛先におけるオーディエンスのライフサイクル
description: Experience Platformのオーディエンス名とマッピングがストリーミング宛先プラットフォームに反映される仕組みについて説明します。
source-git-commit: 6b4dfa714e078fb5b97900811aade081ffef0d78
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 3%

---


# ストリーミング宛先のオーディエンスライフサイクル

このページでは、Experience Platformのオーディエンス名の更新とマッピングをストリーミング宛先プラットフォームと同期する方法について説明します。 Experience Platformでオーディエンス名を変更したり、オーディエンスマッピングを削除したりする場合、動作は宛先プラットフォームの機能によって異なります。

これらの違いを理解することは、オーディエンスライフサイクルの運用を管理し、宛先プラットフォームにExperience Platformのオーディエンスの現在の状態を反映させるうえで重要です。

## オーディエンス名の伝達動作 {#audience-name-propagation}

ストリーミング宛先に対してオーディエンスをアクティブ化すると、最初のアクティベーション中にオーディエンス名が宛先に送信されます。 ただし、オーディエンス名の更新動作は、宛先によって異なります。

* **[オーディエンス名の更新をサポートする宛先](#name-update-supported)**:Experience Platformでオーディエンス名を変更すると、更新された名前がこれらの宛先に自動的に反映されます。
* **[オーディエンス名の更新をサポートしていない宛先](#name-update-not-supported)**:Experience Platformでオーディエンス名を変更した場合、宛先では最初のアクティベーションから引き続き元の名前が使用されます。

### オーディエンス名の更新をサポートする宛先 {#name-update-supported}

次のストリーミング宛先では、Experience Platformでオーディエンス名を変更する際の、オーディエンス名の自動更新をサポートしています。

* [Acxiom Audience 接続](../catalog/advertising/acxiom-audience-connection.md)
* [Adobe Campaign Managed Cloud](../catalog/email-marketing/adobe-campaign-managed-services.md)
* [Advertising Cloud DSP](../catalog/advertising/adobe-advertising-cloud-connection.md)
* [ボンボラ](../catalog/advertising/bombora.md)
* [Criteo](../catalog/advertising/criteo.md)
* [Demandbase](../catalog/advertising/demandbase.md)
* [Demandbase People](../catalog/advertising/demandbase-people.md)
* [Experience Cloud Audiences](../catalog/adobe/experience-cloud-audiences.md)
* [Facebook カスタムオーディエンス](../catalog/social/facebook.md)
* [Gainsight PX](../catalog/analytics/gainsight-px.md)
* [LINE](../catalog/mobile-engagement/line.md)
* [（会社）リンクインでマッチしたオーディエンス](../catalog/social/linkedin-b2b.md)
* [LinkedIn でマッチしたオーディエンス](../catalog/social/linkedin.md)
* [（従来）（V2）Marketo Engage](../catalog/adobe/marketo-engage.md)
* [PubMatic 接続](../catalog/advertising/pubmatic.md)
* [SendGrid](../catalog/email-marketing/sendgrid.md)
* [株式会社スナップ](../catalog/advertising/snap-inc.md)
* [TikTok](../catalog/social/tiktok.md)
* [Twitter カスタムオーディエンス](../catalog/social/twitter.md)
* [Yahoo DataX](../catalog/advertising/datax.md)

### オーディエンス名の更新をサポートしていない宛先 {#name-update-not-supported}

上記以外の宛先の場合、オーディエンス名は最初のアクティベーション後も静的なままです。 これらの宛先のオーディエンス名を更新する必要がある場合、

1. Experience Platformで目的の名前で新しいオーディエンスを作成します
2. 宛先に対して新しいオーディエンスをアクティブ化

>[!TIP]
>
>混乱を避けるため、特に、オーディエンス名の更新をサポートしていない宛先に対してアクティブ化する場合は、最初のアクティベーションから説明的なオーディエンス名を使用します。

## オーディエンスの削除をサポートする宛先 {#support-removal}

ストリーミング宛先からオーディエンスを削除（マッピング解除）すると、Experience Platformは、対応するオーディエンスを宛先プラットフォームから削除しようとします。 ただし、すべての宛先がこの機能をサポートしているわけではありません。

次のストリーミング宛先では、宛先からオーディエンスのマッピングを解除する際の、自動オーディエンス削除をサポートしています。

* [（API）Oracle Eloqua](../catalog/email-marketing/oracle-eloqua-api.md)
* [（会社）リンクインでマッチしたオーディエンス](../catalog/social/linkedin-b2b.md)
* [（従来）（V2）Marketo Engage](../catalog/adobe/marketo-engage.md)
* [Adobe Advertising Cloud DSP](../catalog/advertising/adobe-advertising-cloud-connection.md)
* [Bombora アカウントオーディエンス](../catalog/advertising/bombora.md)
* [Criteo](../catalog/advertising/criteo.md)
* [Experience Cloud Audiences](../catalog/adobe/experience-cloud-audiences.md)
* [Facebook](../catalog/social/facebook.md)
* [Gainsight PX](../catalog/analytics/gainsight-px.md)
* [HubSpot](../catalog/crm/hubspot.md)
* [LINE](../catalog/mobile-engagement/line.md)
* [LinkedIn でマッチしたオーディエンス](../catalog/social/linkedin.md)
* [LiveRamp – 配布](../catalog/advertising/liveramp-distribution.md)
* [Mailchimp の興味カテゴリ](../catalog/email-marketing/mailchimp-interest-categories.md)
* [PubMatic 接続](../catalog/advertising/pubmatic.md)
* [Salesforce Marketing Cloud アカウントのエンゲージメント](../catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md)
* [SendGrid](../catalog/email-marketing/sendgrid.md)
* [株式会社スナップ](../catalog/advertising/snap-inc.md)
* [TikTok](../catalog/social/tiktok.md)
* [Twitter カスタムオーディエンス](../catalog/social/twitter.md)
* [Yahoo DataX](../catalog/advertising/datax.md)

### オーディエンスの削除をサポートしていない宛先

上記以外の宛先については、オーディエンスのマッピングを宛先から解除すると、Experience Platformはマッピングのみを削除します。 宛先プラットフォームのオーディエンスは、パートナープラットフォームで手動で削除するまで、アクティブなままです。

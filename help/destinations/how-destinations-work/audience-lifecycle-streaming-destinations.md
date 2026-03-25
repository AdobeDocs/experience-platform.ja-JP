---
title: Experience Platformとストリーミングの宛先のオーディエンスライフサイクル
description: Experience Platformのオーディエンス名とマッピングが、ストリーミング宛先プラットフォームに反映される方法について説明します。
exl-id: 8a9a9e2f-d52f-41c9-ae27-9d2cd797bb85
source-git-commit: 381d1f952067cece9f9a9618a00bbed304214906
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 2%

---

# ストリーミング宛先のオーディエンスライフサイクル

このページでは、Experience Platformのオーディエンス名の更新とマッピングをストリーミング宛先プラットフォームと同期する方法について説明します。 Experience Platformでオーディエンス名を変更したり、オーディエンスマッピングを削除したりすると、動作は宛先プラットフォームの機能によって異なります。

これらの違いを理解することは、オーディエンスのライフサイクルオペレーションを管理し、宛先プラットフォームがExperience Platformでのオーディエンスの現状を反映させるために重要です。

## オーディエンス名の伝播の動作 {#audience-name-propagation}

ストリーミング宛先に対してオーディエンスをアクティベートすると、最初のアクティベーション中にオーディエンス名が宛先に送信されます。 ただし、オーディエンス名の更新動作は宛先によって異なります。

* **[オーディエンス名の更新をサポートする宛先](#name-update-supported)**: Experience Platformでオーディエンス名を変更すると、更新された名前はこれらの宛先に自動的に反映されます。
* **[オーディエンス名の更新をサポートしていない宛先](#name-update-not-supported)**: Experience Platformでオーディエンス名を変更した場合、最初のアクティベーションの元の名前が引き続き使用されます。

### オーディエンス名の更新をサポートする宛先 {#name-update-supported}

次のストリーミング宛先は、Experience Platformでオーディエンス名を変更する際のオーディエンス名の自動更新をサポートしています。

* [Acxiom Audience Connection](../catalog/advertising/acxiom-audience-connection.md)
* [Adobe Campaign Managed Cloud](../catalog/email-marketing/adobe-campaign-managed-services.md)
* [Adobe Advertising DSP](../catalog/advertising/adobe-advertising-dsp-connection.md)
* [ボンボラ](../catalog/advertising/bombora.md)
* [クリテオ](../catalog/advertising/criteo.md)
* [Demandbase](../catalog/advertising/demandbase.md)
* [Demandbaseの人物](../catalog/advertising/demandbase-people.md)
* [Experience Cloud Audiences](../catalog/adobe/experience-cloud-audiences.md)
* [Facebook カスタムオーディエンス](../catalog/social/facebook.md)
* [Gainsight PX](../catalog/analytics/gainsight-px.md)
* [LINE](../catalog/mobile-engagement/line.md)
* [（企業） LinkedIn Matched Audience](../catalog/social/linkedin-b2b.md)
* [LinkedIn Matched Audience](../catalog/social/linkedin.md)
* [（レガシー） （V2） Marketo Engage](../catalog/adobe/marketo-engage.md)
* [PubMatic Connect](../catalog/advertising/pubmatic.md)
* [SendGrid](../catalog/email-marketing/sendgrid.md)
* [Snap Inc](../catalog/advertising/snap-inc.md)
* [TikTok](../catalog/social/tiktok.md)
* [Twitter カスタムオーディエンス](../catalog/social/twitter.md)
* [Yahoo DataX](../catalog/advertising/datax.md)

### オーディエンス名の更新をサポートしていない宛先 {#name-update-not-supported}

上記に記載されていない宛先の場合、オーディエンス名は最初のアクティベーション後も静的なままになります。 これらの宛先のオーディエンス名を更新する必要がある場合は、次の手順を実行する必要があります。

1. 目的の名前でExperience Platformに新しいオーディエンスを作成します
2. 宛先に対して新しいオーディエンスをアクティブ化する

>[!TIP]
>
>混乱を避けるには、最初のアクティベーションから記述的なオーディエンス名を使用します。特に、オーディエンス名の更新をサポートしていない宛先に対してアクティベートする場合に使用します。

## オーディエンスの削除をサポートする宛先 {#support-removal}

ストリーミング宛先からオーディエンスを削除（マッピング解除）すると、Experience Platformは、対応するオーディエンスを宛先プラットフォームから削除しようとします。 ただし、すべての宛先がこの機能をサポートしているわけではありません。

次のストリーミング宛先は、宛先からオーディエンスをマッピング解除する際のオーディエンスの自動削除をサポートしています。

* [（API）Oracle Eloqua](../catalog/email-marketing/oracle-eloqua-api.md)
* [（企業） LinkedIn Matched Audience](../catalog/social/linkedin-b2b.md)
* [（レガシー） （V2） Marketo Engage](../catalog/adobe/marketo-engage.md)
* [Adobe Advertising DSP](../catalog/advertising/adobe-advertising-dsp-connection.md)
* [Bombora アカウントオーディエンス](../catalog/advertising/bombora.md)
* [クリテオ](../catalog/advertising/criteo.md)
* [Experience Cloud Audiences](../catalog/adobe/experience-cloud-audiences.md)
* [Facebook](../catalog/social/facebook.md)
* [Gainsight PX](../catalog/analytics/gainsight-px.md)
* [HubSpot](../catalog/crm/hubspot.md)
* [LINE](../catalog/mobile-engagement/line.md)
* [LinkedIn Matched Audiences](../catalog/social/linkedin.md)
* [LiveRamp – 配布](../catalog/advertising/liveramp-distribution.md)
* [Mailchimp の興味カテゴリ](../catalog/email-marketing/mailchimp-interest-categories.md)
* [PubMatic Connect](../catalog/advertising/pubmatic.md)
* [Salesforce Marketing Cloudのアカウントエンゲージメント](../catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md)
* [SendGrid](../catalog/email-marketing/sendgrid.md)
* [Snap Inc](../catalog/advertising/snap-inc.md)
* [TikTok](../catalog/social/tiktok.md)
* [Twitter カスタムオーディエンス](../catalog/social/twitter.md)
* [Yahoo DataX](../catalog/advertising/datax.md)

### オーディエンスの削除をサポートしない宛先 {#no-removal-support}

上記に記載されていない宛先の場合、宛先からオーディエンスのマッピングを解除すると、Experience Platformはマッピングのみを削除します。 宛先プラットフォームのオーディエンスは、パートナープラットフォームで手動で削除するまでアクティブのままです。

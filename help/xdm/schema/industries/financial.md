---
solution: Experience Platform
title: 金融サービス業界のデータモデル ERD
description: 銀行業、金融サービス業、保険業 (BFSI) 業界の標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。 このデータモデルは、Adobe Experience Platformで使用する Experience Data Model(XDM) と互換性があります。
exl-id: 2e8f6b2a-10e7-4394-b45f-c03db0f25400
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 11%

---

# [!UICONTROL 金融サービス] 業界データモデル ERD

次のエンティティ関係図 (ERD) は、銀行、金融サービス、保険 (BFSI) 業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの格納方法を考慮し、意図的に非正規化された方法で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨スキーマとその関係を自分で構築する必要があります。 管理に関するガイドを参照してください。 [スキーマ](../../ui/resources/schemas.md) および [関係](../../tutorials/relationship-ui.md) （ UI 内）を参照してください。

この ERD を解釈するには、次の凡例を使用します。

* に表示される各エンティティは、基になる [エクスペリエンスデータモデル (XDM) クラス](../composition.md#class).
* 特定のエンティティに対し、 **太字** は、フィールドグループまたはデータ型を表し、次に示す関連フィールドを太字化されていないテキストで示します。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/financial.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、一意の識別子 (`_id`) 属性は、XDM ExperienceEvent クラスで提供されます。 次のリファレンスドキュメントを参照してください： [XDM ExperienceEvent](../../classes/experienceevent.md) を参照してください。

## [!UICONTROL 金融サービス] 使用例

次の表に、一般的な財務上の使用例で推奨されるクラスとスキーマフィールドグループを示します。

| ユースケース | 推奨クラスとフィールドグループ |
| --- | --- |
| オムニチャネルレポートのインサイトとジャーニーの自動化を通じて、好みのセグメントに合わせてパーソナライゼーションを推進し、優先する報酬プログラムへの登録を増やします。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL 製品カテゴリ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL カードアクション]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL 見積依頼の詳細]](../../field-groups/event/quote-request-details.md)</li><li>[[!UICONTROL 預金の詳細]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li><li>[[!UICONTROL 残高移動]](../../field-groups/event/balance-transfers.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL ロイヤルティの詳細]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| オンラインチャネルとオフラインチャネルにわたって、クロスチャネルのパーソナライゼーションを最適化します。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL 製品カテゴリ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL ロイヤルティの詳細]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| クロスチャネルの行動分析から得たインサイトを使用し、新しい製品オファーにつながる可能性のある製品の使用パターンを識別して、新しい収益機会を促進します。 | <ul><li>**[[!UICONTROL ポリシー]](../../classes/policy.md)**</li><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL 製品カテゴリ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL カードアクション]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL サポートサイト検索]](../../field-groups/event/support-site-search.md)</li><li>[[!UICONTROL 預金の詳細]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL ロイヤルティの詳細]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

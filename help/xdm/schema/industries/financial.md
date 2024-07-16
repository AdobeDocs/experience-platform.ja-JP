---
solution: Experience Platform
title: 金融サービス業界のデータモデル ERD
description: 銀行、金融サービス、および保険（BFSI）業界向けの標準化されたデータ モデルを記述するエンティティ関係図（ERD）を表示します。 このデータモデルは、Adobe Experience Platformで使用するエクスペリエンスデータモデル（XDM）と互換性があります。
exl-id: 2e8f6b2a-10e7-4394-b45f-c03db0f25400
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 2%

---

# [!UICONTROL  金融サービス ] 業界データモデル ERD

次の ERD （エンティティ関係図）は、銀行、金融サービス、保険（BFSI）業界の標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) および [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [ エクスペリエンスデータモデル（XDM）クラス ](../composition.md#class) に基づいています。
* 特定のエンティティについて、**太字** でマークされた各行はフィールドグループまたはデータタイプを表し、その行が提供する関連フィールドが太字なしのテキストで表示されます。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。

![](../../images/industries/financial.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、「_ID」フィールドが含まれます。このフィールドは、XDM ExperienceEvent クラスが提供する一意の識別子（`_id`）属性を表します。 この値に期待される値について詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) に関する参照ドキュメントを参照してください。

## [!UICONTROL  金融サービス ] 使用例

次の表に、いくつかの一般的な財務ユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| オムニチャネルレポートインサイトと自動化されたジャーニーを通じて、好ましいセグメントのパーソナライゼーションを大規模に推進し、好ましい報酬プログラムへの登録を増やします。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL  製品カテゴリ ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL  カードのアクション ]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL  見積依頼の詳細 ]](../../field-groups/event/quote-request-details.md)</li><li>[[!UICONTROL  供託内容等 ]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li><li>[[!UICONTROL  残高移動 ]](../../field-groups/event/balance-transfers.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL  ロイヤルティの詳細 ]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| オンラインチャネルとオフラインチャネルのクロスチャネルパーソナライゼーションを最適化します。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL  製品カテゴリ ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL  ロイヤルティの詳細 ]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| クロスチャネル行動分析から得られたインサイトを使用し、新しい製品オファーにつながる可能性のある製品の使用パターンを特定することで、新しい収益機会を促進します。 | <ul><li>**[[!UICONTROL ポリシー]](../../classes/policy.md)**</li><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL  製品カテゴリ ]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL  カードのアクション ]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL  サポートサイト検索 ]](../../field-groups/event/support-site-search.md)</li><li>[[!UICONTROL  供託内容等 ]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL  ロイヤルティの詳細 ]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

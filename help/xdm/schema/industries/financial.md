---
solution: Experience Platform
title: 金融サービス業界のデータモデル ERD
description: 銀行、金融サービス、および保険（BFSI）業界向けの標準化されたデータ モデルを記述するエンティティ関係図（ERD）を表示します。 このデータモデルは、Adobe Experience Platformで使用するエクスペリエンスデータモデル（XDM）と互換性があります。
exl-id: 2e8f6b2a-10e7-4394-b45f-c03db0f25400
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 2%

---

# [!UICONTROL &#x200B; 金融サービス &#x200B;] 業界データモデル ERD

次の ERD （エンティティ関係図）は、銀行、金融サービス、保険（BFSI）業界の標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Experience Platformでこのデータモデルを利用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) および [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [ エクスペリエンスデータモデル（XDM）クラス ](../composition.md#class) に基づいています。
* 親フィールドの下にインデントされたフィールドは、親のフィールドグループに属する子フィールド（サブフィールド）を表します。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。

![ 金融業界のデータモデルの ERD の例 ](../../images/industries/financial.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、「_ID」フィールドが含まれます。このフィールドは、XDM ExperienceEvent クラスが提供する一意の識別子（`_id`）属性を表します。 この値に期待される値について詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) に関する参照ドキュメントを参照してください。

## [!UICONTROL &#x200B; 金融サービス &#x200B;] 使用例

次の表に、いくつかの一般的な財務ユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| オムニチャネルレポートインサイトと自動化されたジャーニーを通じて、好ましいセグメントのパーソナライゼーションを大規模に推進し、好ましい報酬プログラムへの登録を増やします。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL &#x200B; 製品カテゴリ &#x200B;]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL &#x200B; カードのアクション &#x200B;]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL &#x200B; 見積依頼の詳細 &#x200B;]](../../field-groups/event/quote-request-details.md)</li><li>[[!UICONTROL &#x200B; 供託内容等 &#x200B;]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li><li>[[!UICONTROL &#x200B; 残高移動 &#x200B;]](../../field-groups/event/balance-transfers.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL &#x200B; 人口統計の詳細 &#x200B;]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| オンラインチャネルとオフラインチャネルのクロスチャネルパーソナライゼーションを最適化します。 | <ul><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL &#x200B; 製品カテゴリ &#x200B;]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL &#x200B; 人口統計の詳細 &#x200B;]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| クロスチャネル行動分析から得られたインサイトを使用し、新しい製品オファーにつながる可能性のある製品の使用パターンを特定することで、新しい収益機会を促進します。 | <ul><li>**[[!UICONTROL ポリシー]](../../classes/policy.md)**</li><li>**[[!UICONTROL 製品]](../../classes/product.md)**:<ul><li>[[!UICONTROL &#x200B; 製品カテゴリ &#x200B;]](../../field-groups/product/product-category.md)</li></ul></li><li>**[[!UICONTROL XDM ExperienceEvent]](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL &#x200B; カードのアクション &#x200B;]](../../field-groups/event/card-actions.md)</li><li>[[!UICONTROL &#x200B; サポートサイト検索 &#x200B;]](../../field-groups/event/support-site-search.md)</li><li>[[!UICONTROL &#x200B; 供託内容等 &#x200B;]](../../field-groups/event/deposit-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL &#x200B; 人口統計の詳細 &#x200B;]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL &#x200B; 個人の連絡先の詳細 &#x200B;]](../../field-groups/profile/personal-contact-details.md)</li><li>[[!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;]](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

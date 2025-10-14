---
solution: Experience Platform
title: 小売業界のデータモデル
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル（XDM）と互換性のある、小売業界向けの標準化されたデータモデルを表示します。
exl-id: 40cbb243-668b-4280-815f-1f94a06b6b87
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 小売 &#x200B;] 業界のデータモデル

次の ERD （Entity Relationship Diagram）は、小売業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Experience Platformでこのデータモデルを利用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [&#x200B; スキーマ &#x200B;](../../ui/resources/schemas.md) および [&#x200B; 関係 &#x200B;](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [&#x200B; エクスペリエンスデータモデル（XDM）クラス &#x200B;](../composition.md#class) に基づいています。
* 親フィールドの下にインデントされたフィールドは、親のフィールドグループに属する子フィールド（サブフィールド）を表します。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。

![&#x200B; 小売業界のデータモデルの ERD の例 &#x200B;](../../images/industries/retail.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、「_ID」フィールドが含まれます。このフィールドは、XDM ExperienceEvent クラスが提供する一意の識別子（`_id`）属性を表します。 この値に期待される値について詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) に関する参照ドキュメントを参照してください。

## [!UICONTROL &#x200B; 小売 &#x200B;] ユースケース

次の表に、小売の一般的なユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| オンラインとオフラインのデータソースを組み合わせ、クロスデバイスおよびオンライン/オフライン ID を解決して、クロスチャネルとクロスデバイスのアトリビューションレポートを総合的に提供します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[&#x200B; 製品カテゴリ &#x200B;](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| 様々なセグメントに対してターゲットを絞りパーソナライズされたエクスペリエンスを提供し、売上高を増やし、オムニチャネルオーケストレーションでプラットフォームを強化するのに役立ちます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティング詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャンネル詳細](../../field-groups/event/channel-details.md)</li><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[環境詳細](../../field-groups/event/environment-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[&#x200B; 人口統計の詳細 &#x200B;](../../field-groups/profile/demographic-details.md)</li><li>[&#x200B; 個人の連絡先の詳細 &#x200B;](../../field-groups/profile/personal-contact-details.md)</li><li>[&#x200B; 仕事用連絡先の詳細 &#x200B;](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| マルチタッチアトリビューションを分析して、マーケティング効率を向上させます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティング詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャンネル詳細](../../field-groups/event/channel-details.md)</li><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[&#x200B; 人口統計の詳細 &#x200B;](../../field-groups/profile/demographic-details.md)</li></ul></li></ul> |
| 男性と女性のセグメント化を改善して、メールの関連性を向上させます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[&#x200B; 人口統計の詳細 &#x200B;](../../field-groups/profile/demographic-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[&#x200B; 製品カテゴリ &#x200B;](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| ロイヤルティ（パートナー）データを取り込んで、web、メール、デジタルマーケティングチャネル全体で関連製品情報を増やします。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[&#x200B; 人口統計の詳細 &#x200B;](../../field-groups/profile/demographic-details.md)</li><li>[&#x200B; ロイヤルティの詳細 &#x200B;](../../field-groups/profile/loyalty-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[&#x200B; 製品カテゴリ &#x200B;](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| 自動化され、パーソナライズされたメールを通じて、買い物かごの放棄を再ターゲット化します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[&#x200B; 製品カテゴリ &#x200B;](../../field-groups/product/product-category.md)</li></ul></li></ul> |

{style="table-layout:auto"}

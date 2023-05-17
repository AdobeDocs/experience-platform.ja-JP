---
solution: Experience Platform
title: 小売業界のデータモデル
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル (XDM) と互換性のある、小売業界用の標準化されたデータモデルを表示します。
exl-id: 40cbb243-668b-4280-815f-1f94a06b6b87
source-git-commit: 5ceb261dbf1cac58d0cfe620875b8fa7c761abf2
workflow-type: tm+mt
source-wordcount: '458'
ht-degree: 20%

---

# [!UICONTROL 小売] 業界データモデル

次のエンティティ関係図 (ERD) は、小売業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの格納方法を考慮し、意図的に非正規化された方法で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨スキーマとその関係を自分で構築する必要があります。 管理に関するガイドを参照してください。 [スキーマ](../../ui/resources/schemas.md) および [関係](../../tutorials/relationship-ui.md) （ UI 内）を参照してください。

この ERD を解釈するには、次の凡例を使用します。

* に表示される各エンティティは、基になる [エクスペリエンスデータモデル (XDM) クラス](../composition.md#class).
* 特定のエンティティに対し、 **太字** は、フィールドグループまたはデータ型を表し、次に示す関連フィールドを太字化されていないテキストで示します。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/retail.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、一意の識別子 (`_id`) 属性は、XDM ExperienceEvent クラスで提供されます。 次のリファレンスドキュメントを参照してください： [XDM ExperienceEvent](../../classes/experienceevent.md) を参照してください。

## [!UICONTROL 小売] 使用例

次の表に、一般的な小売の使用例で推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨クラスとフィールドグループ |
| --- | --- |
| オンラインとオフラインのデータソースを組み合わせ、クロスデバイスおよびオンライン/オフラインの ID を解決して、クロスチャネルおよびクロスデバイスの全体的なアトリビューションレポートを提供します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[製品カテゴリ](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| 様々なセグメントに対してターゲットを絞り、パーソナライズされたエクスペリエンスを提供して、チャネルオーケストレーションでのプラットフォームの拡大に役立てます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティング詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャンネル詳細](../../field-groups/event/channel-details.md)</li><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[環境詳細](../../field-groups/event/environment-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[仕事用連絡先の詳細](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| マルチタッチ属性を分析して、マーケティング効率を高めます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[キャンペーンマーケティング詳細](../../field-groups/event/campaign-marketing-details.md)</li><li>[チャンネル詳細](../../field-groups/event/channel-details.md)</li><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li></ul></li></ul> |
| 男性と女性のセグメント化を改善し、メールの関連性を高めます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[製品カテゴリ](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| ロイヤリティ（パートナー）データを取り込み、Web、電子メール、デジタルマーケティングチャネルにわたって関連製品情報を増やします。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[製品カテゴリ](../../field-groups/product/product-category.md)</li></ul></li></ul> |
| 自動およびパーソナライズされた E メールで、買い物かごの放棄者を再ターゲット化します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[コマース詳細](../../field-groups/event/commerce-details.md)</li><li>[Web 詳細](../../field-groups/event/web-details.md)</li></ul></li><li>**[製品](../../classes/product.md)**:<ul><li>[製品カタログ](../../field-groups/product/product-catalog.md)</li><li>[製品カテゴリ](../../field-groups/product/product-category.md)</li></ul></li></ul> |

{style="table-layout:auto"}

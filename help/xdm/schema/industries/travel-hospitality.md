---
solution: Experience Platform
title: 旅行および接客業のデータモデル ERD
description: Adobe Experience Platformで使用する Experience Data Model(XDM) と互換性のある、旅行業界および接客業向けの標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。
exl-id: 4d454160-9066-4702-815b-9509942f709e
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 17%

---

# [!UICONTROL 旅行と接客] 業界データモデル ERD

次のエンティティ関係図 (ERD) は、旅行業界や接客業向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの格納方法を考慮し、意図的に非正規化された方法で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨スキーマとその関係を自分で構築する必要があります。 管理に関するガイドを参照してください。 [スキーマ](../../ui/resources/schemas.md) および [関係](../../tutorials/relationship-ui.md) （ UI 内）を参照してください。

この ERD を解釈するには、次の凡例を使用します。

* に表示される各エンティティは、基になる [エクスペリエンスデータモデル (XDM) クラス](../composition.md#class).
* 特定のエンティティに対し、 **太字** は、フィールドグループまたはデータ型を表し、次に示す関連フィールドを太字化されていないテキストで示します。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/travel-hospitality.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、一意の識別子 (`_id`) 属性は、XDM ExperienceEvent クラスで提供されます。 次のリファレンスドキュメントを参照してください： [XDM ExperienceEvent](../../classes/experienceevent.md) を参照してください。

## [!UICONTROL 旅行と接客] 使用例

次の表に、旅行業界や接客業の一般的な使用例で推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨クラスとフィールドグループ |
| --- | --- |
| クロスセルダイニングやその他の居住者のアトラクションは、近日のホテル予約で市場内のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[仕事用連絡先の詳細](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| アップセルダイニングやその他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[仕事用連絡先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルホテルやその他のレジデントアトラクションは、近日のホテル予約で市場内のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[仕事用連絡先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルフライトやその他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[フライト予約](../../field-groups/event/flight-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[デモグラフィックの詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[仕事用連絡先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

{style="table-layout:auto"}

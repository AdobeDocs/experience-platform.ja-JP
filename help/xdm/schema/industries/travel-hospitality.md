---
solution: Experience Platform
title: 旅行および接客業向けデータモデル ERD
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル (XDM) と互換性のある、旅行業界や接客業向けの標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。
exl-id: 4d454160-9066-4702-815b-9509942f709e
source-git-commit: 295dc040f3af7342226e3d78d0ae21e73db58d57
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 5%

---

# [!UICONTROL 旅行および] 病院業界データモデル ERD

次のエンティティ関係図 (ERD) は、旅行業界や接客業向けの標準化されたデータモデルを表しています。 ERD は、データのAdobe Experience Platformへの格納方法を考慮して、意図的に非正規化方式で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で作成する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) と [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示す各エンティティは、基礎となる [ エクスペリエンスデータモデル (XDM) クラス ](../composition.md#class) に基づいています。
* 特定のエンティティの各行が **bold** でマークされている場合、フィールドグループまたはデータ型を表し、次のフィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントは、多くの場合、トランザクションをおこなった個人や個人を特定できないので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/travel-hospitality.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEvent クラスが提供する一意の識別子 (`_id`) 属性を表す「_ID」フィールドが含まれます。 この値に何が予想されるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) の参照ドキュメントを参照してください。

## [!UICONTROL 旅行と入] 院の使用例

次の表に、旅行業界や接客業でよく使用される使用例の推奨クラスとスキーマフィールドグループを示します。

| 使用例 | 推奨クラスとフィールドグループ |
| --- | --- |
| クロスセルダイニングや他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li><li>[食事の予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| アップセルダイニングやその他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[食事の予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルホテルやその他の住民の観光スポットは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルフライトやその他の居住者の観光スポットは、市場のゲストや宿泊客に向けて、近日のホテル予約です。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[飛行予約](../../field-groups/event/flight-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

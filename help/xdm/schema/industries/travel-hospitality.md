---
solution: Experience Platform
title: 旅行業界と接客業のデータモデルERD
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル(XDM)と互換性のある、旅行業界や接客業向けの標準化されたデータモデルを示すエンティティ関係図(ERD)を表示します。
exl-id: 4d454160-9066-4702-815b-9509942f709e
source-git-commit: 295dc040f3af7342226e3d78d0ae21e73db58d57
workflow-type: tm+mt
source-wordcount: '451'
ht-degree: 5%

---

# [!UICONTROL 旅行業界と] 病院業界のデータモデルERD

次のエンティティ関係図(ERD)は、旅行業界と接客業界向けの標準化されたデータモデルを表しています。 ERDは、Adobe Experience Platformでのデータの格納方法を考慮して、意図的に非正規化方式で提示されます。

>[!NOTE]
>
>説明に従うERDは、この業界の使用例に合わせてデータをモデル化する方法を推奨します。 Platformでこのデータモデルを利用するには、推奨されるスキーマとその関係を自分で作成する必要があります。 詳しくは、UIでの[スキーマ](../../ui/resources/schemas.md)と[関係](../../tutorials/relationship-ui.md)の管理に関するガイドを参照してください。

次の凡例を使用して、このERDを解釈します。

* に表示される各エンティティは、基になる[エクスペリエンスデータモデル(XDM)クラス](../composition.md#class)に基づいています。
* 特定のエンティティに対して、**太字**&#x200B;でマークされた各行は、フィールドグループまたはデータ型を表し、次に示す関連フィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは、「ID」としてマークされ、これらのプロパティの1つが「プライマリID」としてマークされます。
* Cookieベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。

![](../../images/industries/travel-hospitality.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEventクラスで提供される一意の識別子(`_id`)属性を表す「_ID」フィールドが含まれます。 この値に何が求められるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md)の参照ドキュメントを参照してください。

## [!UICONTROL 旅行と入院] の使用例

次の表に、旅行業および接客業の一般的な使用例に関して推奨されるクラスとスキーマフィールドグループを示します。

| 使用例 | 推奨クラスとフィールドグループ |
| --- | --- |
| クロスセルダイニングやその他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| アップセルダイニングやその他の居住者のアトラクションは、近日のホテル予約で市場のゲストやゲストに。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルホテルやその他の滞在アトラクションは、近日のホテル予約で市場のゲストやゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| アップセルフライトやその他の居住者のアトラクションは、市場のゲストや、今後のホテル予約のゲストに提供されます。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約の詳細](../../field-groups/event/reservation-details.md)</li><li>[飛行予約](../../field-groups/event/flight-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[人口統計の詳細](../../field-groups/profile/demographic-details.md)</li><li>[個人の連絡先の詳細](../../field-groups/profile/personal-contact-details.md)</li><li>[勤務先の詳細](../../field-groups/profile/work-contact-details.md)</li><li>[ロイヤルティの詳細](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

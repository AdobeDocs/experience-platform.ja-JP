---
solution: Experience Platform
title: 旅行および接客業データモデル ERD
description: Adobe Experience Platformで使用するためのエクスペリエンスデータモデル（XDM）と互換性のある、旅行および接客業向けの標準化されたデータモデルを表すエンティティ関係図（ERD）を表示します。
exl-id: 4d454160-9066-4702-815b-9509942f709e
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 4%

---

# [!UICONTROL  旅行およびホスピタリティ ] 業界データモデル ERD

次の ERD （Entity Relationship Diagram）は、旅行および接客業向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) および [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [ エクスペリエンスデータモデル（XDM）クラス ](../composition.md#class) に基づいています。
* 特定のエンティティについて、**太字** でマークされた各行はフィールドグループまたはデータタイプを表し、その行が提供する関連フィールドが太字なしのテキストで表示されます。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。

![](../../images/industries/travel-hospitality.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、「_ID」フィールドが含まれます。このフィールドは、XDM ExperienceEvent クラスが提供する一意の識別子（`_id`）属性を表します。 この値に期待される値について詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) に関する参照ドキュメントを参照してください。

## [!UICONTROL  旅行およびホスピタリティ ] ユースケース

次の表に、旅行および接客業の一般的なユースケースをいくつか示し、推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| 今後のホテル予約で、市場内のお客様やゲストにクロスセルのダイニングやその他の居住者のアトラクション。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 人口統計の詳細 ](../../field-groups/profile/demographic-details.md)</li><li>[ 個人の連絡先の詳細 ](../../field-groups/profile/personal-contact-details.md)</li><li>[ 仕事用連絡先の詳細 ](../../field-groups/profile/work-contact-details.md)</li></ul></li></ul> |
| 今後のホテル予約で、インマーケットのお客様やゲストにダイニングやその他の居住者のアトラクションをアップセル。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[食事予約](../../field-groups/event/dining-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 人口統計の詳細 ](../../field-groups/profile/demographic-details.md)</li><li>[ 個人の連絡先の詳細 ](../../field-groups/profile/personal-contact-details.md)</li><li>[ 仕事用連絡先の詳細 ](../../field-groups/profile/work-contact-details.md)</li><li>[ ロイヤルティの詳細 ](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| 今後のホテル予約を持つ市場内のお客様やゲストにアップセルのホテルやその他の居住者のアトラクション。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[宿泊予約](../../field-groups/event/lodging-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 人口統計の詳細 ](../../field-groups/profile/demographic-details.md)</li><li>[ 個人の連絡先の詳細 ](../../field-groups/profile/personal-contact-details.md)</li><li>[ 仕事用連絡先の詳細 ](../../field-groups/profile/work-contact-details.md)</li><li>[ ロイヤルティの詳細 ](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |
| 今後のホテル予約を持つ市場内のお客様やゲストにアップセルのフライトやその他の居住者のアトラクション。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予約詳細](../../field-groups/event/reservation-details.md)</li><li>[フライト予約](../../field-groups/event/flight-reservation.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 人口統計の詳細 ](../../field-groups/profile/demographic-details.md)</li><li>[ 個人の連絡先の詳細 ](../../field-groups/profile/personal-contact-details.md)</li><li>[ 仕事用連絡先の詳細 ](../../field-groups/profile/work-contact-details.md)</li><li>[ ロイヤルティの詳細 ](../../field-groups/profile/loyalty-details.md)</li></ul></li></ul> |

{style="table-layout:auto"}

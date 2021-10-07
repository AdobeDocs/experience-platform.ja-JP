---
solution: Experience Platform
title: 通信業界のデータモデル ERD
topic-legacy: overview
description: Adobe Experience Platformで使用するエクスペリエンスデータモデル (XDM) と互換性のある、通信業界用の標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。
exl-id: 96f267ce-a177-4384-a512-841c89d942ba
source-git-commit: 421b4a448370f9903b8bc826fd9be9e5b2e11c59
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 4%

---

#  通信業界データモデル ERD

次のエンティティ関係図 (ERD) は、通信業界向けの標準化されたデータモデルを表しています。 ERD は、データのAdobe Experience Platformへの格納方法を考慮して、意図的に非正規化方式で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨されるスキーマとその関係を自分で作成する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) と [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示す各エンティティは、基礎となる [ エクスペリエンスデータモデル (XDM) クラス ](../composition.md#class) に基づいています。
* 特定のエンティティの各行が **bold** でマークされている場合、フィールドグループまたはデータ型を表し、次のフィールドが太字で示されています。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントは、多くの場合、トランザクションをおこなった個人や個人を特定できないので、エンティティの関係は非依存としてマークされます。


![](../../images/industries/telecom.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、XDM ExperienceEvent クラスが提供する一意の識別子 (`_id`) 属性を表す「_ID」フィールドが含まれます。 この値に何が予想されるかについて詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) の参照ドキュメントを参照してください。

##  通信事例

次の表に、通信業界の一般的な使用例の推奨クラスとスキーマフィールドグループを示します。

| 使用例 | 推奨クラスとフィールドグループ |
| --- | --- |
| 現在の保有状況と閲覧行動に基づいて、アップセルやクロスセルの機会に適した顧客を把握します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL アップセルの詳細]](../../field-groups/event/upsell-details.md)</li><li>[[!UICONTROL アップグレードの詳細]](../../field-groups/event/upgrade-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL 通信購読]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL 人口統計の詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 関連する広告と自動パーソナライズされた E メールを通じて、買い物かごの放棄者を再ターゲット化します。 コンバージョン時に広告を抑制します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL コマースの詳細]](../../field-groups/event/upsell-details.md) （買い物かごの放棄をキャプチャするため）</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL 通信購読]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL 人口統計の詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 顧客が（従業員のインタラクションまたは自動機械学習アルゴリズムに基づいて）チャーンする可能性が高いとマークされた場合、顧客の詳細をデジタルチャネルと非デジタルチャネルに送信します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL キャンペーンマーケティングの詳細]](../../field-groups/event/campaign-marketing-details.md)</li><li>[[!UICONTROL チャネルの詳細]](../../field-groups/event/channel-details.md)</li><li>パーソナライズされたコンテンツを含むカスタムフィールドグループ</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL 人口統計の詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

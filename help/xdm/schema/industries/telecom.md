---
solution: Experience Platform
title: 通信業界のデータモデル ERD
description: 通信業界向けの標準化されたデータモデルを記述し、Adobe Experience Platformで使用するエクスペリエンスデータモデル（XDM）と互換性のある ERD （Entity Relationship Diagram）を表示します。
exl-id: 96f267ce-a177-4384-a512-841c89d942ba
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 1%

---

# [!UICONTROL  通信業 ] 業界データモデル ERD

次の ERD （Entity Relationship Diagram）は、通信業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの保存方法を考慮して、正規化されていない方法で意図的に表示されます。

>[!NOTE]
>
>説明している ERD は、この業界のユースケースに合わせてデータをモデル化する方法の推奨事項です。 Experience Platformでこのデータモデルを利用するには、推奨されるスキーマとその関係を自分で構築する必要があります。 詳しくは、UI での [ スキーマ ](../../ui/resources/schemas.md) および [ 関係 ](../../tutorials/relationship-ui.md) の管理に関するガイドを参照してください。

次の凡例を使用して、この ERD を解釈します。

* に示されている各エンティティは、基になる [ エクスペリエンスデータモデル（XDM）クラス ](../composition.md#class) に基づいています。
* 親フィールドの下にインデントされたフィールドは、親のフィールドグループに属する子フィールド（サブフィールド）を表します。
* 特定のエンティティで最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つは「プライマリ ID」としてマークされます。
* エンティティ関係は、cookie ベースのイベントでは多くの場合、トランザクションを行った個人を特定できないので、非依存としてマークされます。


![ 通信業界のデータモデルの ERD の例 ](../../images/industries/telecom.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、「_ID」フィールドが含まれます。このフィールドは、XDM ExperienceEvent クラスが提供する一意の識別子（`_id`）属性を表します。 この値に期待される値について詳しくは、[XDM ExperienceEvent](../../classes/experienceevent.md) に関する参照ドキュメントを参照してください。

## [!UICONTROL  電気通信 ] 使用例

次の表に、通信業界の一般的なユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨されるクラスおよびフィールドグループ |
| --- | --- |
| 現在の保有量とブラウジング行動に基づいて、アップセルまたはクロスセルの機会に適した候補である顧客を理解します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL  アップセルの詳細 ]](../../field-groups/event/upsell-details.md)</li><li>[[!UICONTROL  アップグレードの詳細 ]](../../field-groups/event/upgrade-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  電気通信の登録 ]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 関連する広告とパーソナライズされた自動メールを使用して、買い物かごの放棄を再ターゲット化します。 変換時に広告を抑制 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL Commerceの詳細 ]](../../field-groups/event/upsell-details.md) （買い物かごの放棄をキャプチャするため）</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  電気通信の登録 ]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 従業員のインタラクションや自動機械学習アルゴリズムに基づいて、チャーンを起こす可能性があると顧客がマークされた場合、顧客の詳細をデジタルチャネルと非デジタルチャネルに送信します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL キャンペーンマーケティング詳細]](../../field-groups/event/campaign-marketing-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li><li>パーソナライズされたコンテンツを含むカスタムフィールドグループ</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL  人口統計の詳細 ]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL  個人の連絡先の詳細 ]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |

{style="table-layout:auto"}

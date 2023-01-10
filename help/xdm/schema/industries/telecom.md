---
solution: Experience Platform
title: 通信業界のデータモデル ERD
description: Adobe Experience Platformで使用する Experience Data Model(XDM) と互換性のある、通信業界向けの標準化されたデータモデルを示すエンティティ関係図 (ERD) を表示します。
exl-id: 96f267ce-a177-4384-a512-841c89d942ba
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 11%

---

# [!UICONTROL 通信業] 業界データモデル ERD

次のエンティティ関係図 (ERD) は、通信業界向けの標準化されたデータモデルを表しています。 ERD は、Adobe Experience Platformでのデータの格納方法を考慮し、意図的に非正規化された方法で提示されています。

>[!NOTE]
>
>説明に従った ERD は、この業界の使用例でデータをモデル化する方法を推奨します。 Platform でこのデータモデルを使用するには、推奨スキーマとその関係を自分で構築する必要があります。 管理に関するガイドを参照してください。 [スキーマ](../../ui/resources/schemas.md) および [関係](../../tutorials/relationship-ui.md) （ UI 内）を参照してください。

この ERD を解釈するには、次の凡例を使用します。

* に表示される各エンティティは、基になる [エクスペリエンスデータモデル (XDM) クラス](../composition.md#class).
* 特定のエンティティに対し、 **太字** は、フィールドグループまたはデータ型を表し、次に示す関連フィールドを太字化されていないテキストで示します。
* 特定のエンティティの最も重要なフィールドは、赤でハイライト表示されます。
* 個々の顧客の識別に使用できるすべてのプロパティは「ID」としてマークされ、これらのプロパティの 1 つが「プライマリ ID」としてマークされます。
* Cookie ベースのイベントはトランザクションをおこなった個人や個人を特定できないことが多いので、エンティティの関係は非依存としてマークされます。


![](../../images/industries/telecom.png)

>[!NOTE]
>
>エクスペリエンスイベントエンティティには、一意の識別子 (`_id`) 属性は、XDM ExperienceEvent クラスで提供されます。 次のリファレンスドキュメントを参照してください： [XDM ExperienceEvent](../../classes/experienceevent.md) を参照してください。

## [!UICONTROL 通信業] 使用例

次の表に、通信業界の一般的な使用例で推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | 推奨クラスとフィールドグループ |
| --- | --- |
| 現在の保有状況と閲覧行動に基づいて、アップセルまたはクロスセルの機会に適した顧客を把握します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL アップセルの詳細]](../../field-groups/event/upsell-details.md)</li><li>[[!UICONTROL アップグレードの詳細]](../../field-groups/event/upgrade-details.md)</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL 通信サブスクリプション]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 関連する広告と自動化されたパーソナライズされた E メールを通じて、買い物かごの放棄者をリターゲティングします。 コンバージョン時に広告を抑制します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL コマースの詳細]](../../field-groups/event/upsell-details.md) （買い物かごの放棄をキャプチャするため）</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL 通信サブスクリプション]](../../field-groups/profile/telecom-subscription.md)</li><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |
| 顧客が（従業員のインタラクションまたは自動機械学習アルゴリズムに基づいて）チャーンする可能性が高いとマークされた場合、顧客の詳細をデジタルチャネルと非デジタルチャネルに送信します。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[[!UICONTROL キャンペーンマーケティング詳細]](../../field-groups/event/campaign-marketing-details.md)</li><li>[[!UICONTROL チャンネル詳細]](../../field-groups/event/channel-details.md)</li><li>パーソナライズされたコンテンツを含むカスタムフィールドグループ</li></ul></li><li>**[[!UICONTROL XDM 個人プロファイル]](../../classes/individual-profile.md)**:<ul><li>[[!UICONTROL デモグラフィックの詳細]](../../field-groups/profile/demographic-details.md)</li><li>[[!UICONTROL 個人の連絡先の詳細]](../../field-groups/profile/personal-contact-details.md)</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

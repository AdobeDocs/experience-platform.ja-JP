---
title: ヘルスケアデータモデル V2
description: ヘルスケアの一般的なユースケースと、使用すべきベストクラス、関連するフィールドグループ、データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: a796b58b-b36f-4277-870b-0d3939af8061
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 3%

---

# [!UICONTROL Healthcare] データモデル V2

## フィールドグループとクラス {#field-groups}

次の表に、ヘルスケアの一般的な使用例に推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | フィールドグループと互換性のあるクラス |
| --- | --- |
| **患者の作成/更新**：患者が病院のフロント デスクに到着すると、ID （オプション）、患者名、生年月日、性別、住所などのデモグラフィック情報を含む患者の記録が作成されます。 これは、ヘルスケア ITの重要な要素として機能します。 | <ul><li>**[XDM個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[患者](./field-groups/patient.md)</li></ul></li></ul> |
| **ワクチン接種**：ワクチン接種プロセスの促進、患者の予防接種記録の管理、EMRとワクチン管理システムの統合。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[予防接種](./field-groups/immunization.md)</li></ul></li><li>**[XDM個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li><li>[患者](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[薬](../../classes/medication.md)**:<ul><li>[薬](./field-groups/medication.md)</li><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li></ul></li><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li></ul></li></ul> |
| **ポストケアアドヒアランス**：患者と介護者に治療計画を完了させ、送金率を下げるよう動機付ける。 | <ul><li>**[XDM個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[&#x200B; ケアプラン &#x200B;](./field-groups/care-plan.md)</li><li>[目標](./field-groups/goal.md)</li><li>[患者](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[目標](./field-groups/goal.md)</li></ul></li></ul> |
| **保険の消費者体験**：保険を購入する消費者のデジタル獲得と体験を向上させます。 以下に例を示します。 <li> 一般的な情報（プラン、プラン名、利用階層、メディケイド、ウェルネスプログラムなど）を含むページにアクセスする人々に対して、プロモーションメールやターゲットを絞ったサードパーティ広告を送信する消費者行動を把握したい</li><li> 心臓病に関するワクチン関連情報を送信してブランド認知度を高めたり、心臓病やワクチン情報を探している人々にワクチン接種のスケジュールを設定したりします。 </li> | <ul><li>**[XDM個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[アカウント](./field-groups/account.md)</li><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li><li>[患者](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[薬](../../classes/medication.md)**:<ul><li>[薬](./field-groups/medication.md)</li><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li></ul></li><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[アカウント](./field-groups/account.md)</li><li>[調剤](./field-groups/medication-dispense.md)</li><li>[薬の使用申請](./field-groups/medication-request.md)</li></ul><li>**[プラン](../../classes/plan.md)**:<ul><li>[目標](./field-groups/coverage.md)</li></ul></li></ul> |
| **強化されたプロバイダー体験**: EMR システムのプロバイダーのデータを使用して、予約の空き状況、場所、専門性に基づいて代替プロバイダーを提案します。<br> <br> プロバイダー検索を改善して、必要な可用性を持つ結果を表示し、選択したプロバイダーが支払い者ネットワークの一部であることを確認し、コスト見積もりを提供します。 | <ul><li>**[XDM個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[予定](./field-groups/appointment.md)</li><li>[組織](./field-groups/organization.md)</li><li>[患者](./field-groups/patient.md)</li><li>[実務担当者](./field-groups/practioner.md)</li><li>[スケジュール](./field-groups/schedule.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[予定](./field-groups/appointment.md)</li><li>[組織](./field-groups/organization.md)</li><li>[実務担当者](./field-groups/practioner.md)</li><li>[スケジュール](./field-groups/schedule.md)</li></ul></li></ul> |

{style="table-layout:fixed"}

## データタイプ {#data-types}

次の表は、[!DNL HL7 FHIR Release 5]の仕様に従って作成されたデータタイプの概要を示しています。

| 名前 | 説明 |
| --- | --- |
| [[!UICONTROL Address]](./data-types/address.md) | （GPSやその他の位置定義形式とは異なり）郵便規則を使用して表されるアドレスを記述します。 |
| [[!UICONTROL Annotation]](./data-types/annotation.md) | 作成者に対する属性を含むテキストノード。 |
| [[!UICONTROL Availability]](./data-types/availability.md) | 品目の可用性データ。 |
| [[!UICONTROL Codeable Concept]](./data-types/codeable-concept.md) | あるリソースから別のリソースへの参照。 |
| [[!UICONTROL Codeable Reference]](./data-types/codeable-reference.md) | リソースやコンセプトへの参照。 |
| [[!UICONTROL Coding]](./data-types/coding.md) | 用語システムによって定義されたコードへの参照。 |
| [[!UICONTROL Contact Point]](./data-types/contact-point.md) | 個人の連絡先情報。 |
| [[!UICONTROL Dosage]](./data-types/dosage.md) | 薬がどのように服用されているか、または服用すべきか。 |
| [[!UICONTROL Duration]](./data-types/duration.md) | 長い時間です。 |
| [[!UICONTROL Extended Contact Details]](./data-types/extended-contact-detail.md) | 拡張連絡先の情報。 |
| [[!UICONTROL Human Name]](./data-types/human-name.md) | 人間またはその他の生命体の名前に関する情報。 |
| [[!UICONTROL Identifier]](./data-types/identifier.md) | 計算を目的とした識別子。 |
| [[!UICONTROL Money]](./data-types/money.md) | ある認識された通貨における経済的有用性の量。 |
| [[!UICONTROL Period]](./data-types/period.md) | 開始日時と終了日時で定義される期間。 |
| [[!UICONTROL Person]](./data-types/person.md) | 汎用的な人物レコードに関する情報。 |
| [[!UICONTROL Quantity]](./data-types/quantity.md) | 測定可能な額です。 |
| [[!UICONTROL Range]](./data-types/range.md) | 低い値と高い値によってバインドされた値のセット。 |
| [[!UICONTROL Ratio]](./data-types/ratio.md) | 分子と分母による2つの[[!UICONTROL Quantity]](./data-types/quantity.md)値の比率。 |
| [[!UICONTROL Reference]](./data-types/reference.md) | あるリソースから別のリソースへの参照。 |
| [[!UICONTROL Repeat]](./data-types/repeat.md) | イベントがいつスケジュールされるかを記述する一連のルール。 |
| [[!UICONTROL Simple Quantity]](./data-types/simple-quantity.md) | 測定可能な額です。 |
| [[!UICONTROL Timing]](./data-types/timing.md) | 複数回発生する可能性のあるイベントに関する情報。 |
| [[!UICONTROL Virtual Service Detail]](./data-types/virtual-service-detail.md) | 仮想サービスの連絡先の詳細： |


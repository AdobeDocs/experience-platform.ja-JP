---
title: ヘルスケアデータモデル V2
description: ヘルスケアの一般的なユースケースと、使用する最適なクラス、関連するフィールドグループ、データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: a796b58b-b36f-4277-870b-0d3939af8061
source-git-commit: 8eaff2361e76a7856b3371156ed9fe5c542fec28
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 4%

---

# [!UICONTROL  ヘルスケア ] データモデル V2

## フィールドグループとクラス {#field-groups}

次の表に、いくつかの一般的なヘルスケアユースケースで推奨されるクラスとスキーマフィールドグループの概要を示します。

| ユースケース | フィールドグループと互換性のあるクラス |
| --- | --- |
| **患者の作成/更新**：患者が病院のフロントデスクに到着すると、識別子（オプション）、患者の名前、生年月日、性別、住所などの人口統計の詳細を含む、患者レコードが作成されます。 これは、医療 IT の重要な要素として機能します。 | <ul><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 患者 ](./field-groups/patient.md)</li></ul></li></ul> |
| **ワクチン接種**：ワクチン接種プロセスの促進、患者の予防接種記録の管理、および EMR とワクチン管理システムの統合。 | <ul><li>**[XDM ExperienceEvent](../../classes/experienceevent.md)**:<ul><li>[ 免疫 ](./field-groups/immunization.md)</li></ul></li><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li><li>[ 患者 ](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[医薬品](../../classes/medication.md)**:<ul><li>[ 医薬品 ](./field-groups/medication.md)</li><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li></ul></li><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li></ul></li></ul> |
| **ポストケアのアドヒアランス**：患者および介護者に治療計画の完了および送金率の低減を促す。 | <ul><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 介護計画 ](./field-groups/care-plan.md)</li><li>[ 目標 ](./field-groups/goal.md)</li><li>[ 患者 ](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[ 目標 ](./field-groups/goal.md)</li></ul></li></ul> |
| **保険における消費者エクスペリエンス**：保険を購入する消費者間で、デジタル獲得とエクスペリエンスを向上させます。 以下に例を示します。 <li> 一般的な情報（プラン、プラン名/層、メディケイド、ウェルネスプログラムなど）を含んだページにアクセスする人にプロモーションメールやターゲット設定されたサードパーティ広告を送信する消費者行動について</li><li> 心臓の健康に関するワクチン関連情報を送信して、ブランドの認知度を高めたり、心臓の健康とワクチンに関する情報を探している人にワクチンのスケジュールを設定したりするリクエストを送信する。 </li> | <ul><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[アカウント](./field-groups/account.md)</li><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li><li>[ 患者 ](./field-groups/patient.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[医薬品](../../classes/medication.md)**:<ul><li>[ 医薬品 ](./field-groups/medication.md)</li><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li></ul></li><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[アカウント](./field-groups/account.md)</li><li>[ 調剤 ](./field-groups/medication-dispense.md)</li><li>[ 投薬請求 ](./field-groups/medication-request.md)</li></ul><li>**[計画](../../classes/plan.md)**:<ul><li>[ 目標 ](./field-groups/coverage.md)</li></ul></li></ul> |
| **プロバイダーエクスペリエンスの強化**:EMR システムのプロバイダーデータを使用して、予約の空き時間、場所、専門に基づいて代替プロバイダーを提案します。<br> <br> プロバイダー検索を改善して必要な可用性で結果を表示し、選択したプロバイダーが支払い者ネットワークの一部であることを確認し、コストの見積もりを提供する。 | <ul><li>**[XDM 個人プロファイル](../../classes/individual-profile.md)**:<ul><li>[ 組織 ](./field-groups/organization.md)</li><li>[ 患者 ](./field-groups/patient.md)</li><li>[ 実務担当者 ](./field-groups/practioner.md)</li><li>[スケジュール](./field-groups/schedule.md)</li></ul></li><li>**[場所](./classes/location.md)**:<ul><li>[場所](./field-groups/location.md)</li></ul><li>**[プロバイダー](../../classes/provider.md)**:<ul><li>[ 組織 ](./field-groups/organization.md)</li><li>[ 実務担当者 ](./field-groups/practioner.md)</li><li>[スケジュール](./field-groups/schedule.md)</li></ul></li></ul> |

## データタイプ {#data-types}

次の表に、[!DNL HL7 FHIR Release 5] の仕様に従って作成されるデータタイプの概要を示します。

| 名前 | 説明 |
| --- | --- |
| [[!UICONTROL アドレス]](./data-types/address.md) | （GPS やその他の場所定義形式とは異なり）郵便規則を使用して表されたアドレスを表します。 |
| [[!UICONTROL  注釈 ]](./data-types/annotation.md) | オーサーに対するアトリビューションを持つテキストノード。 |
| [[!UICONTROL 対象]](./data-types/availability.md) | アイテムの空き時間データ。 |
| [[!UICONTROL  コード化可能な概念 ]](./data-types/codeable-concept.md) | あるリソースから別のリソースへの参照。 |
| [[!UICONTROL  コード化可能な参照 ]](./data-types/codeable-reference.md) | リソースまたは概念への参照。 |
| [[!UICONTROL  コーディング ]](./data-types/coding.md) | 用語システムで定義されているコードへの参照。 |
| [[!UICONTROL  連絡窓口 ]](./data-types/contact-point.md) | 個人の連絡先詳細。 |
| [[!UICONTROL  用量 ]](./data-types/dosage.md) | 薬の服用方法または服用方法。 |
| [[!UICONTROL 期間]](./data-types/duration.md) | 時間の長さ。 |
| [[!UICONTROL  詳細な連絡先 ]](./data-types/extended-contact-detail.md) | 拡張連絡先の情報。 |
| [[!UICONTROL  人間の名前 ]](./data-types/human-name.md) | 人間またはその他の生命体の名前に関する情報。 |
| [[!UICONTROL 識別子]](./data-types/identifier.md) | 計算を目的とした識別子。 |
| [[!UICONTROL  金額 ]](./data-types/money.md) | ある認識された通貨での経済的有用性の量。 |
| [[!UICONTROL  期間 ]](./data-types/period.md) | 開始日時と終了日時で定義される期間。 |
| [[!UICONTROL  人物 ]](./data-types/person.md) | 汎用人物レコードに関する情報。 |
| [[!UICONTROL  数量 ]](./data-types/quantity.md) | 測定または測定可能な量。 |
| [[!UICONTROL 範囲]](./data-types/range.md) | 下限と上限の値によってバインドされた値のセット。 |
| [[!UICONTROL  比率 ]](./data-types/ratio.md) | 分子と分母を介した 2 つの [[!UICONTROL  数量 ]](./data-types/quantity.md) 値の比率。 |
| [[!UICONTROL  参考 ]](./data-types/reference.md) | あるリソースから別のリソースへの参照。 |
| [[!UICONTROL  繰り返し ]](./data-types/repeat.md) | イベントがスケジュールされるタイミングを記述する一連のルール。 |
| [[!UICONTROL  単純量 ]](./data-types/simple-quantity.md) | 測定または測定可能な量。 |
| [[!UICONTROL  タイミング ]](./data-types/timing.md) | 複数回発生する可能性のあるイベントに関する情報。 |
| [[!UICONTROL  仮想サービスの詳細 ]](./data-types/virtual-service-detail.md) | 仮想サービスの連絡先の詳細。 |

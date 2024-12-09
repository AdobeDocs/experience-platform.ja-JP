---
title: スキーマフィールドグループをスケジュール
description: スケジュール スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: e01a6cd4cf42338f87222a5e2871cd6c3683309a
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 5%

---

# [!UICONTROL  スケジュール ] スキーマフィールドグループ

[!UICONTROL  スケジュール ] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) および [[!DNL Provider class]](../../classes/provider.md) の標準スキーマフィールドグループです。 これは、予定を予約するのに利用できる時間枠のコンテナである、単一のオブジェクトタイプのフィールド `healthcareSchedule` を提供します。

![ フィールドグループ構造 ](../../images/field-groups/schedule.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL  アクター ] | `actor` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | このスケジュールを参照するスロットで、参照先のリソースの可用性の詳細が提供されます。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../../data-types/healthcare/identifier.md) の配列 | スケジュールの外部識別子。 |
| [!UICONTROL  計画期間 ] | `planningHorizon` | [[!UICONTROL  期間 ]](../../data-types/healthcare/period.md) | このスケジュールを参照するスロットが存在しない場合でも、カバーする期間。 |
| [!UICONTROL  サービス区分 ] | `serviceCategory` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) の配列 | 任用中に実行されるサービスの幅広いカテゴリ。 |
| [!UICONTROL  サービスタイプ ] | `serviceType` | [[!UICONTROL  コード化可能な参照 ]](../../data-types/healthcare/codeable-reference.md) の配列 | 予定中に実行される特定のサービス。 |
| [!UICONTROL  専門分野 ] | `specialty` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) の配列 | 予約で要求されたサービスを実行するために必要とされる実務担当者の専門分野。 |
| [!UICONTROL  アクティブ ] | `active` | ブール値 | スケジュール レコードが有効に使用されているかどうかを示します。 |
| [!UICONTROL  コメント ] | `comment` | 文字列 | スロットのカスタム制約など、拡張情報を記述する目的での可用性に関するコメント。 |
| [!UICONTROL 名前] | `name` | 文字列 | 検索中に消費者に表示されるスケジュールの説明。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/schedule.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/schedule.schema.json)

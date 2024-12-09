---
title: 組織スキーマフィールドグループ
description: 組織スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 7%

---

# [!UICONTROL  組織 ] スキーマフィールドグループ

[!UICONTROL  組織 ] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) および [[!DNL Provider class]](../../classes/provider.md) の標準スキーマフィールドグループです。 共通の目的を持つ人物または組織のグループに関す `healthcareOrganization` 情報を含む、単一のオブジェクトタイプのフィールドを提供します。

![ フィールドグループ構造 ](../../images/field-groups/healthcare-organization/organization.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| ---| --- | --- | --- |
| [!UICONTROL  連絡先詳細 ] | `contact` | の配列 [[!UICONTROL  拡張連絡先の詳細 ]](../../data-types/healthcare/extended-contact-detail.md) | 特定の組織で使用可能な通信デバイスの連絡先詳細。 これには、住所、電話番号、FAX 番号、携帯電話番号、メールアドレス、web サイトが含まれます。 |
| [!UICONTROL  終点 ] | `endpoint` | [[!UICONTROL  参照 ]](../../data-types/healthcare/reference.md) の配列 | 組織で動作するサービスへのアクセスを提供するテクニカルエンドポイント。 |
| [!UICONTROL 識別子] | `indentifier` | [[!UICONTROL  識別子 ]](../../data-types/healthcare/identifier.md) の配列 | 複数の異なるシステムをまたいで組織を識別するために使用される識別子。 |
| [!UICONTROL  組織の一部 ] | `partOf` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | この組織が属する組織。 |
| [!UICONTROL  資格 ] | `qualification` | オブジェクトの配列 | 組織によるケアの提供を承認または承認する、公式の認定、認定、トレーニング、指定、およびライセンス。 詳しくは、[ 以下の節 ](#qualification) を参照してください。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) の配列 | 組織の種類。 |
| [!UICONTROL  アクティブ ] | `active` | ブール値 | 組織のレコードがまだ有効に使用されているかどうか。 |
| [!UICONTROL  エイリアス ] | `alias` | 文字列の配列 | 組織の別名（旧称）のリスト。 |
| [!UICONTROL 説明] | `description` | 文字列 | 組織の説明。適切な組織が選択されていることを確認するために、一般的なコンテキストを提供するのに役立ちます。 |
| [!UICONTROL 名前] | `name` | 文字列 | 組織に関連付けられた名前。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.schema.json)

## `qualification` {#qualification}

`qualification` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![ 認定構造 ](../../images/field-groups/healthcare-organization/qualification.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | [[!UICONTROL  コード化可能な概念 ]](../../data-types/healthcare/codeable-concept.md) | 選定のコード化表現。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../../data-types/healthcare/identifier.md) の配列 | この組織のこの選定に割り当てられた識別子。 |
| [!UICONTROL  発行者 ] | `issuer` | [[!UICONTROL  参考 ]](../../data-types/healthcare/reference.md) | 資格を規制および発行する組織。 |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../../data-types/healthcare/period.md) | 資格が有効である期間。 |

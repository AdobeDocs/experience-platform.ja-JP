---
title: 組織スキーマフィールドグループ
description: 組織スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: b0698d36-ebc3-4b76-adcc-1deb2cbb1564
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 組織 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 組織 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../../classes/individual-profile.md) および [[!DNL Provider class]](../../../classes/provider.md) の標準スキーマフィールドグループです。 共通の目的を持つ人物または組織のグループに関す `healthcareOrganization` 情報を含む、単一のオブジェクトタイプのフィールドを提供します。

![&#x200B; フィールドグループ構造 &#x200B;](../../../images/healthcare/field-groups/organization/organization.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| ---| --- | --- | --- |
| [!UICONTROL &#x200B; 連絡先詳細 &#x200B;] | `contact` | の配列 [[!UICONTROL &#x200B; 拡張連絡先の詳細 &#x200B;]](../data-types/extended-contact-detail.md) | 特定の組織で使用可能な通信デバイスの連絡先詳細。 これには、住所、電話番号、FAX 番号、携帯電話番号、メールアドレス、web サイトが含まれます。 |
| [!UICONTROL &#x200B; 終点 &#x200B;] | `endpoint` | [[!UICONTROL &#x200B; 参照 &#x200B;]](../data-types/reference.md) の配列 | 組織で動作するサービスへのアクセスを提供するテクニカルエンドポイント。 |
| [!UICONTROL 識別子] | `indentifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | 複数の異なるシステムをまたいで組織を識別するために使用される識別子。 |
| [!UICONTROL &#x200B; 組織の一部 &#x200B;] | `partOf` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | この組織が属する組織。 |
| [!UICONTROL &#x200B; 資格 &#x200B;] | `qualification` | オブジェクトの配列 | 組織によるケアの提供を承認または承認する、公式の認定、認定、トレーニング、指定、およびライセンス。 詳しくは、[&#x200B; 以下の節 &#x200B;](#qualification) を参照してください。 |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) の配列 | 組織の種類。 |
| [!UICONTROL &#x200B; アクティブ &#x200B;] | `active` | ブール値 | 組織のレコードがまだ有効に使用されているかどうか。 |
| [!UICONTROL &#x200B; エイリアス &#x200B;] | `alias` | 文字列の配列 | 組織の別名（旧称）のリスト。 |
| [!UICONTROL 説明] | `description` | 文字列 | 組織の説明。適切な組織が選択されていることを確認するために、一般的なコンテキストを提供するのに役立ちます。 |
| [!UICONTROL 名前] | `name` | 文字列 | 組織に関連付けられた名前。 |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/coverage.schema.json)

## `qualification` {#qualification}

`qualification` はオブジェクトの配列として指定されます。 各オブジェクトの構造については、以下で説明します。

![&#x200B; 認定構造 &#x200B;](../../../images/healthcare/field-groups/organization/qualification.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL コード] | `code` | [[!UICONTROL &#x200B; コード化可能な概念 &#x200B;]](../data-types/codeable-concept.md) | 選定のコード化表現。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL &#x200B; 識別子 &#x200B;]](../data-types/identifier.md) の配列 | この組織のこの選定に割り当てられた識別子。 |
| [!UICONTROL &#x200B; 発行者 &#x200B;] | `issuer` | [[!UICONTROL &#x200B; 参考 &#x200B;]](../data-types/reference.md) | 資格を規制および発行する組織。 |
| [!UICONTROL &#x200B; 期間 &#x200B;] | `period` | [[!UICONTROL &#x200B; 期間 &#x200B;]](../data-types/period.md) | 資格が有効である期間。 |

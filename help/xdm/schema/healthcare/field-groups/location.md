---
title: 場所スキーマフィールドグループ
description: 場所スキーマフィールドグループについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 99831093-89da-4329-be29-c130c1d48f63
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 6%

---

# [!UICONTROL  場所 ] スキーマフィールドグループ

[!UICONTROL Location] は、[[!DNL Location]  クラス ](../classes/location.md) の標準スキーマフィールドグループです。 場所の詳細と位置情報を取得する単一のオブジェクトタイプフィールド `healthcareLocation` を提供します。

![ フィールドグループ構造 ](../../../images/healthcare/field-groups/location.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL アドレス]](../data-types/address.md) | 物理的な場所のアドレス。 |
| [!UICONTROL  特性 ] | `characteristic` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) の配列 | 場所の特性のコレクション。 |
| [!UICONTROL  連絡先 ] | `contact` | の配列 [[!UICONTROL  拡張連絡先の詳細 ]](../data-types/extended-contact-detail.md) | 場所の連絡先詳細。 |
| [!UICONTROL  エンドポイント ] | `endpoint` | [[!UICONTROL  参照 ]](../data-types/reference.md) の配列 | その場所の運用サービスへのアクセスを提供するテクニカルエンドポイント。 |
| [!UICONTROL フォーム] | `form` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 場所の物理的な形式。 |
| [!UICONTROL  運用時間 ] | `hoursOfOperation` | 配列 [[!UICONTROL  可用性 ]](../data-types/availability.md) | この場所の通常の営業時間（例外を含む）。 |
| [!UICONTROL 識別子] | `identifier` | [[!UICONTROL  識別子 ]](../data-types/identifier.md) の配列 | 場所を識別する一意のコードまたは番号。 |
| [!UICONTROL  組織の管理 ] | `managingOrganization` | [[!UICONTROL  参考 ]](../data-types/reference.md) | プロビジョニングと維持管理を担当する組織。 |
| [!UICONTROL  運用状況 ] | `operationalStatus` | [[!UICONTROL  コーディング ]](../data-types/coding.md) | 場所の操作ステータス。 |
| [!UICONTROL  場所の一部 ] | `partOf` | [[!UICONTROL  参考 ]](../data-types/reference.md) | この場所が属する場所。 |
| [!UICONTROL  位置 ] | `position` | オブジェクト | 地理的な絶対位置。 Double 形式の 3 つのプロパティが含まれます。 <li>`longitude`：経度（WGS84 データム付き）</li> <li>`latitude`: WGS84 データムの緯度。</li> <li>`altitude`: WGS84 データムの高度。</li> |
| [!UICONTROL タイプ] | `type` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) の配列 | その場所で実行される関数のタイプ。 |
| [!UICONTROL  仮想サービス ] | `virtualService` | の配列 [[!UICONTROL  仮想サービスの詳細 ]](../data-types/virtual-service-detail.md) | 仮想サービスの接続の詳細。 |
| [!UICONTROL  エイリアス ] | `alias` | 文字列の配列 | その場所が現在の、または以前の名前の代替名のリスト。 |
| [!UICONTROL 説明] | `description` | 文字列 | 名前の後に場所を識別するための詳細情報。 |
| [!UICONTROL  モード ] | `mode` | 文字列 | 場所のモード。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `instance` </li> <li> `kind` </li> |
| [!UICONTROL 名前] | `name` | 文字列 | 場所の名前。 |
| [!UICONTROL ステータス] | `status` | 文字列 | 場所のステータス。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `active` </li> <li> `inactive` </li> <li> `suspended` </li> |

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/location.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/fieldgroups/location.schema.json)

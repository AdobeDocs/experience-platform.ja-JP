---
title: 拡張連絡先の詳細データ タイプ
description: 拡張連絡先詳細エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
exl-id: 4ac9b3d7-acc8-4a82-b34f-ec63a8bf12e0
source-git-commit: 3071d16b6b98040ea3f2e3a34efffae517253b8e
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 8%

---

# [!UICONTROL  拡張連絡先詳細 ] データタイプ

[!UICONTROL  拡張連絡先詳細 ] は、拡張連絡先の情報を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 拡張連絡先詳細データタイプ構造 ](../../../images/healthcare/data-types/extended-contact-detail.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL アドレス]](../data-types/address.md) | 連絡先の住所。 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL  人名 ]](../data-types/human-name.md) の配列 | 連絡する個人の名前。 |
| [!UICONTROL  組織 ] | `organization` | [[!UICONTROL  参考 ]](../data-types/reference.md) | 連絡先の詳細を処理/監視する組織。 |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../data-types/period.md) | 連絡先が使用に対して有効または有効だった期間。 |
| [!UICONTROL 目的] | `purpose` | [[!UICONTROL  コード化可能な概念 ]](../data-types/codeable-concept.md) | 連絡先のタイプ。 |
| [!UICONTROL  テレコム ] | `telecom` | [[!UICONTROL  連絡先 ]](../data-types/contact-point.md) の配列 | 連絡先の詳細。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/extendedcontactdetail.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/extendedcontactdetail.schema.json)

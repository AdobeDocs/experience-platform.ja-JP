---
title: 拡張連絡先の詳細データ タイプ
description: 拡張連絡先詳細エクスペリエンスデータモデル（XDM）データタイプについて説明します。
badgePrivateBeta: label="Private Beta" type="Informative"
hide: true
hidefromtoc: true
source-git-commit: 7f7de89a843d867d18ef84051eee69face0570a0
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 8%

---

# [!UICONTROL  拡張連絡先詳細 ] データタイプ

[!UICONTROL  拡張連絡先詳細 ] は、拡張連絡先の情報を記述する標準の Experience Data Model （XDM）データタイプです。 このデータタイプは、HL7 FHIR リリース 5 の仕様に従って作成されます。

![ 拡張連絡先詳細データタイプ構造 ](../../images/data-types/healthcare/extended-contact-detail.png)

| 表示名 | プロパティ | データタイプ | 説明 |
| --- | --- | --- | --- |
| [!UICONTROL アドレス] | `address` | [[!UICONTROL アドレス]](../healthcare/address.md) | 連絡先の住所。 |
| [!UICONTROL 名前] | `name` | [[!UICONTROL  人名 ]](../healthcare/human-name.md) の配列 | 連絡する個人の名前。 |
| [!UICONTROL  組織 ] | `organization` | [[!UICONTROL  参考 ]](../healthcare/reference.md) | 連絡先の詳細を処理/監視する組織。 |
| [!UICONTROL  期間 ] | `period` | [[!UICONTROL  期間 ]](../healthcare/period.md) | 連絡先が使用に対して有効または有効だった期間。 |
| [!UICONTROL 目的] | `purpose` | [[!UICONTROL  コード化可能な概念 ]](../healthcare/codeable-concept.md) | 連絡先のタイプ。 |
| [!UICONTROL  テレコム ] | `telecom` | [[!UICONTROL  連絡先 ]](../healthcare/contact-point.md) の配列 | 連絡先の詳細。 |

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/extendedcontactdetail.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/extensions/industry/healthcare/fhir/datatypes/extendedcontactdetail.schema.json)

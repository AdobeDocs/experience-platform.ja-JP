---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；データ型；データ型；データ型；
solution: Experience Platform
title: 地域データタイプ
topic-legacy: overview
description: このドキュメントでは、地域 XDM データタイプの概要を説明します。
exl-id: d0eef943-ef86-4abd-8a51-dc45f2ed782d
source-git-commit: dc81da58594fac4ce304f9d030f2106f0c3de271
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 37%

---

# [!UICONTROL 地域] データタイプ

[!UICONTROL 地域] は、イベントが観測された地域を示す標準的な XDM データ型です。

<img src="../images/data-types/geo.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema` | [[!UICONTROL 地理座標]](./geo-coordinates.md) | 場所の地理的座標を表します。 |
| `_id` | 文字列 | システムで生成された、座標の一意の ID。 |
| `city` | 文字列 | 市区町村の名前。 |
| `countryCode` | 文字列 | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `dmaID` | 整数 | Nielsen メディア研究が指定された市場領域。 |
| `msaID` | 整数 | 観測が行われた米国の大都市統計地域。 |
| `postalCode` | 文字列 | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `stateProvince` | 文字列 | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国および下位区分）](https://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/geo.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/geo.schema.json)

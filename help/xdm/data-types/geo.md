---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；地域；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 地域データ タイプ
description: Geo XDM データタイプについて説明します。
exl-id: d0eef943-ef86-4abd-8a51-dc45f2ed782d
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 52%

---

# [!UICONTROL Geo] データタイプ

[!UICONTROL Geo] は、イベントが観測された地理的領域を記述する標準の XDM データタイプです。

<img src="../images/data-types/geo.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema` | [[!UICONTROL  地理座標 ]](./geo-coordinates.md) | 場所の地理的座標を表します。 |
| `_id` | 文字列 | 座標の一意のシステム生成 ID。 |
| `city` | 文字列 | 市区町村の名前。 |
| `countryCode` | 文字列 | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `dmaID` | 整数 | ニールセンメディアリサーチが指定したマーケットエリア。 |
| `msaID` | 整数 | 観測が行われた米国の大都市統計地域。 |
| `postalCode` | 文字列 | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `stateProvince` | 文字列 | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国および下位区分）](https://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/geo.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/geo.schema.json)

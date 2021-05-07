---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；地理；データ型；データ型；
solution: Experience Platform
title: 地域データタイプ
topic-legacy: overview
description: このドキュメントでは、Geo XDMデータタイプの概要を説明します。
exl-id: d0eef943-ef86-4abd-8a51-dc45f2ed782d
translation-type: tm+mt
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 36%

---

#  Geodata型

 Geoisは、イベントが観察された地域を示す標準のXDMデータ型です。

<img src="../images/data-types/geo.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `_schema` | [[!UICONTROL 地理座標]](./geo-coordinates.md) | 場所の地理的座標を示します。 |
| `_id` | 文字列 | システムによって生成された、座標の一意のID。 |
| `city` | 文字列 | 市区町村の名前。 |
| `countryCode` | 文字列 | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `dmaID` | 整数 | ニールセンのメディアリサーチは、市場を指定した。 |
| `msaID` | 整数 | 観測が発生した米国の大都市圏統計地区。 |
| `postalCode` | 文字列 | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `stateProvince` | 文字列 | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国およびサブディビジョン）](http://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/geo.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/geo.schema.json)

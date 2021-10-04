---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ユーザー；データ型；データ型；データ型；
solution: Experience Platform
title: ユーザーデータタイプ
topic-legacy: overview
description: このドキュメントでは、Person Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: f28a52be-90c7-4ed0-a460-97165bb58046
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 17%

---

#  ペルソナデータ型

 ペルソナは、個人を記述する標準のエクスペリエンスデータモデル (XDM) データ型です。このデータ型は、顧客、連絡先、所有者など、様々な役割を果たす個人を表すことができます。

<img src="../images/data-types/person.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `name` | [[!UICONTROL ユーザー名]](./person-name.md) | ユーザーの姓に関する詳細を説明します。 |
| `birthDate` | 日付 | 生年月日（年月日）。日付形式（時刻なし）は、[RFC 3339 の 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 規格に従う必要があります。 |
| `birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。このプロパティの形式は、この正規表現 `[0-1][0-9]-[0-9][0-9]` に準拠している必要があります。 |
| `birthYear` | 整数 | 世紀を含めて人が生まれた年（例：`1983`）。 このフィールドは、正式な生年月日ではなく年齢のみがわかる場合に使用します。 この値は 1～32767の範囲で指定する必要があります。 |
| `gender` | 文字列 | 個人の性別 ID。 このプロパティの値は、次の既知の列挙値のいずれかと等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `not_specified` </li> <li> `non_specific` </li> この値のデフォルトは `not_specified` です。 |
| `maritalStatus` | 文字列 | 重要な相手との人の関係を表します。 このプロパティの値は、次の列挙値のいずれかと等しい必要があります。 <li> `married` </li> <li> `single` </li> <li> `divorced` </li> <li> `widowed` </li> <li> `not_specified` </li> この値のデフォルトは `not_specified` です。 |
| `nationality` | 文字列 | 個人とその州との法的関係は、ISO 3166-1 Alpha-2 コードを使用して表されます。 このプロパティの形式は、この正規表現 `^[A-Z]{2}$` に準拠している必要があります。 |
| `taxId` | 文字列 | 個人の税金または会計 ID。例えば、米国の納税者 ID 番号 (TIN) や、スペインの証明書 de Identificación 会計 (CIF/NIF) など。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；個人；データ型；データ型；
solution: Experience Platform
title: ユーザーデータタイプ
topic-legacy: overview
description: このドキュメントでは、Person Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: f28a52be-90c7-4ed0-a460-97165bb58046
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 17%

---

#  ペルソナデータタイプ

 ペルソナは、個人を記述する標準のExperience Data Model(XDM)データ型です。このデータ型は、顧客、連絡先、所有者など、様々な役割を果たす個人を表すことができます。

<img src="../images/data-types/person.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `name` | [[!UICONTROL ユーザー名]](./person-name.md) | ユーザーの姓名に関する詳細を説明します。 |
| `birthDate` | 日付 | 生年月日（年月日）。日付形式（時刻なし）は、[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。このプロパティの形式は、この正規表現`[0-1][0-9]-[0-9][0-9]`に準拠している必要があります。 |
| `birthYear` | 整数 | 世紀を含む人の生まれた年（例：`1983`）。 このフィールドは、正式な生年月日ではなく年齢のみがわかる場合に使用します。 この値は1 ～ 32767の範囲で指定する必要があります。 |
| `gender` | 文字列 | 個人の性別ID。 このプロパティの値は、次の既知の列挙値のいずれかと等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `not_specified` </li> <li> `non_specific` </li> この値のデフォルトは`not_specified`です。 |
| `maritalStatus` | 文字列 | 重要な人との人の関係を表します。 このプロパティの値は、次の列挙値のいずれかと等しい必要があります。 <li> `married` </li> <li> `single` </li> <li> `divorced` </li> <li> `widowed` </li> <li> `not_specified` </li> この値のデフォルトは`not_specified`です。 |
| `nationality` | 文字列 | ISO 3166-1 Alpha-2コードを使用して表される、個人とその州との法的関係。 このプロパティの形式は、この正規表現`^[A-Z]{2}$`に準拠している必要があります。 |
| `taxId` | 文字列 | 米国の納税者識別番号(TIN)やスペインの証明書(CIF/NIF)など、個人の税金または会計ID。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.schema.json)

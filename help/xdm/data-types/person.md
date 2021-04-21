---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；人；データ型；データ型；
solution: Experience Platform
title: 個人データタイプ
topic-legacy: overview
description: このドキュメントでは、Person Experience Data Model(XDM)データタイプの概要を説明します。
exl-id: f28a52be-90c7-4ed0-a460-97165bb58046
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 16%

---

#  Persondata型

 Personは、個人を説明する標準のExperience Data Model(XDM)データ型です。このデータ型は、顧客、連絡先、所有者など、様々な役割を果たす人を表すことができます。

<img src="../images/data-types/person.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `name` | [[!UICONTROL 個人名]](./person-name.md) | フルネームに関する詳細を説明します。 |
| `birthDate` | Date | 生年月日（年月日）。日付形式（時刻なし）は、[RFC 3339，セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。このプロパティの形式は、この正規式`[0-1][0-9]-[0-9][0-9]`に従う必要があります。 |
| `birthYear` | 整数 | 世紀を含む人が生まれた年（例：`1983`）。 このフィールドは、人の年齢だけがわかり、正式な生年月日ではない場合に使用します。 この値は1 ～ 32767の範囲で設定する必要があります。 |
| `gender` | 文字列 | 個人の性別ID。 このプロパティの値は、次の既知の列挙値のいずれかと等しくなければなりません。 <li> `female` </li> <li> `male` </li> <li> `not_specified` </li> <li> `non_specific` </li> この値のデフォルトは`not_specified`です。 |
| `maritalStatus` | 文字列 | 重要な人との関係を表します。 このプロパティの値は、次の列挙値のいずれかと等しくなければなりません。 <li> `married` </li> <li> `single` </li> <li> `divorced` </li> <li> `widowed` </li> <li> `not_specified` </li> この値のデフォルトは`not_specified`です。 |
| `nationality` | 文字列 | ISO 3166-1 Alpha-2コードを使用して表される、個人とその州との法的関係。 このプロパティの形式は、この正規式`^[A-Z]{2}$`に従う必要があります。 |
| `taxId` | 文字列 | 個人の税金または会計ID。例えば、米国の納税者ID番号(TIN)や、スペインのCertificade de Identificacion Fiscal(CIF/NIF)などです。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.schema.json)

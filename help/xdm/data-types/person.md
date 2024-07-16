---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；ユーザー；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 人物データタイプ
description: ユーザーエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: f28a52be-90c7-4ed0-a460-97165bb58046
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 7%

---

# [!UICONTROL Person] データタイプ

[!UICONTROL Person] は、個々のユーザーを記述する標準のエクスペリエンスデータモデル（XDM）データタイプです。 このデータタイプは、顧客、連絡先、所有者など、様々な役割を果たす人物を表すことができます。

<img src="../images/data-types/person.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `name` | [[!UICONTROL  人物名 ]](./person-name.md) | 人物のフルネームに関する詳細を説明します。 |
| `birthDate` | 日付 | 個人が誕生した完全な日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、年ではなく、誕生日が分かっている場合に使用する必要があります。 このプロパティの形式は、この正規表現 `[0-1][0-9]-[0-9][0-9]` に準拠する必要があります。 |
| `birthYear` | 整数 | 世紀を含む、個人が誕生した年（例：`1983`）。 このフィールドは、人物の年齢がわかっていて、完全な誕生日がない場合にのみ使用する必要があります。 この値は 1 から 32767 の間にする必要があります。 |
| `gender` | 文字列 | 人物の性自認。 このプロパティの値は、次の既知の列挙値のいずれかに等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `not_specified` </li> <li> `non_specific` </li> この値のデフォルトは `not_specified` です。 |
| `maritalStatus` | 文字列 | 人物と重要な他者との関係を表します。 このプロパティの値は、次の列挙値のいずれかに等しい必要があります。 <li> `married` </li> <li> `single` </li> <li> `divorced` </li> <li> `widowed` </li> <li> `not_specified` </li> この値のデフォルトは `not_specified` です。 |
| `nationality` | 文字列 | ISO 3166-1 Alpha-2 コードを使用して表される、個人とその国との間の法的関係。 このプロパティの形式は、この正規表現 `^[A-Z]{2}$` に準拠する必要があります。 |
| `taxId` | 文字列 | その人物の税金または納税者 ID （米国の Taxpayer Identificación Fiscal （CIF/NIF）やスペインの Certificado de Identificación Fiscal など）。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.schema.json)

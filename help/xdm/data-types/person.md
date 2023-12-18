---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；個人；データ型；データ型；データ型；
solution: Experience Platform
title: 人物データタイプ
description: Person Experience Data Model(XDM) データタイプについて説明します。
exl-id: f28a52be-90c7-4ed0-a460-97165bb58046
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 7%

---

# [!UICONTROL 人物] データタイプ

[!UICONTROL 人物] は、個人を表す標準の Experience Data Model(XDM) データ型です。 このデータ型は、顧客、連絡先、所有者など、様々な役割を果たす人物を表すことができます。

<img src="../images/data-types/person.PNG" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `name` | [[!UICONTROL 人物名]](./person-name.md) | 人物の姓名に関する詳細を表します。 |
| `birthDate` | 日付 | 人物が生まれた完全な日付。 日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかっているが、年がわからない場合に使用します。 このプロパティの形式は、この正規表現に準拠している必要があります `[0-1][0-9]-[0-9][0-9]`. |
| `birthYear` | 整数 | 世紀を含む、人が生まれた年 ( 例： `1983`) をクリックします。 このフィールドは、人の年齢だけがわかり、完全な生年月日がわからない場合に使用します。 この値は 1 ～ 32767の範囲で設定する必要があります。 |
| `gender` | 文字列 | 人物の性別アイデンティティ。 このプロパティの値は、次の既知の列挙値のいずれかと等しい必要があります。 <li> `female` </li> <li> `male` </li> <li> `not_specified` </li> <li> `non_specific` </li> この値のデフォルトはです。 `not_specified`. |
| `maritalStatus` | 文字列 | 人物と重要な他者との関係を表します。 このプロパティの値は、次の列挙値のいずれかと等しい必要があります。 <li> `married` </li> <li> `single` </li> <li> `divorced` </li> <li> `widowed` </li> <li> `not_specified` </li> この値のデフォルトはです。 `not_specified`. |
| `nationality` | 文字列 | ISO 3166-1Alpha-2 コードを使用して表される、個人とその国との法的関係。 このプロパティの形式は、この正規表現に準拠している必要があります `^[A-Z]{2}$`. |
| `taxId` | 文字列 | 個人の税金または会計 ID ( 米国の納税者識別番号 (TIN) やスペインの証明書 de Identificación 会計 (CIF/NIF) など )。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/person/person.schema.json)

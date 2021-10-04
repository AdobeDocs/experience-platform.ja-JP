---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アドレス；xdm:address；データ型；データ型；
solution: Experience Platform
title: 郵送先住所データタイプ
topic-legacy: overview
description: このドキュメントでは、 Postal Address XDM データタイプの概要を説明します。
exl-id: 94457fe5-80bc-4822-9f6c-48f77d56c89b
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '341'
ht-degree: 24%

---

# [!UICONTROL 郵送先住] 所データ型

[!UICONTROL 郵送先] 住所は、郵送先住所の詳細を記述する標準の XDM データ型です。

<img src="../images/data-types/postal-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `city` | 市区町村の名前。 |
| `country` | 政府が管理する領土の名前。これは、任意の言語で国名を付けることができる自由形式のフィールドです。 |
| `countryCode` | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `createdByBatchID` | アドレスレコードを作成した、取得したバッチファイルの ID。 |
| `dmaID` | Nielsen メディア研究指定市場領域。 |
| `label` | 住所の自由形式の名前。 |
| `lastVerifiedDate` | アドレスが個人に関連付けられていることを最後に確認した日付。 |
| `modifiedByBatchID` | レコードを最後に変更した、取り込んだバッチファイルの ID。 |
| `msaID` | 観測が行われた米国の大都市統計地域。 |
| `postOfficeBox` | 住所の郵便局のボックス。 |
| `postalCode` | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `primary` | このアドレスが個人のプライマリアドレスであるかどうかを示す Boolean 値です。 1 つのプロファイルに指定できる `primary` アドレスは、特定の時点で 1 つだけです。 |
| `region` | 住所の地域、郡または地域の部分。 |
| `repositoryCreatedBy` | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーの ID。 |
| `stateProvince` | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国および下位区分）](http://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |
| `status` | アドレスが現在使用可能かどうかを示します。 |
| `statusReason` | 現在の `status` の説明。 |
| `street1` -  `street4` | これらの 4 つのフィールドには、主な番地レベルの情報、アパート番号、番地、および番地名が含まれます。 `street2` はオプ `street4` ションです。 |

{style=&quot;table-layout:auto&quot;}

郵送先住所のデータタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/address.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/address.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アドレス；xdm:address；データ型；データ型；
solution: Experience Platform
title: 郵送先住所のデータ型
topic-legacy: overview
description: このドキュメントでは、住所XDMデータタイプの概要を説明します。
exl-id: 94457fe5-80bc-4822-9f6c-48f77d56c89b
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 23%

---

# [!UICONTROL 郵送先] 住所データ型

[!UICONTROL 郵送先] 住所は、郵送先住所の詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/postal-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `city` | 市区町村の名前。 |
| `country` | 政府が管理する領土の名前。これは、任意の言語で国名を付けることができる自由形式のフィールドです。 |
| `countryCode` | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `createdByBatchID` | アドレスレコードを作成した、取り込んだバッチファイルのID。 |
| `dmaID` | Nielsenメディア研究指定市場領域。 |
| `label` | 住所の自由形式の名前。 |
| `lastVerifiedDate` | アドレスが個人に関連付けられていることを最後に確認した日付。 |
| `modifiedByBatchID` | レコードを最後に変更した、取り込んだバッチファイルのID。 |
| `msaID` | 観測が行われた米国の大都市統計地域。 |
| `postOfficeBox` | 住所の郵便局のボックス。 |
| `postalCode` | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `primary` | このアドレスが個人のプライマリアドレスであるかどうかを示すBoolean値です。 1つのプロファイルに指定できるアドレスは、一定の時点で1つの`primary`のみです。 |
| `region` | 住所の地域、郡または地域の部分。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |
| `stateProvince` | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国およびサブディビジョン）](http://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |
| `status` | アドレスが現在使用可能かどうかを示します。 |
| `statusReason` | 現在の`status`の説明。 |
| `street1` - `street4` | これらの4つのフィールドには、主な番地レベルの情報、アパート番号、番地番号、および番地名が含まれます。 `street2` はオプ `street4` ションです。 |

{style=&quot;table-layout:auto&quot;}

郵送先住所のデータタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/address.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/address.schema.json)

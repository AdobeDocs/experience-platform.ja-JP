---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；アドレス；xdm：アドレス；データ型；データ型；
solution: Experience Platform
title: 住所のデータの種類
topic-legacy: overview
description: このドキュメントでは、Postal Address XDMデータ型の概要を説明します。
exl-id: 94457fe5-80bc-4822-9f6c-48f77d56c89b
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 22%

---

# [!UICONTROL 郵便] 先住所のデータ型

[!UICONTROL 郵便番号] は、住所の詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/postal-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `city` | 市区町村の名前。 |
| `country` | 政府が管理する領土の名前。これは、任意の言語で国名を持つことができる自由形式のフィールドです。 |
| `countryCode` | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `createdByBatchID` | アドレスレコードを作成した、取り込んだバッチファイルのID。 |
| `dmaID` | ニールセンのメディアリサーチは、市場を指定した。 |
| `label` | アドレスのフリーフォーム名。 |
| `lastVerifiedDate` | 住所が最後に個人に関連付けられていると確認された日付。 |
| `modifiedByBatchID` | レコードを最後に変更した、取り込んだバッチファイルのID。 |
| `msaID` | 観測が発生した米国の大都市圏統計地区。 |
| `postOfficeBox` | 住所の郵便局のボックス。 |
| `postalCode` | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `primary` | これが個人のプライマリアドレスであるかどうかを示すBoolean値です。 1人のプロファイルに指定できる`primary`アドレスは1つだけです。 |
| `region` | 住所の地域、郡または地域の部分。 |
| `repositoryCreatedBy` | レコードを作成したユーザーのID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーのID。 |
| `stateProvince` | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国およびサブディビジョン）](http://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |
| `status` | アドレスを現在使用できるかどうかを示します。 |
| `statusReason` | 現在の`status`の説明。 |
| `street1` -  `street4` | これらの4つのフィールドは、主な通りのレベル情報、アパート番号、番地、および番地を含めるためのものです。 `street2` はオプション `street4` です。 |

住所データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/address.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/address.schema.json)

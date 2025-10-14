---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；アドレス；xdm：アドレス；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 郵送先住所データタイプ
description: 住所 XDM データタイプについて説明します。
exl-id: 94457fe5-80bc-4822-9f6c-48f77d56c89b
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 35%

---

# [!UICONTROL &#x200B; 住所 &#x200B;] データタイプ

[!UICONTROL &#x200B; 郵送先住所 &#x200B;] は、郵送先住所の詳細を記述する標準の XDM データタイプです。

![](../images/data-types/postal-address.png){width=450}

| プロパティ | 説明 |
| --- | --- |
| `city` | 市区町村の名前。 |
| `country` | 政府が管理する領土の名前。これは、任意の言語で国名を持つことができる自由形式フィールドです。 |
| `countryCode` | 国の 2 文字の <a href="https://datahub.io/core/country-list">ISO 3166-1 alpha-2</a> コード。 |
| `createdByBatchID` | アドレスレコードを作成した、取り込まれたバッチファイルの ID。 |
| `dmaID` | ニールセンメディアリサーチが指定したマーケットエリア。 |
| `label` | 住所の自由形式の名前。 |
| `lastVerifiedDate` | アドレスが人物に関連していることが最後に検証された日付。 |
| `modifiedByBatchID` | レコードを最後に変更した、取り込まれたバッチファイルの ID。 |
| `msaID` | 観測が行われた米国の大都市統計地域。 |
| `postOfficeBox` | その住所の私書箱。 |
| `postalCode` | 場所の郵便番号。一部の国には郵便番号がありません。一部の国では、郵便番号の一部のみが含まれます。 |
| `primary` | これが個人のプライマリアドレスかどうかを示すブール値。 プロファイルは、特定の時点で `primary` アドレスを 1 つのみ持つことができます。 |
| `region` | 住所の地域、郡または地域の部分。 |
| `repositoryCreatedBy` | レコードを作成したユーザーの ID。 |
| `repositoryLastModifiedBy` | レコードを最後に変更したユーザーの ID。 |
| `stateProvince` | 観測される州、または都道府県の部分。この形式は、[ISO 3166-2（国および下位区分）](https://www.unece.org/cefact/locode/subdivisions.html)規格に従います。 |
| `status` | アドレスが現在使用できるかどうかを示します。 |
| `statusReason` | 現在の `status` の説明。 |
| `street1` - `street4` | これらの 4 つのフィールドは、主要なストリートレベルの情報、アパート番号、ストリート番号、ストリート名を含むためのものです。 `street2`～`street4` はオプションです。 |

{style="table-layout:auto"}

住所データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/address.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/address.schema.json)

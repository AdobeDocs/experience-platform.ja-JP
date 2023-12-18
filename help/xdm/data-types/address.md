---
title: 郵送先住所のデータタイプ
description: Postal Address Experience Data Model(XDM) データタイプについて説明します。
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 35%

---

# [!UICONTROL 郵送先住所] データタイプ

[!UICONTROL 郵送先住所] は、住所の詳細を提供する標準の Experience Data Model(XDM) データ型です。

![の図 [!UICONTROL 郵送先住所] データタイプ。](../images/data-types/postal-address.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|------------------------------------|------------------|-----------|-----------------------------------------------------------------------------------------------|
| [!UICONTROL プライマリ] | `primary` | ブール値 | プライマリの住所インジケーター。 プロファイルに設定できるのは 1 つだけです `primary` 特定の時点のアドレス。 |
| [!UICONTROL ラベル] | `label` | 文字列 | アドレスの自由形式名。 |
| [!UICONTROL 番地 1] | `street1` | 文字列 | プライマリの番地の情報、アパート番号、番地、および番地。 |
| [!UICONTROL 番地 2] | `street2` | 文字列 | 2 行目（オプション） |
| [!UICONTROL 番地 3] | `street3` | 文字列 | 3 行目（オプション） |
| [!UICONTROL 番地 4] | `street4` | 文字列 | 4 行目（オプション） |
| [!UICONTROL 地域] | `region` | 文字列 | 住所の地域、郡または地域の部分。 |
| [!UICONTROL 私書箱] | `postOfficeBox` | 文字列 | 住所の私書箱 |
| [!UICONTROL 国] | `country` | 文字列 | 政府が管理する領土の名前。``countryCode`` 以外で、任意の言語で国名を付けることができる自由形式のフィールドです。 |
| [!UICONTROL 都道府県] | `state` | 文字列 | 都道府県の名前。 これは自由形式のフィールドです。 |
| [!UICONTROL ステータス] | `status` | 文字列 | その住所を使用できるかどうかを示します。 |
| [!UICONTROL ステータスの理由] | `statusReason` | 文字列 | 現在のステータスの説明。 |
| [!UICONTROL 最終検証日] | `lastVerifiedDate` | 文字列 | アドレスが人物と関連付けられていることを最後に検証した日付。 |

{style="table-layout:auto"}

データタイプについて詳しくは、 [フルスキーマ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/address.schema.json) パブリック XDM リポジトリで次の操作を実行します。

---
title: 郵送先住所データタイプ
description: 住所エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 92385cd8-60c8-4360-a8e7-e6224e85e4d4
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 51%

---

# [!UICONTROL  住所 ] データタイプ

[!UICONTROL  郵送先住所 ] は、住所の詳細を提供する標準の Experience Data Model （XDM）データタイプです。

![[!UICONTROL  郵送先住所 ] データタイプの図。](../images/data-types/postal-address.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|------------------------------------|------------------|-----------|-----------------------------------------------------------------------------------------------|
| [!UICONTROL プライマリ] | `primary` | ブール値 | プライマリアドレスインジケーター。 プロファイルは、特定の時点で `primary` アドレスを 1 つのみ持つことができます。 |
| [!UICONTROL ラベル] | `label` | 文字列 | アドレスの自由形式名。 |
| [!UICONTROL  通り 1] | `street1` | 文字列 | 主要ストリートレベルの情報 (番地、アパート番号、ストリート番号、ストリート名)。 |
| [!UICONTROL  住所 2] | `street2` | 文字列 | 2 行目（オプション） |
| [!UICONTROL 3 番地 ] | `street3` | 文字列 | 3 行目（オプション） |
| [!UICONTROL 4 番地 ] | `street4` | 文字列 | 4 行目（オプション） |
| [!UICONTROL  地域 ] | `region` | 文字列 | 住所の地域、郡または地域の部分。 |
| [!UICONTROL Post office box] | `postOfficeBox` | 文字列 | 住所の私書箱 |
| [!UICONTROL 国] | `country` | 文字列 | 政府が管理する領土の名前。``countryCode`` 以外で、任意の言語で国名を付けることができる自由形式のフィールドです。 |
| [!UICONTROL  都道府県 ] | `state` | 文字列 | 都道府県の名前。これはフリーフォームフィールドです。 |
| [!UICONTROL ステータス] | `status` | 文字列 | その住所を使用できるかどうかを示します。 |
| [!UICONTROL  ステータスの理由 ] | `statusReason` | 文字列 | 現在のステータスの説明。 |
| [!UICONTROL  最終検証日 ] | `lastVerifiedDate` | 文字列 | アドレスが人物に関連付けられていることが最後に検証された日付。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリの [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/docs/reference/datatypes/address.schema.json) を参照してください。

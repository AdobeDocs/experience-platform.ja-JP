---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；通信；購読；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 通信購読データタイプ
description: 通信購読エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: d67915b6-daaa-489f-81b4-bd3dbe0ffa44
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 20%

---

# [!UICONTROL  通信購読 ] データタイプ

[!UICONTROL  通信購読 ] は、特定の通信購読タイプ（インターネット、モバイル、メディア、固定電話など）の詳細を記述する標準の Experience Data Model （XDM）データタイプです。

>[!NOTE]
>
>このドキュメントでは、データタイプについて説明します。 同じ名前のフィールドグループについては、[[!UICONTROL Telecom Subscription] フィールドグループリファレンスガイド ](../field-groups/profile/telecom-subscription.md) を参照してください。
>
>通信業界に関係のないサブスクリプションタイプを説明している場合は、代わりに汎用 [[!UICONTROL  サブスクリプション ] データタイプ ](./subscription.md) を使用してください。

![Telecom Subscription structure](../images/data-types/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `devices` | オブジェクトの配列 | プランに関連付けられたデバイスやアクセサリのリストを表します。 各配列項目の想定される構造について詳しくは、以下の [ 節 ](#devices) を参照してください。 |
| `subscriber` | [[!UICONTROL  人物 ]](./person.md) | サブスクリプションの所有者を表します。 |
| `ID` | 文字列 | 購読インスタンスの一意の ID。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 請求期間が開始される日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `chargeMethod` | 文字列 | 顧客に請求するための請求書の設定方法。 |
| `contractID` | 文字列 | このサブスクリプションに適用される契約の一意の ID。 |
| `country` | 文字列 | サブスクリプションの契約条件および合意条件が定着している国。 |
| `endDate` | 日付 | 現在のサブスクリプション期間が終了する日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `paymentDueDate` | 日付 | サブスクリプションの支払い期日。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | アカウントの支払い状況。 |
| `planName` | 文字列 | サブスクリプションの人間が読み取れる名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用に対して持っている一般的な目的。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `startDate` | 日付 | サブスクリプションが開始する日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `status` | 文字列 | サブスクリプションの現在のステータス。 |
| `subscriptionCategory` | 文字列 | このタイプの購読の主なトップレベルの分類。 |
| `subscriptionSKU` | 文字列 | サブスクリプションの最小在庫管理単位（SKU）。 |
| `subscriptionSubCategory` | 文字列 | サブスクリプションの具体的なサブカテゴリ。 |
| `term` | 整数 | サブスクリプション期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中のサブスクリプションの消耗品要素の再購入方法に関して合意した条件を説明します。 |
| `type` | 文字列 | サブスクリプションでカバーされる人数に関する使用権限の範囲。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

## `devices` {#devices}

`devices` はオブジェクトの配列で、各オブジェクトはサブスクリプションに関連付けられたデバイスまたはアクセサリを表します。

![ デバイスアレイ構造 ](../images/data-types/telecom-subscription/devices.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `deviceFees` | オブジェクト | ルーター、モデム、受信機など、任意のデバイス料金をキャプチャするオブジェクトです。 では、次のプロパティが必要です。<ul><li>`amount`:`currencyCode` で表される金額。</li><li>`conversionDate`：通貨換算が行われた日付。</li><li>`currencyCode`:`amount` の [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) 通貨コード。</li></ul> |
| `ID` | 文字列 | デバイスの一意の ID。 |
| `OS` | 文字列 | デバイスのオペレーティングシステム。 |
| `deviceInsurance` | 文字列 | 顧客がこのデバイスの保険にオプトインしたかどうかを示します。 |
| `manufacturer` | 文字列 | デバイスの製造元。 |
| `name` | 文字列 | デバイスの名前。 |
| `paymentOptions` | 文字列 | デバイスが分割払いまたは定価のどちらで支払われるかを示します。 |
| `serialNumber` | 文字列 | デバイスのシリアル番号 |
| `status` | 文字列 | デバイスのステータス。 |
| `storageCapacity` | 文字列 | デバイスのストレージ容量。 |
| `type` | 文字列 | デバイスタイプ。 |

{style="table-layout:auto"}

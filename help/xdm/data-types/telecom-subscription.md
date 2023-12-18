---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；通信；購読；データ型；データ型；
solution: Experience Platform
title: テレコムサブスクリプションデータタイプ
description: 通信サブスクリプションエクスペリエンスデータモデル (XDM) のデータタイプについて説明します。
exl-id: d67915b6-daaa-489f-81b4-bd3dbe0ffa44
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 13%

---

# [!UICONTROL 通信配信登録] データタイプ

[!UICONTROL 通信配信登録] は、インターネット、モバイル、メディア、固定電話など、特定の通信サブスクリプションの種類の詳細を記述する標準の Experience Data Model(XDM) データ型です。

>[!NOTE]
>
>このドキュメントでは、データタイプについて説明します。 同じ名前のフィールドグループについては、 [[!UICONTROL 通信配信登録] フィールドグループリファレンスガイド](../field-groups/profile/telecom-subscription.md).
>
>通信業界とは無関係なサブスクリプションタイプを記述する場合は、汎用の [[!UICONTROL 購読] データタイプ](./subscription.md) 代わりに、

![通信購読構造](../images/data-types/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `devices` | オブジェクトの配列 | プランに関連するデバイスやアクセサリのリストを表します。 詳しくは、 [以下の節](#devices) を参照してください。 |
| `subscriber` | [[!UICONTROL ユーザー]](./person.md) | 購読の所有者を表します。 |
| `ID` | 文字列 | サブスクリプションインスタンスの一意の ID。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 請求期間が開始される日付。 日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `chargeMethod` | 文字列 | 顧客に対する請求の設定方法。 |
| `contractID` | 文字列 | この購読を管理する契約の一意の ID。 |
| `country` | 文字列 | サブスクリプションの契約条件および契約条件が根付いている国。 |
| `endDate` | 日付 | 現在の購読期間が終了する日付。 日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `paymentDueDate` | 日付 | サブスクリプションの支払い期日。 日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | アカウントの支払い状態。 |
| `planName` | 文字列 | 人間が読み取り可能なサブスクリプションの名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用に対して持つ一般的な目的。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `startDate` | 日付 | サブスクリプションが開始される日付。 日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `status` | 文字列 | 配信登録の現在のステータス。 |
| `subscriptionCategory` | 文字列 | このタイプのサブスクリプションの主な最上位レベルの分類。 |
| `subscriptionSKU` | 文字列 | 購読の在庫管理単位 (SKU)。 |
| `subscriptionSubCategory` | 文字列 | 購読の特定のサブカテゴリ。 |
| `term` | 整数 | 購読期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中にサブスクリプションの消耗品を再購入する方法に関して同意された条件を説明します。 |
| `type` | 文字列 | サブスクリプションの対象となる人数に関するエンタイトルメントの範囲。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

## `devices` {#devices}

`devices` はオブジェクトの配列で、各オブジェクトはサブスクリプションに関連付けられたデバイスまたはアクセサリを表します。

![デバイスの配列構造](../images/data-types/telecom-subscription/devices.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `deviceFees` | オブジェクト | ルーター、モデム、レシーバーなどの項目のデバイス料金を取り込むオブジェクト。 次のプロパティが必要です。<ul><li>`amount`：金額を `currencyCode`.</li><li>`conversionDate`：通貨換算がおこなわれた日付。</li><li>`currencyCode`: [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) 通貨コード `amount`.</li></ul> |
| `ID` | 文字列 | デバイスの一意の ID。 |
| `OS` | 文字列 | デバイスのオペレーティングシステム。 |
| `deviceInsurance` | 文字列 | 顧客がこのデバイスの保険にオプトインしたかどうかを示します。 |
| `manufacturer` | 文字列 | デバイスの製造元。 |
| `name` | 文字列 | デバイスの名前。 |
| `paymentOptions` | 文字列 | デバイスが分割払いで支払われるか、定価で支払われるかを示します。 |
| `serialNumber` | 文字列 | デバイスのシリアル番号。 |
| `status` | 文字列 | デバイスのステータス。 |
| `storageCapacity` | 文字列 | デバイスのストレージ容量。 |
| `type` | 文字列 | デバイスタイプ。 |

{style="table-layout:auto"}

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；通信；購読；データ型；データ型；
solution: Experience Platform
title: 通信購読データタイプ
topic-legacy: overview
description: このドキュメントでは、通信購読エクスペリエンスデータモデル(XDM)のデータタイプの概要を説明します。
source-git-commit: 19675e4042c28061a4b2ed4e68374d5e09216ba1
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 18%

---

# [!UICONTROL Telecom Subscriptiondata ] type

[!UICONTROL 通信購] 読は、インターネット、モバイル、メディア、固定回線など、特定の通信購読タイプの詳細を記述する、標準のExperience Data Model(XDM)データタイプです。

>[!NOTE]
>
>このドキュメントでは、データタイプについて説明します。 同じ名前のフィールドグループについては、[[!UICONTROL Telecom Subscription]フィールドグループリファレンスガイド](../field-groups/profile/telecom-subscription.md)を参照してください。
>
>通信業界とは無関係な購読タイプを記述する場合は、代わりに、汎用の[[!UICONTROL 購読]データタイプ](./subscription.md)を使用してください。

![通信購読構造](../images/data-types/telecom-subscription/structure.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `devices` | オブジェクトの配列 | プランに関連するデバイスやアクセサリのリストを示します。 各配列項目の想定される構造の詳細については、](#devices)の下の[の節を参照してください。 |
| `subscriber` | [[!UICONTROL ユーザー]](./person.md) | 購読の所有者を示します。 |
| `ID` | 文字列 | 購読インスタンスの一意の識別子。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 請求期間が開始される日付。 日付形式（時刻なし）は、[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `chargeMethod` | 文字列 | 顧客に対する請求の設定方法。 |
| `contractID` | 文字列 | この購読を管理する契約の一意のID。 |
| `country` | 文字列 | 購読契約条件および契約条件のルートにある国。 |
| `endDate` | 日付 | 現在の購読期間終了日。日付形式（時刻なし）は、[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `paymentDueDate` | 日付 | 購読支払の期日。 日付形式（時刻なし）は、[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | 口座の支払状況。 |
| `planName` | 文字列 | 人間が読み取り可能な購読の名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用を目的とする一般的な目的。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `startDate` | 日付 | 購読開始日。日付形式（時刻なし）は、[RFC 3339、セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `status` | 文字列 | 購読の現在のステータス。 |
| `subscriptionCategory` | 文字列 | このタイプの購読のメインの最上位カテゴリ。 |
| `subscriptionSKU` | 文字列 | サブスクリプションの在庫管理単位(SKU)。 |
| `subscriptionSubCategory` | 文字列 | 購読の特定のサブカテゴリ。 |
| `term` | 整数 | 購読期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中に購読の消耗品を再購入する方法に関して合意された条件について説明します。 |
| `type` | 文字列 | 購読の対象となる人数に関する権利の範囲。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

## `devices` {#devices}

`devices` はオブジェクトの配列で、各オブジェクトは購読に関連付けられたデバイスまたはアクセサリを表します。

![デバイスの配列構造](../images/data-types/telecom-subscription/devices.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `deviceFees` | オブジェクト | ルータ、モデム、受信機などの項目のデバイス料金を取得するオブジェクト。 次のプロパティが必要です。<ul><li>`amount`:で表される金額 `currencyCode`。</li><li>`conversionDate`:通貨換算が行われた日付。</li><li>`currencyCode`:の [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) 通貨コー `amount`ド。</li></ul> |
| `ID` | 文字列 | デバイスの一意のID。 |
| `OS` | 文字列 | デバイスのオペレーティングシステム。 |
| `deviceInsurance` | 文字列 | 顧客がこのデバイスの保険をオプトインしたかどうかを示します。 |
| `manufacturer` | 文字列 | デバイスの製造元。 |
| `name` | 文字列 | デバイスの名前。 |
| `paymentOptions` | 文字列 | デバイスが分割払いで支払われるか、完全な小売価格で支払われるかを示します。 |
| `serialNumber` | 文字列 | デバイスのシリアル番号。 |
| `status` | 文字列 | デバイスのステータス。 |
| `storageCapacity` | 文字列 | デバイスのストレージ容量。 |
| `type` | 文字列 | デバイスのタイプ。 |

{style=&quot;table-layout:auto&quot;}
---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；フィールド；スキーマ；スキーマ；サブスクリプション；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: 購読データタイプ
description: 購読エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 6fd1e073-441b-45f0-bb4f-54f51ab18694
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 29%

---

# [!UICONTROL  購読 ] データタイプ

[!UICONTROL  サブスクリプション ] は、標準のエクスペリエンスデータモデル（XDM）データタイプで、時間や使用状況に基づいて利用するソフトウェア、サービス、商品に対するライセンスされたエンタイトルメントを記述します。

<img src="../images/data-types/subscription-data-type.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [[!UICONTROL  デバイス ]](./device.md) | サブスクリプションにリンクされているデバイスに関する詳細を示します。 |
| `environment` | [[!UICONTROL 環境]](./environment.md) | イベントが観測された周囲の状況に関する情報が含まれます。特に、ネットワークやソフトウェアバージョンなどの一時的な情報を説明します。 |
| `subscriber` | [[!UICONTROL  人物 ]](./person.md) | 個人を表します。 また、これは、顧客、連絡先、所有者など、様々な役割を果たす人物を表すこともできます。 |
| `SKU` | 文字列 | 最小在庫管理単位（SKU）。商品の一意の ID。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 最初の請求書の支払い期日。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `category` | 文字列 | このタイプの購読の主なトップレベルの分類。 |
| `chargeMethod` | 文字列 | 顧客に請求するための請求書の設定方法。 |
| `contractID` | 文字列 | このサブスクリプションに適用される契約の一意の ID。 |
| `country` | 文字列 | サブスクリプションの契約条件および合意条件が定着している国。 |
| `endDate` | 日付 | 現在のサブスクリプション期間が終了する日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | アカウントの支払い状況。 |
| `planName` | 文字列 | サブスクリプションの人間が読み取れる名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用に対して持っている一般的な目的。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `revision` | 文字列 | 同じ名前の購読と階層のカテゴリの ID。 |
| `startDate` | 日付 | サブスクリプションが開始する日付。 日付形式（時間なし）は、[RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準に従う必要があります。 |
| `status` | 文字列 | サブスクリプションの現在のステータス。 |
| `subCategory` | 文字列 | サブスクリプションの具体的なサブカテゴリ。 |
| `term` | 整数 | サブスクリプション期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中のサブスクリプションの消耗品要素の再購入方法に関して合意した条件を説明します。 |
| `type` | 文字列 | サブスクリプションでカバーされる人数に関する使用権限の範囲。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

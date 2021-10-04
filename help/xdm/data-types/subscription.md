---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；購読；データ型；データ型；
solution: Experience Platform
title: 購読データタイプ
topic-legacy: overview
description: このドキュメントでは、Subscription Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: 6fd1e073-441b-45f0-bb4f-54f51ab18694
source-git-commit: d99ddc65849a88350bf61977b399b07989554426
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 25%

---

#  Subscriptiondata type

 サブスクリプションは、時間や使用状況に基づいて使用されるソフトウェア、サービス、商品に対するライセンス付与の権限を記述する、標準の Experience Data Model(XDM) データ型です。

<img src="../images/data-types/subscription-data-type.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [[!UICONTROL デバイス]](./device.md) | サブスクリプションにリンクされているデバイスに関する詳細を説明します。 |
| `environment` | [[!UICONTROL 環境]](./environment.md) | イベント監視が発生した周囲の状況に関する情報が含まれ、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細が記述されます。 |
| `subscriber` | [[!UICONTROL ユーザー]](./person.md) | 個人を表します。 また、顧客、連絡先、所有者など、様々な役割を果たす個人を表すこともできます。 |
| `SKU` | 文字列 | 在庫管理単位 (SKU)。製品の一意の識別子です。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 最初の請求の支払期日。日付形式（時刻なし）は、[RFC 3339 の 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 規格に従う必要があります。 |
| `category` | 文字列 | このタイプの購読のメインの最上位カテゴリ。 |
| `chargeMethod` | 文字列 | 顧客に対する請求の設定方法。 |
| `contractID` | 文字列 | この購読を管理する契約の一意の ID。 |
| `country` | 文字列 | 購読契約条件および契約条件が根付いている国。 |
| `endDate` | 日付 | 現在の購読期間終了日。日付形式（時刻なし）は、[RFC 3339 の 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 規格に従う必要があります。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | 口座の支払状況。 |
| `planName` | 文字列 | 人間が読み取り可能な購読の名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用を目的とする一般的な意図。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `revision` | 文字列 | 同じ名前の購読と階層のカテゴリの ID。 |
| `startDate` | 日付 | 購読開始日。日付形式（時刻なし）は、[RFC 3339 の 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 規格に従う必要があります。 |
| `status` | 文字列 | 購読の現在のステータス。 |
| `subCategory` | 文字列 | 購読の特定のサブカテゴリ。 |
| `term` | 整数 | 購読期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中に購読の消耗品を再購入する方法に関して同意された条件について説明します。 |
| `type` | 文字列 | 購読の対象となる人数に関する権利の範囲。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；購読；データ型；データ型；
solution: Experience Platform
title: 購読データタイプ
description: このドキュメントでは、Subscription Experience Data Model(XDM) データタイプの概要を説明します。
exl-id: 6fd1e073-441b-45f0-bb4f-54f51ab18694
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '420'
ht-degree: 25%

---

# [!UICONTROL 購読] データタイプ

[!UICONTROL 購読] は標準の Experience Data Model(XDM) データ型です。これは、時間や使用状況に基づいて使用されるソフトウェア、サービスまたは商品に対するライセンスされた使用権限を記述したものです。

<img src="../images/data-types/subscription-data-type.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [[!UICONTROL デバイス]](./device.md) | サブスクリプションにリンクされているデバイスに関する詳細を説明します。 |
| `environment` | [[!UICONTROL 環境]](./environment.md) | イベント監視が発生した周囲の状況に関する情報が含まれます。特に、ネットワークやソフトウェアのバージョンなどの一時的な情報の詳細が記載されます。 |
| `subscriber` | [[!UICONTROL 人物]](./person.md) | 個人を表します。 また、顧客、連絡先、所有者など、様々な役割を果たす人物を表すこともできます。 |
| `SKU` | 文字列 | 在庫管理単位 (SKU)。製品の一意の ID。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | 日付 | 最初の請求の支払期日。日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `category` | 文字列 | このタイプのサブスクリプションの主な最上位レベルの分類。 |
| `chargeMethod` | 文字列 | 顧客に対する請求の設定方法。 |
| `contractID` | 文字列 | この購読を管理する契約の一意の ID。 |
| `country` | 文字列 | サブスクリプションの契約条件および契約条件が根付いている国。 |
| `endDate` | 日付 | 現在の購読期間終了日。日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | アカウントの支払い状態。 |
| `planName` | 文字列 | 人間が読み取れるサブスクリプションの名前。 |
| `reason` | 文字列 | メンバーがサブスクリプションの使用に対して持つ一般的な目的。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `revision` | 文字列 | 同じ名前の購読と階層のカテゴリの ID。 |
| `startDate` | 日付 | 購読開始日。日付形式（時刻なし）は、 [RFC 3339、セクション 5.6](https://tools.ietf.org/html/rfc3339#section-5.6) 標準。 |
| `status` | 文字列 | 購読の現在のステータス。 |
| `subCategory` | 文字列 | 購読の特定のサブカテゴリ。 |
| `term` | 整数 | 購読期間の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中にサブスクリプションの消耗品を再購入する方法に関して同意された条件を説明します。 |
| `type` | 文字列 | サブスクリプションの対象となる人数に関するエンタイトルメントの範囲。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;購読；データ型；データ型；
solution: Experience Platform
title: 購読データタイプ
topic-legacy: overview
description: このドキュメントでは、購読エクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
exl-id: 6fd1e073-441b-45f0-bb4f-54f51ab18694
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 27%

---

# [!UICONTROL Subscriptiondata] 型

[!UICONTROL Subscriptionは、] 標準のExperience Data Model(XDM)データ型です。時間や使用量に基づいて使用されるソフトウェア、サービス、または商品に対するライセンス付与の権利を記述します。

<img src="../images/data-types/subscription-data-type.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [[!UICONTROL デバイス]](./device.md) | 購読にリンクされているデバイスの詳細を説明します。 |
| `environment` | [[!UICONTROL 環境]](./environment.md) | イベント監視が発生した周囲の状況に関する情報が含まれ、特にネットワークやソフトウェアのバージョンなどの一時的な情報が詳細に示されます。 |
| `subscriber` | [[!UICONTROL ユーザー]](./person.md) | 個人を表します。 これは、顧客、連絡先、所有者など、様々な役割を持つ人物を表すこともできます。 |
| `SKU` | 文字列 | 在庫管理ユニット(SKU)。製品の一意の識別子です。 |
| `billingPeriod` | 文字列 | 請求の間隔。 |
| `billingStartDate` | Date | 最初の請求の支払期日。日付形式（時刻なし）は、[RFC 3339，セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `category` | 文字列 | この種類の購読のメインの最上位レベルの分類。 |
| `chargeMethod` | 文字列 | 顧客に対する請求の設定方法。 |
| `contractID` | 文字列 | この購読を管理する契約の一意のID。 |
| `country` | 文字列 | 購読契約および契約条件が根付いている国。 |
| `endDate` | 日付 | 現在の購読期間終了日。日付形式（時刻なし）は、[RFC 3339，セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `paymentMethod` | 文字列 | 定期支払の支払方法。 |
| `paymentStatus` | 文字列 | 口座の支払状況。 |
| `planName` | 文字列 | 購読のわかりやすい名前。 |
| `reason` | 文字列 | 購読の使用に対するメンバーの一般的な意図。 |
| `renew` | 文字列 | 購読が終了日後も継続するという合意された方法。 |
| `revision` | 文字列 | 同じ名前の購読と階層のカテゴリの ID。 |
| `startDate` | 日付 | 購読開始日。日付形式（時刻なし）は、[RFC 3339，セクション5.6](https://tools.ietf.org/html/rfc3339#section-5.6)標準に従う必要があります。 |
| `status` | 文字列 | 購読の現在のステータス。 |
| `subCategory` | 文字列 | 購読の特定の下位分類。 |
| `term` | 整数 | 購読用語の数値。 |
| `termUnitOfTime` | 文字列 | 期間の時間の単位。 |
| `topUp` | 文字列 | 請求期間中に購読の消耗品を再購入する方法について、同意された用語を説明します。 |
| `type` | 文字列 | 購読が対象とする人数に関するエンタイトルメントの範囲。 |

データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/industry-verticals/subscription.schema.json)

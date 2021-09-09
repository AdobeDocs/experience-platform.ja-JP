---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；予約の詳細；
title: 予約詳細スキーマフィールドグループ
description: このドキュメントでは、「 Reservation Details 」スキーマフィールドグループの概要を説明します。
source-git-commit: 295dc040f3af7342226e3d78d0ae21e73db58d57
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 6%

---


# [!UICONTROL 予約詳細スキ] ーマフィールドグループ

[!UICONTROL 予約] 詳細は、長さ、変更、返金可能なステータス、部屋数など、予約に関する情報を取り込むために使用される、クラスの標準スキーマフィールドグループで [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) す。

フィールドグループには、1つのオブジェクトタイプフィールド`reservations`が用意されています。 このオブジェクトに含まれるプロパティについて、以下で説明します。

![予約詳細構造](../../images/field-groups/reservation-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `nonRefundableAmount` | [通貨](../../data-types/currency.md) | 返金不能とマークされた予約価格の金額。 |
| `transaction` | [トランザクション](../../data-types/transaction.md) | 引当の通貨トランザクションを説明します。 |
| `id` | 文字列 | 予約の一意の識別子。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた場合に取り込まれます。 |
| `confirmationNumber` | 文字列 | 予約の確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されたときに取り込まれます。 |
| `currencyCode` | 文字列 | 購入に使用されるISO 4217通貨コード。 |
| `endDate` | DateTime | 予約の終了ドロップオフ、返却日またはチェックアウト日。 |
| `length` | 整数 | 予約の合計日数。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された日時。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `purpose` | 文字列 | 予約の目的（通常はビジネスまたは個人）。 |
| `startDate` | DateTime | 予約の開始ピックアップ日、アウトバウンド日またはチェックイン日。 |
| `triptype` | 文字列 | 予約が片道、往復、または複数の都市旅行のどちらかを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.schema.json)

## 業界固有の予約フィールドグループ

業界固有の使用例のために、 [!UICONTROL 予約の詳細]スキーマを拡張するその他の標準フィールドグループがいくつかあります。 詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 食事予約]](./dining-reservation.md)
* [[!UICONTROL 飛行予約]](./flight-reservation.md)
* [[!UICONTROL 宿泊予約]](./lodging-reservation.md)
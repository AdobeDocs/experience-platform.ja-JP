---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；予約の詳細；
title: 予約詳細スキーマフィールドグループ
description: このドキュメントでは、「 Reservation Details 」スキーマフィールドグループの概要を説明します。
exl-id: 06f9ee37-9879-4db2-af68-9336366f7521
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 8%

---

# [!UICONTROL 予約の詳細] スキーマフィールドグループ

[!UICONTROL 予約の詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) 予約に関する情報（長さ、変更、払い戻し可能なステータス、部屋数など）をキャプチャするために使用します。

フィールドグループには、単一のオブジェクトタイプのフィールドが用意されています。 `reservations`. このオブジェクトに含まれるプロパティについて、以下で説明します。

![予約の詳細構造](../../images/field-groups/reservation-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `nonRefundableAmount` | [通貨](../../data-types/currency.md) | 払い戻し不可とマークされた予約価格の金額。 |
| `transaction` | [トランザクション](../../data-types/transaction.md) | 予約の通貨トランザクションを記述します。 |
| `id` | 文字列 | 予約の一意の ID。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされるとキャプチャされます。 |
| `confirmationNumber` | 文字列 | 予約の確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `endDate` | 日時 | 予約の最終降車日、返却日、またはチェックアウト日。 |
| `length` | 整数 | 予約の合計日数。 |
| `modification` | 整数 | この値は、予約が変更されるとキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられた大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `startDate` | 日時 | 予約の受け取り開始日、アウトバウンド日、またはチェックイン日。 |
| `triptype` | 文字列 | 予約が片道の旅行、往復、または複数都市の旅行のどちらであるかを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.schema.json)

## 業界固有の予約フィールドグループ

他にも、 [!UICONTROL 予約の詳細] 業界固有の使用例のスキーマ。 詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 食事予約]](./dining-reservation.md)
* [[!UICONTROL フライト予約]](./flight-reservation.md)
* [[!UICONTROL 宿泊予約]](./lodging-reservation.md)

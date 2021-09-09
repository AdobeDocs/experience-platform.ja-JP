---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；ダイニング；
title: ダイニング予約スキーマフィールドグループ
description: このドキュメントでは、「ダイニング予約スキーマ」フィールドグループの概要を説明します。
source-git-commit: d230cfa9e74eb96aa44e8b83ca8f2306db4ba4ec
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 6%

---


# [!UICONTROL Dining ] Reservationschemaフィールドグループ

[!UICONTROL ダイニン] グ予約は、ダイニング予約に関する情報を取り込むた [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) めの、クラスの標準スキーマフィールドグループです。

フィールドグループは、[!UICONTROL Reservation Details]フィールドグループの拡張で、1つのオブジェクトタイプフィールド`reservations`の下に同じフィールドがすべて含まれます。 これらの一般的なフィールドに加えて、[!UICONTROL ダイニング予約]には`diningReservations`配列も含まれます。 このオブジェクトの配列は、レストラン固有のプロパティを持つ1つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`diningReservations`配列の詳細について説明します。 `reservations`オブジェクトの下に指定されるその他のフィールドについて詳しくは、[[!UICONTROL 予約の詳細]フィールドグループのリファレンス](./reservation-details.md)を参照してください。

![食堂の予約構造](../../images/field-groups/dining-reservation/structure.png)

## `diningReservations`

`diningReservations` は、食事予約のリストを表すオブジェクトの配列です。予約イベントが、1日の異なる時間に複数の異なるレストランで予約を行う場合、例えば、1つのイベントに対して`diningReservations`の下に個々のオブジェクトとしてリストすることができます。

`diningReservations`の下に提供される各オブジェクトの構造を以下に示します。

![diningReservations構造](../../images/field-groups/dining-reservation/diningReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | 予約番号または識別子。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた場合に取り込まれます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されたときに取り込まれます。 |
| `cuisine` | 整数 | レストラン料理のタイプ。 |
| `currencyCode` | 文字列 | 購入に使用されるISO 4217通貨コード。 |
| `deliveryPartners` | 文字列 | レストランから提供される配信パートナー。 |
| `diningOptions` | 文字列 | レストランで提供される配送およびダイニングのオプション。 |
| `groupReservation` | Boolean | グループの予約がおこなわれているかどうかを示します。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約にリストされたゲストのロイヤルティプログラムID。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された日時。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `numberOfRooms` | 整数 | 予約に関連付けられている部屋の数。 |
| `partySize` | 整数 | 食堂の個人数。 |
| `priceCategory` | 文字列 | 予約の価格カテゴリ。 |
| `purpose` | 文字列 | 予約の目的（通常はビジネスまたは個人）。 |
| `reservationTime` | DateTime | 食事の予約が予約されている時間。 |
| `restaurantID` | 文字列 | レストランまたはダイニングの場所の識別子。 |
| `reservationStatus` | 文字列 | 予約のステータス。 |
| `specialOccasion` | Boolean | 特別な日に予約されているかどうかを示します。 |
| `status` | 整数 | 食事の予約状況。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.schema.json)

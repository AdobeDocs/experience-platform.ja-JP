---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；ダイニング；
title: ダイニング予約スキーマフィールドグループ
description: このドキュメントでは、「ダイニング予約スキーマ」フィールドグループの概要を説明します。
exl-id: 672b7a77-c433-4502-a1ad-a17c811b253e
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 6%

---

# [!UICONTROL Dining Reservationschema フ] ィールドグループ

[!UICONTROL 食事予] 約は、食事の予約に関する情報を取り込むた [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) めに使用される、クラスの標準スキーマフィールドグループです。

フィールドグループは、[!UICONTROL Reservation Details] フィールドグループの拡張で、1 つのオブジェクトタイプフィールド `reservations` の下に同じフィールドがすべて含まれます。 これらの汎用フィールドに加えて、[!UICONTROL Dining Reservation] には `diningReservations` 配列も含まれます。 このオブジェクトの配列は、1 つ以上の予約をレストラン固有のプロパティで記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`diningReservations` 配列の詳細について説明します。 `reservations` オブジェクトの下で提供されるその他のフィールドについては、[[!UICONTROL  予約の詳細 ] フィールドグループのリファレンス ](./reservation-details.md) を参照してください。

![食堂の予約構造](../../images/field-groups/dining-reservation/structure.png)

## `diningReservations`

`diningReservations` は、食事の予約のリストを表すオブジェクトの配列です。予約イベントが、1 日の異なる時間に複数の異なるレストランで予約を行う場合、例えば、1 回のイベントに対して `diningReservations` の下に個々のオブジェクトとして表示できます。

`diningReservations` の下に提供される各オブジェクトの構造を以下に示します。

![diningReservations 構造](../../images/field-groups/dining-reservation/diningReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | 予約番号または識別子。 |
| `cancellation` | 整数 | この値は、予約が取り消されたときに取り込まれます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されたときに取り込まれます。 |
| `cuisine` | 整数 | レストラン料理のタイプ。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `deliveryPartners` | 文字列 | レストランから提供される配信パートナー。 |
| `diningOptions` | 文字列 | レストランから提供される配送およびダイニングオプション。 |
| `groupReservation` | Boolean | グループの予約がおこなわれているかどうかを示します。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に登録されているゲストのロイヤリティプログラム ID。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された時刻。 |
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

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.schema.json)

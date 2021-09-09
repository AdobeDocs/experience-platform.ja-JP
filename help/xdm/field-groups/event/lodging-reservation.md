---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；下宿；
title: 宿泊予約スキーマフィールドグループ
description: このドキュメントでは、「宿泊予約スキーマ」フィールドグループの概要を説明します。
source-git-commit: d230cfa9e74eb96aa44e8b83ca8f2306db4ba4ec
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 6%

---


# [!UICONTROL Lodging ] Reservationschemaフィールドグループ

[!UICONTROL 宿泊予約] は、宿泊予約に関する情報を取り込むための [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) クラスの標準スキーマフィールドグループです。

フィールドグループは、[!UICONTROL Reservation Details]フィールドグループの拡張で、1つのオブジェクトタイプフィールド`reservations`の下に同じフィールドがすべて含まれます。 これらの一般的なフィールドに加えて、[!UICONTROL 宿泊予約]には`lodgingReservations`配列も含まれます。 このオブジェクトの配列は、1つ以上の宿泊施設を表すために使用され、宿泊施設に固有のプロパティを持ちます。

>[!NOTE]
>
>このドキュメントでは、`lodgingReservations`配列の詳細について説明します。 `reservations`オブジェクトの下に指定されるその他のフィールドについて詳しくは、[[!UICONTROL 予約の詳細]フィールドグループのリファレンス](./reservation-details.md)を参照してください。

![宿泊施設](../../images/field-groups/lodging-reservation/structure.png)

## `lodgingReservations`

`lodgingReservations` は、宿泊予約のリストを表すオブジェクトの配列です。例えば、旅行のルートに沿って複数の異なるホテルで予約を行う場合、1回のイベントに対して`lodgingReservations`の下に個々のオブジェクトとしてリストすることができます。

`lodgingReservations`の下に提供される各オブジェクトの構造を以下に示します。

![lodgingReservations構造](../../images/field-groups/lodging-reservation/lodgingReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `averageDailyPrice` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ホテルの部屋の平均日額。 |
| `lodgingCheckIn` | オブジェクト | 宿泊のチェックインの詳細を表すオブジェクト。 次の値が含まれます。<ul><li>`digitalKey`:（整数）チェックイン時にゲストがデジタルキーの使用を選択するタイミングを示します。</li><li>`earlyCheckInRequested`:（整数）ゲストが通常のチェックイン時間より早くチェックインを要求した場合を示します。</li><li>`lateCheckInRequested`:（整数）ゲストが通常のチェックイン時間より後にチェックインを要求した場合を示します。</li><li>`noRoomCheckIn`:（整数）この値は、その時点で利用可能な部屋がない場合にゲストがチェックインを終了したときに取り込まれます。</li><li>`oneRoomCheckIn`:（整数）この値は、その時点で使用可能なルームが1つしかない場合に、ゲストがチェックインを完了したときに取り込まれます。</li><li>`roomKeys`:（整数）チェックイン時に提供される標準部屋キーの数。</li><li>`userSelectedRoom`:（ブール値）チェックイン時にゲストが自分の部屋を選択したかどうかを示します。</li></ul> |
| `rackrate` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 予約前の予約なしの同日予約の費用。 |
| `ID` | 文字列 | 予約番号または識別子。 |
| `agentID` | 文字列 | ホテルの予約に関連付けられているエージェントID。 |
| `basePrice` | 文字列 | 割引が追加される前の基本価格。 |
| `bookingID` | 文字列 | ホテルの予約に関連付けられている予約ID。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた場合に取り込まれます。 |
| `checkInDate` | DateTime | 部屋予約のチェックイン日。 |
| `checkOutDate` | DateTime | 部屋の予約のチェックアウト日。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `couponCode` | 文字列 | ホテルの予約に関連付けられたクーポンコード。 |
| `created` | 整数 | この値は、予約が作成されたときに取り込まれます。 |
| `currencyCode` | 文字列 | 購入に使用されるISO 4217通貨コード。 |
| `discountPercent` | Double | 予約に関連付けられた割引率。 |
| `freeCancelation` | Boolean | 部屋に無料キャンセルポリシーがあるかどうかを示します。 |
| `guestID` | 文字列 | ホテルの予約に関連付けられているゲストID。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約にリストされたゲストのロイヤルティプログラムID。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された日時。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `numberOfRooms` | 整数 | 予約に関連付けられている部屋の数。 |
| `propertyID` | 文字列 | 予約のホテルまたはリゾートの識別子。 |
| `propertyName` | 文字列 | 予約のホテルまたはリゾートの名前。 |
| `purpose` | 文字列 | 予約の目的（通常はビジネスまたは個人）。 |
| `ratePlan` | 文字列 | 部屋が売られたレート取引。 |
| `refundable` | Boolean | 部屋が払い戻し可能かどうかを示します。 |
| `reservationStatus` | 文字列 | 予約のステータス。 |
| `roomAccessibilityType` | 文字列 | モビリティ、聴覚、その他など、部屋のアクセシビリティタイプ。 |
| `roomCapacity` | 整数 | ホテルの部屋に泊まる人の数。 |
| `roomType` | 文字列 | 予約されている部屋のタイプ。 |
| `smoking` | Boolean | 部屋が喫煙を許可するかどうかを示します。 |
| `tripType` | 文字列 | 予約が片道、往復、または複数の都市旅行のどちらかを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json)

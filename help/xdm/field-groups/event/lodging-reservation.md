---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；宿泊；
title: 宿泊予約スキーマフィールドグループ
description: このドキュメントでは、「宿泊予約スキーマ」フィールドグループの概要を説明します。
exl-id: f0eafc83-21f1-483d-9397-1133e3777699
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 6%

---

# [!UICONTROL Lodging ] Reservationschema フィールドグループ

[!UICONTROL 宿泊予] 約とは、宿泊予約に関する情報を取り込むた [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) めの、クラスの標準スキーマフィールドグループです。

フィールドグループは、[!UICONTROL Reservation Details] フィールドグループの拡張で、1 つのオブジェクトタイプフィールド `reservations` の下に同じフィールドがすべて含まれます。 これらの汎用フィールドに加えて、[!UICONTROL  宿泊予約 ] には `lodgingReservations` 配列も含まれます。 この一連のオブジェクトは、宿泊施設に固有のプロパティを持つ 1 つ以上の予約を記述するのに使用されます。

>[!NOTE]
>
>このドキュメントでは、`lodgingReservations` 配列の詳細について説明します。 `reservations` オブジェクトの下で提供されるその他のフィールドについては、[[!UICONTROL  予約の詳細 ] フィールドグループのリファレンス ](./reservation-details.md) を参照してください。

![宿泊施設](../../images/field-groups/lodging-reservation/structure.png)

## `lodgingReservations`

`lodgingReservations` は、宿泊予約のリストを表すオブジェクトの配列です。予約イベントが旅行のルートに沿って複数の異なるホテルで予約を行う場合、例えば、1 回のイベントの `lodgingReservations` の下に個々のオブジェクトとして登録できます。

`lodgingReservations` の下に提供される各オブジェクトの構造を以下に示します。

![lodgingReservations 構造](../../images/field-groups/lodging-reservation/lodgingReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `averageDailyPrice` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ホテルの部屋の平均日額。 |
| `lodgingCheckIn` | オブジェクト | 宿泊のチェックインの詳細を表すオブジェクト。 次の値が含まれます。<ul><li>`digitalKey`:（整数）チェックイン時にゲストがデジタルキーの使用を選択するタイミングを示します。</li><li>`earlyCheckInRequested`:（整数）ゲストが通常のチェックイン時間より早いチェックインをリクエストした日時を示します。</li><li>`lateCheckInRequested`:（整数）ゲストが通常のチェックイン時間より後にチェックインをリクエストした日時を示します。</li><li>`noRoomCheckIn`:（整数）この値は、その時点で利用可能な客室がない場合にゲストがチェックインを終了したときに取り込まれます。</li><li>`oneRoomCheckIn`:（整数）この値は、その時点で利用可能な部屋が 1 つしかない場合に、ゲストがチェックインを終了したときに取り込まれます。</li><li>`roomKeys`:（整数）チェックイン時に提供される標準部屋キーの数。</li><li>`userSelectedRoom`:(Boolean) チェックイン時にゲストが自分の部屋を選択したかどうかを示します。</li></ul> |
| `rackrate` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 予約前の予約なしの同日予約のコスト。 |
| `ID` | 文字列 | 予約番号または識別子。 |
| `agentID` | 文字列 | ホテルの予約に関連付けられているエージェント ID。 |
| `basePrice` | 文字列 | 割引が追加される前の基準価格。 |
| `bookingID` | 文字列 | ホテルの予約に関連付けられている予約 ID。 |
| `cancellation` | 整数 | この値は、予約が取り消されたときに取り込まれます。 |
| `checkInDate` | DateTime | 部屋予約のチェックイン日。 |
| `checkOutDate` | DateTime | 部屋の予約のチェックアウト日。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `couponCode` | 文字列 | ホテルの予約に関連付けられたクーポンコード。 |
| `created` | 整数 | この値は、予約が作成されたときに取り込まれます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `discountPercent` | Double | 予約に関連付けられている割引率。 |
| `freeCancelation` | Boolean | 部屋が無料のキャンセルポリシーを持っているかどうかを示します。 |
| `guestID` | 文字列 | ホテルの予約に関連付けられているゲスト ID。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に登録されているゲストのロイヤリティプログラム ID。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された時刻。 |
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
| `roomCapacity` | 整数 | ホテルの部屋に入る人の数。 |
| `roomType` | 文字列 | 予約されている部屋のタイプ。 |
| `smoking` | Boolean | 部屋が喫煙を許可しているかどうかを示します。 |
| `tripType` | 文字列 | 予約が片道、往復、または複数の都市旅行のどちらであるかを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json)

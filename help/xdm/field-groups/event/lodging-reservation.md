---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；宿泊；
title: 宿泊予約スキーマフィールドグループ
description: このドキュメントでは、「宿泊施設の予約スキーマ」フィールドグループの概要を説明します。
exl-id: f0eafc83-21f1-483d-9397-1133e3777699
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 7%

---

# [!UICONTROL 宿泊施設の予約] スキーマフィールドグループ

[!UICONTROL 宿泊施設の予約] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) 宿泊施設の予約に関する情報をキャプチャするために使用されます。

フィールドグループは、 [!UICONTROL 予約の詳細] フィールドグループを作成し、1 つのオブジェクトタイプフィールドの下に同じフィールドをすべて含める `reservations`. これらの汎用フィールドに加えて、 [!UICONTROL 宿泊施設の予約] 次を含む `lodgingReservations` 配列。 このオブジェクトの配列は、宿泊施設に固有のプロパティを持つ 1 つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、 `lodgingReservations` 配列。 の下に表示されるその他のフィールドに関する情報 `reservations` オブジェクトの場合は、 [[!UICONTROL 予約の詳細] フィールドグループ参照](./reservation-details.md).

![宿泊施設の予約構造](../../images/field-groups/lodging-reservation/structure.png)

## `lodgingReservations`

`lodgingReservations` は、宿泊予約のリストを表すオブジェクトの配列です。 予約イベントが旅行のルートに沿って複数の異なるホテルでの予約を含む場合、例えば、これらの予約は個々のオブジェクトとして以下にリスト表示できます。 `lodgingReservations` 1 つのイベント用。

の下に提供される各オブジェクトの構造 `lodgingReservations` は以下に示します。

![lodgingReservations 構造](../../images/field-groups/lodging-reservation/lodgingReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `averageDailyPrice` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ホテルの部屋の 1 日あたりの平均価格。 |
| `lodgingCheckIn` | オブジェクト | 宿泊施設のチェックインの詳細を表すオブジェクト。 次の値が含まれます。<ul><li>`digitalKey`:（整数）ゲストがチェックイン時にデジタルキーの使用を選択するタイミングを示します。</li><li>`earlyCheckInRequested`:（整数）ゲストが通常のチェックイン時間より前にチェックインをリクエストするタイミングを示します。</li><li>`lateCheckInRequested`:（整数）ゲストが通常のチェックイン時間より後にチェックインをリクエストするタイミングを示します。</li><li>`noRoomCheckIn`:（整数）この値は、ゲストがチェックインを完了し、その時点で利用可能な部屋がない場合にキャプチャされます。</li><li>`oneRoomCheckIn`:（整数）この値は、その時点で利用可能なルームが 1 つしかない場合に、ゲストがチェックインを完了するとキャプチャされます。</li><li>`roomKeys`:（整数）チェックイン時に提供される標準ルームキーの数。</li><li>`userSelectedRoom`:（ブール値）チェックイン時にゲストが自分の部屋を選択したかどうかを示します。</li></ul> |
| `rackrate` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 事前の予約手配なしの当日予約のコスト。 |
| `ID` | 文字列 | 予約番号または識別子。 |
| `agentID` | 文字列 | ホテル予約に関連付けられたエージェント ID。 |
| `basePrice` | 文字列 | 割引が追加される前の基本価格。 |
| `bookingID` | 文字列 | ホテル予約に関連付けられた予約 ID。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされるとキャプチャされます。 |
| `checkInDate` | 日時 | 部屋の予約のチェックイン日。 |
| `checkOutDate` | 日時 | 部屋の予約のチェックアウト日。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `couponCode` | 文字列 | ホテル予約に関連付けられたクーポンコード。 |
| `created` | 整数 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `discountPercent` | Double | 予約に関連付けられた割引率。 |
| `freeCancelation` | ブール値 | 部屋に無料のキャンセルポリシーがあるかどうかを示します。 |
| `guestID` | 文字列 | ホテル予約に関連付けられたゲスト ID。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載されているゲストのロイヤリティプログラム ID。 |
| `modification` | 整数 | この値は、予約が変更されるとキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられた大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `numberOfRooms` | 整数 | 予約に関連付けられている部屋の数。 |
| `propertyID` | 文字列 | 予約するホテルまたはリゾートの識別子。 |
| `propertyName` | 文字列 | 予約するホテルまたはリゾートの名前。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `ratePlan` | 文字列 | 部屋が販売されたレート取引。 |
| `refundable` | ブール値 | 部屋が払い戻し可能かどうかを示します。 |
| `reservationStatus` | 文字列 | 予約のステータス。 |
| `roomAccessibilityType` | 文字列 | 部屋のアクセシビリティタイプ（モビリティ、聴覚、その他）。 |
| `roomCapacity` | 整数 | ホテルの部屋が保有する人数。 |
| `roomType` | 文字列 | 予約されている部屋のタイプ。 |
| `smoking` | ブール値 | 部屋が喫煙を許可するかどうかを示します。 |
| `tripType` | 文字列 | 予約が片道の旅行、往復、または複数都市の旅行のどちらであるかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json)

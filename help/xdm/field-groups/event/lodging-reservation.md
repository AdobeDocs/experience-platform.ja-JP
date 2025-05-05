---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；予約；宿泊施設；
title: 宿泊施設の予約スキーマフィールドグループ
description: 宿泊予約スキーマフィールドグループについて説明します。
exl-id: f0eafc83-21f1-483d-9397-1133e3777699
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 7%

---

# [!UICONTROL &#x200B; 宿泊予約 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 宿泊予約 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、宿泊予約に関する情報を取得するために使用されます。

フィールドグループは、[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループの拡張機能で、単一のオブジェクトタイプのフィールド `reservations` の下に同じフィールドをすべて含んでいます。 これらの一般的なフィールドに加えて、[!UICONTROL &#x200B; 宿泊予約 &#x200B;] には配列も含まれ `lodgingReservations` います。 このオブジェクトの配列は、宿泊施設に固有のプロパティを持つ 1 つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`lodgingReservations` 配列の詳細を説明します。 `reservations` オブジェクトの下に提供されるその他のフィールドについて詳しくは、[[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループのリファレンス ](./reservation-details.md) を参照してください。

![ 宿泊予約構造 ](../../images/field-groups/lodging-reservation/structure.png)

## `lodgingReservations`

`lodgingReservations` は、宿泊施設の予約のリストを表すオブジェクトの配列です。 例えば、旅行のルートに沿った複数の異なるホテルでの予約が予約イベントに含まれる場合、これらの予約は、1 つのイベントの `lodgingReservations` の下に個々のオブジェクトとしてリストできます。

`lodgingReservations` の下に提供される各オブジェクトの構造を以下に示します。

![lopingReservations 構造 ](../../images/field-groups/lodging-reservation/lodgingReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `averageDailyPrice` | [[!UICONTROL 通貨]](../../data-types/currency.md) | ホテルの部屋の 1 日あたりの平均価格。 |
| `lodgingCheckIn` | オブジェクト | 宿泊施設のチェックインの詳細を説明するオブジェクト。 次の値が含まれます。<ul><li>`digitalKey`: （整数）ゲストがチェックイン時にデジタルキーの使用を選択したタイミングを示します。</li><li>`earlyCheckInRequested`: （整数）ゲストが通常のチェックイン時間よりも早くチェックインをリクエストするタイミングを示します。</li><li>`lateCheckInRequested`: （整数）ゲストが通常のチェックイン時間よりも遅くチェックインをリクエストするタイミングを示します。</li><li>`noRoomCheckIn`: （整数）この値は、ゲストがその時点で利用可能な部屋がない場合にチェックインを完了したときにキャプチャされます。</li><li>`oneRoomCheckIn`: （整数）この値は、ゲストがその時点で利用できる部屋が 1 つのみの場合にチェックインを完了するとキャプチャされます。</li><li>`roomKeys`: （整数）チェックイン時に提供される標準ルームキーの数。</li><li>`userSelectedRoom`: （ブール値）ゲストがチェックイン時に部屋を選択したかどうかを示します。</li></ul> |
| `rackrate` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 事前の予約手配のない当日予約のコスト。 |
| `ID` | 文字列 | 予約番号または識別子。 |
| `agentID` | 文字列 | ホテル予約に関連付けられたエージェント ID。 |
| `basePrice` | 文字列 | 割引が加えられる前の基本価格。 |
| `bookingID` | 文字列 | ホテル予約に関連付けられた予約 ID。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた際にキャプチャされます。 |
| `checkInDate` | 日時 | 部屋の予約のチェックイン日。 |
| `checkOutDate` | 日時 | 予約した部屋のチェックアウト日。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `couponCode` | 文字列 | ホテル予約に関連付けられたクーポンコード。 |
| `created` | 整数 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `discountPercent` | Double | 予約に関連付けられている割引率。 |
| `freeCancelation` | ブール値 | 部屋に無料のキャンセルポリシーがあるかどうかを示します。 |
| `guestID` | 文字列 | ホテル予約に関連付けられたゲスト ID。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載されているゲストのロイヤルティプログラム ID。 |
| `modification` | 整数 | この値は、予約が変更された際にキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子供の数。 |
| `numberOfRooms` | 整数 | 予約に関連付けられている部屋の数。 |
| `propertyID` | 文字列 | 予約のホテルまたはリゾートの識別子。 |
| `propertyName` | 文字列 | 予約するホテルまたはリゾートの名前。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `ratePlan` | 文字列 | 部屋が販売された際の料金プラン。 |
| `refundable` | ブール値 | 部屋が払い戻し可能かどうかを示します。 |
| `reservationStatus` | 文字列 | 予約のステータス。 |
| `roomAccessibilityType` | 文字列 | 部屋のアクセシビリティタイプ （動きやすさ、聴覚、その他）。 |
| `roomCapacity` | 整数 | ホテルの部屋の収容人数。 |
| `roomType` | 文字列 | 予約されている部屋のタイプ。 |
| `smoking` | ブール値 | 部屋での喫煙が許可されているかどうかを示します。 |
| `tripType` | 文字列 | 予約が片道トリップ、往復または複数都市トリップのいずれであるかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-lodging-reservation.schema.json)

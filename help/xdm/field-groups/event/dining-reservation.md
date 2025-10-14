---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；予約；食事；
title: 食事の予約スキーマフィールドグループ
description: 食事の予約スキーマフィールドグループについて説明します。
exl-id: 672b7a77-c433-4502-a1ad-a17c811b253e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 13%

---

# [!UICONTROL &#x200B; 食事予約 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 食事の予約 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループで、食事の予約に関する情報を取り込むために使用されます。

フィールドグループは、[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループの拡張機能で、単一のオブジェクトタイプのフィールド `reservations` の下に同じフィールドをすべて含んでいます。 これらの汎用フィールドに加えて、[!UICONTROL &#x200B; 食事予約 &#x200B;] には配列も含まれ `diningReservations` います。 このオブジェクトの配列は、レストラン固有のプロパティを持つ 1 つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`diningReservations` 配列の詳細を説明します。 `reservations` オブジェクトの下に提供されるその他のフィールドについて詳しくは、[[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループのリファレンス &#x200B;](./reservation-details.md) を参照してください。

![&#x200B; 食事の予約構造 &#x200B;](../../images/field-groups/dining-reservation/structure.png)

## `diningReservations`

`diningReservations` は、食事予約のリストを表すオブジェクトの配列です。 例えば、1 日の異なる時間に複数の異なるレストランで予約する予約イベントの場合、それらの予約は、1 つのイベントの `diningReservations` の下に個々のオブジェクトとしてリストできます。

`diningReservations` の下に提供される各オブジェクトの構造を以下に示します。

![diningReservations 構造 &#x200B;](../../images/field-groups/dining-reservation/diningReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ID` | 文字列 | 予約番号または識別子。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた際にキャプチャされます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されるとキャプチャされます。 |
| `cuisine` | 整数 | レストランの料理のタイプ。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `deliveryPartners` | 文字列 | レストランで利用可能なデリバリーパートナー。 |
| `diningOptions` | 文字列 | レストランで利用可能なデリバリーおよび食事オプション。 |
| `groupReservation` | ブール値 | 予約がグループ向けに行われているかどうかを示します。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載されているゲストのロイヤルティプログラム ID。 |
| `modification` | 整数 | この値は、予約が変更された際にキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子供の数。 |
| `numberOfRooms` | 整数 | 予約に関連付けられている部屋の数。 |
| `partySize` | 整数 | ダイニングパーティーに参加する人数。 |
| `priceCategory` | 文字列 | 予約時の価格帯。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `reservationTime` | 日時 | 食事の予約が入っている時間。 |
| `restaurantID` | 文字列 | レストランまたは食事の場所の識別子。 |
| `reservationStatus` | 文字列 | 予約のステータス。 |
| `specialOccasion` | ブール値 | 予約が特別な日のためのものかどうかを示します。 |
| `status` | 整数 | 食事の予約のステータス。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-dining-reservation.schema.json)

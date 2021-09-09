---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；フライト；
title: フライト予約スキーマフィールドグループ
description: このドキュメントでは、「フライト予約スキーマ」フィールドグループの概要を説明します。
source-git-commit: 295dc040f3af7342226e3d78d0ae21e73db58d57
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 5%

---


# [!UICONTROL Flight Reservationschemaフ] ィールドグループ

[!UICONTROL フライ] ト予約は、フライト予約に関する情報を取得するた [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md) めに使用される、クラスの標準スキーマフィールドグループです。

フィールドグループは、[!UICONTROL Reservation Details]フィールドグループの拡張で、1つのオブジェクトタイプフィールド`reservations`の下に同じフィールドがすべて含まれます。 これらの汎用フィールドに加えて、[!UICONTROL フライトの予約]には`flightReservations`配列も含まれます。 このオブジェクトの配列は、航空便に固有のプロパティを持つ1つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`flightReservations`配列の詳細について説明します。 `reservations`オブジェクトの下に指定されるその他のフィールドについて詳しくは、[[!UICONTROL 予約の詳細]フィールドグループのリファレンス](./reservation-details.md)を参照してください。

![飛行予約構造](../../images/field-groups/flight-reservation/structure.png)

## `flightReservations`

`flightReservations` は、フライト予約のリストを表すオブジェクトの配列です。例えば、1回の旅行で複数の便をつなぐ予約が予約イベントに含まれる場合、1回のイベントで`flightReservations`の下に個々のオブジェクトとしてリストすることができます。

`flightReservations`の下に提供される各オブジェクトの構造を以下に示します。

![flightReservations構造](../../images/field-groups/flight-reservation/flightReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `flightCheckIn` | オブジェクト | フライトのチェックインに関する詳細をキャプチャします。 オブジェクトには次のプロパティが含まれます。<ul><li>`arrivalAirportCode`:（文字列）到着都市の空港コード。</li><li>`boardingGroup`:（文字列）搭乗注文の航空会社固有の指標。</li><li>`checkInMethod`:（文字列）カウンター、オンライン、キオスク、セルフサービスなど、チェックインに使用したメソッド。</li><li>`checkedBags`:（整数）フライトにチェックされたバッグの数。</li><li>`checkedPassengers`:（整数）同じ予約番号に複数の乗客が存在する場合は、その便にチェックインした乗客の数。</li><li>`confirmationNumber`:（文字列）予約確認番号または識別子。</li><li>`departureAirportCode`:（文字列）出発都市の空港コード。</li><li>`flightNumber`:（文字列）予約するフライトのフライト番号。</li></ul> |
| `flightStatusSearch` | オブジェクト | フライトのステータスが検索されたときに返される詳細を取り込みます。 オブジェクトには次のプロパティが含まれます。<ul><li>`arrivalAirportCode`:（文字列）到着都市の空港コード。</li><li>`boardingGroup`:（文字列）搭乗注文の航空会社固有の指標。</li><li>`departureAirportCode`:（文字列）出発都市の空港コード。</li><li>`departureDate`:(DateTime)予約中のフライトの出発日。</li><li>`flightNumber`:（文字列）予約するフライトのフライト番号。</li><li>`searchCount`:（整数）予約済みフライトのステータスが検索された回数。</li></ul> |
| `agentID` | 文字列 | 該当する場合は、予約の予約を担当するエージェントまたはブッカー。 |
| `aircraftID` | 文字列 | 航空機の識別子。 |
| `aircraftType` | 文字列 | 航空機の型。 |
| `arrivalAirportCode` | 文字列 | 到着都市の空港コード。 |
| `arrivalDate` | DateTime | 予約されているフライトの到着日。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた場合に取り込まれます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 文字列 | この値は、予約が作成されたときに取り込まれます。 |
| `currencyCode` | 文字列 | 購入に使用されるISO 4217通貨コード。 |
| `departureAirportCode` | 文字列 | 出発都市の空港コード。 |
| `departureDate` | DateTime | 予約中の便の出発日。 |
| `fareClass` | 文字列 | 予約中の飛行機の料金区分。 |
| `flightNumber` | 文字列 | 予約するフライトのフライト番号。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載された乗客のロイヤルティまたは報酬プログラムID。 |
| `modification` | 整数 | この値は、予約が変更された場合に取り込まれます。 |
| `modificationDate` | DateTime | 予約が最後に変更された日時。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `passengerID` | 文字列 | 予約に関連付けられた旅客情報。 |
| `purpose` | 文字列 | 予約の目的（通常はビジネスまたは個人）。 |
| `salesChannel` | 文字列 | 予約が予約された販売チャネル。 |
| `securityScreening` | 文字列 | 乗客のセキュリティスクリーニングの種類は、対象となります。 |
| `status` | 文字列 | フライトの予約状態。 |
| `ticketNumber` | 文字列 | 予約番号または識別子。 |
| `tripType` | 文字列 | 予約が片道、往復、または複数の都市旅行のどちらかを示します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；予約；フライト；
title: フライト予約スキーマフィールドグループ
description: このドキュメントでは、「フライト予約スキーマ」フィールドグループの概要を説明します。
exl-id: df4bb525-c2d3-4e1d-921f-903142a570ac
source-git-commit: afbbdfff4346ab5240927f5703d3a06676776ea8
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 4%

---

# [!UICONTROL フライトの予約] スキーマフィールドグループ

[!UICONTROL フライトの予約] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) フライト予約に関する情報をキャプチャするために使用されます。

フィールドグループは、 [!UICONTROL 予約の詳細] フィールドグループを作成し、1 つのオブジェクトタイプフィールドの下に同じフィールドをすべて含める `reservations`. これらの汎用フィールドに加えて、 [!UICONTROL フライトの予約] 次を含む `flightReservations` 配列。 このオブジェクトの配列は、航空便に固有のプロパティを持つ 1 つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、 `flightReservations` 配列。 の下に表示されるその他のフィールドに関する情報 `reservations` オブジェクトの場合は、 [[!UICONTROL 予約の詳細] フィールドグループ参照](./reservation-details.md).

![フライト予約構造](../../images/field-groups/flight-reservation/structure.png)

## `flightReservations`

`flightReservations` は、フライト予約のリストを表すオブジェクトの配列です。 予約イベントが 1 回の旅行で複数の接続便の予約を含む場合、例えば、これらの予約は、個々のオブジェクトとして以下に表示されます。 `flightReservations` 1 つのイベント用。

の下に提供される各オブジェクトの構造 `flightReservations` は以下に示します。

![flightReservations 構造](../../images/field-groups/flight-reservation/flightReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `flightCheckIn` | オブジェクト | フライトチェックインに関する詳細をキャプチャします。 このオブジェクトには、次のプロパティが含まれます。<ul><li>`arrivalAirportCode`:(String) 到着都市の空港コード。</li><li>`boardingGroup`:(String) 搭乗注文の航空会社固有の指標。</li><li>`checkInMethod`:（文字列）カウンター、オンライン、キオスク、セルフサービスなど、チェックインで使用したメソッド。</li><li>`checkedBags`:（整数）フライトに対してチェックされたバッグの数。</li><li>`checkedPassengers`:（整数）同じ予約番号に複数の乗客が存在する場合の、フライトにチェックインした乗客の数。</li><li>`confirmationNumber`:(String) 予約確認番号または識別子。</li><li>`departureAirportCode`:（文字列）出発都市の空港コード。</li><li>`flightNumber`:（文字列）予約されているフライトの便名。</li></ul> |
| `flightStatusSearch` | オブジェクト | フライトのステータスが検索されたときに返される詳細をキャプチャします。 このオブジェクトには、次のプロパティが含まれます。<ul><li>`arrivalAirportCode`:(String) 到着都市の空港コード。</li><li>`boardingGroup`:(String) 搭乗注文の航空会社固有の指標。</li><li>`departureAirportCode`:（文字列）出発都市の空港コード。</li><li>`departureDate`:(DateTime) 予約されているフライトの出発日。</li><li>`flightNumber`:（文字列）予約されているフライトの便名。</li><li>`searchCount`:（整数）予約されたフライトのステータスが検索された回数。</li></ul> |
| `agentID` | 文字列 | 予約の予約を担当するエージェントまたはブッカー（該当する場合）。 |
| `aircraftID` | 文字列 | 航空機の識別子。 |
| `aircraftType` | 文字列 | 航空機の型。 |
| `arrivalAirportCode` | 文字列 | 到着都市の空港コード。 |
| `arrivalDate` | 日時 | 予約されているフライトの到着日。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされるとキャプチャされます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 文字列 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `departureAirportCode` | 文字列 | 出発都市の空港コード。 |
| `departureDate` | 日時 | 予約されているフライトの出発日。 |
| `fareClass` | 文字列 | 予約されているフライトの運賃クラス。 |
| `flightNumber` | 文字列 | 予約されているフライトの便名。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載されている乗客のロイヤルティまたは報酬プログラム ID。 |
| `modification` | 整数 | この値は、予約が変更されるとキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられた大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子の数。 |
| `passengerID` | 文字列 | 予約に関連付けられた乗客情報。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `salesChannel` | 文字列 | 予約が行われた販売チャネル。 |
| `securityScreening` | 文字列 | 乗客が受けるセキュリティスクリーニングの種類。 |
| `status` | 文字列 | フライト予約のステータス。 |
| `ticketNumber` | 文字列 | 予約番号または識別子。 |
| `tripType` | 文字列 | 予約が片道の旅行、往復、または複数都市の旅行のどちらであるかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.schema.json)

---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；予約；フライト；
title: フライト予約スキーマフィールドグループ
description: フライト予約スキーマフィールドグループについて説明します。
exl-id: df4bb525-c2d3-4e1d-921f-903142a570ac
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 10%

---

# [!UICONTROL &#x200B; フライト予約 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; フライト予約 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、フライト予約に関する情報を取得するために使用されます。

フィールドグループは、[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループの拡張機能で、単一のオブジェクトタイプのフィールド `reservations` の下に同じフィールドをすべて含んでいます。 これらの汎用フィールドに加えて、[!UICONTROL &#x200B; フライト予約 &#x200B;] には配列も含まれ `flightReservations` います。 このオブジェクトの配列は、航空便に固有のプロパティを持つ 1 つ以上の予約を記述するために使用されます。

>[!NOTE]
>
>このドキュメントでは、`flightReservations` 配列の詳細を説明します。 `reservations` オブジェクトの下に提供されるその他のフィールドについて詳しくは、[[!UICONTROL &#x200B; 予約詳細 &#x200B;] フィールドグループのリファレンス ](./reservation-details.md) を参照してください。

![ フライトの予約構造 ](../../images/field-groups/flight-reservation/structure.png)

## `flightReservations`

`flightReservations` は、フライト予約のリストを表すオブジェクトの配列です。 例えば、予約イベントに旅行中の複数の接続便の予約が含まれる場合、これらの予約は、単一のイベントの `flightReservations` の下に個々のオブジェクトとしてリストできます。

`flightReservations` の下に提供される各オブジェクトの構造を以下に示します。

![flightReservations 構造 ](../../images/field-groups/flight-reservation/flightReservations.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `flightCheckIn` | オブジェクト | フライトチェックインに関する詳細を取得します。 オブジェクトには次のプロパティが含まれます。<ul><li>`arrivalAirportCode`: （文字列）到着都市の空港コード。</li><li>`boardingGroup`: （文字列）搭乗順の航空会社固有のインジケーター。</li><li>`checkInMethod`:（文字列）カウンター、オンライン、キオスク、セルフサービスなど、チェックインに使用されるメソッド。</li><li>`checkedBags`: （整数）フライトで預けられた手荷物の数。</li><li>`checkedPassengers`: （整数）同じ予約番号で複数の乗客が存在する場合の、フライトにチェックインした乗客の数。</li><li>`confirmationNumber`: （文字列）予約確認番号または識別子。</li><li>`departureAirportCode`: （文字列）出発都市の空港コード。</li><li>`flightNumber`: （文字列）予約されているフライトの便名。</li></ul> |
| `flightStatusSearch` | オブジェクト | フライトのステータスが検索されたときに返される詳細をキャプチャします。 オブジェクトには次のプロパティが含まれます。<ul><li>`arrivalAirportCode`: （文字列）到着都市の空港コード。</li><li>`boardingGroup`: （文字列）搭乗順の航空会社固有のインジケーター。</li><li>`departureAirportCode`: （文字列）出発都市の空港コード。</li><li>`departureDate`: （日時）予約されているフライトの出発日。</li><li>`flightNumber`: （文字列）予約されているフライトの便名。</li><li>`searchCount`: （整数）予約済みフライトのステータスが検索された回数。</li></ul> |
| `agentID` | 文字列 | 予約を担当するエージェントまたは予約者（該当する場合）。 |
| `aircraftID` | 文字列 | 航空機の識別子。 |
| `aircraftType` | 文字列 | 航空機のタイプ。 |
| `arrivalAirportCode` | 文字列 | 到着都市の空港コード。 |
| `arrivalDate` | 日時 | 予約されているフライトの到着日。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた際にキャプチャされます。 |
| `confirmationNumber` | 文字列 | 予約確認番号または識別子。 |
| `created` | 文字列 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `departureAirportCode` | 文字列 | 出発都市の空港コード。 |
| `departureDate` | 日時 | 予約されているフライトの出発日。 |
| `fareClass` | 文字列 | 予約されているフライトの運賃クラス。 |
| `flightNumber` | 文字列 | 予約されているフライトの便名。 |
| `length` | 整数 | 予約の合計日数。 |
| `loyaltyID` | 文字列 | 予約に記載されている乗客のロイヤルティまたは報酬プログラム ID。 |
| `modification` | 整数 | この値は、予約が変更された際にキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子供の数。 |
| `passengerID` | 文字列 | 予約に関連付けられている乗客情報。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `salesChannel` | 文字列 | 予約が行われた販売チャネル。 |
| `securityScreening` | 文字列 | 乗客が受ける手荷物検査のタイプ。 |
| `status` | 文字列 | フライト予約のステータス。 |
| `ticketNumber` | 文字列 | 予約番号または識別子。 |
| `tripType` | 文字列 | 予約が片道トリップ、往復または複数都市トリップのいずれであるかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-flight-reservation.schema.json)

---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；予約；予約の詳細；
title: 予約詳細スキーマフィールドグループ
description: 「予約詳細」スキーマフィールドグループについて説明します。
exl-id: 06f9ee37-9879-4db2-af68-9336366f7521
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 8%

---

# [!UICONTROL &#x200B; 予約詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 予約詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、長さ、変更、払い戻し可能ステータス、部屋数など、予約に関する情報を取得するために使用されます。

フィールドグループは、単一のオブジェクトタイプのフィールド `reservations` を提供します。 このオブジェクトに含まれるプロパティについては、以下で説明します。

![ 予約詳細の構造 ](../../images/field-groups/reservation-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `nonRefundableAmount` | [通貨](../../data-types/currency.md) | 払い戻し不可とマークされている予約金額。 |
| `transaction` | [ トランザクション ](../../data-types/transaction.md) | 予約の通貨トランザクションを表します。 |
| `id` | 文字列 | 予約の一意の ID。 |
| `cancellation` | 整数 | この値は、予約がキャンセルされた際にキャプチャされます。 |
| `confirmationNumber` | 文字列 | 予約の確認番号または識別子。 |
| `created` | 整数 | この値は、予約が作成されるとキャプチャされます。 |
| `currencyCode` | 文字列 | 購入に使用される ISO 4217 通貨コード。 |
| `endDate` | 日時 | 予約の最終降車日、返却日またはチェックアウト日。 |
| `length` | 整数 | 予約の合計日数。 |
| `modification` | 整数 | この値は、予約が変更された際にキャプチャされます。 |
| `modificationDate` | 日時 | 予約が最後に変更された時刻。 |
| `numberOfAdults` | 整数 | 予約に関連付けられている大人の数。 |
| `numberOfChildren` | 整数 | 予約に関連付けられている子供の数。 |
| `purpose` | 文字列 | 予約の目的（通常、ビジネスまたは個人）。 |
| `startDate` | 日時 | 予約の積み込み開始、出発、またはチェックイン日。 |
| `triptype` | 文字列 | 予約が片道トリップ、往復または複数都市トリップのいずれであるかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-reservation-details.schema.json)

## 業界固有の予約フィールドグループ

業界固有のユースケース用に [!UICONTROL &#x200B; 予約詳細 &#x200B;] スキーマを拡張した他の標準フィールドグループがいくつかあります。 詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 食事予約]](./dining-reservation.md)
* [[!UICONTROL フライト予約]](./flight-reservation.md)
* [[!UICONTROL 宿泊予約]](./lodging-reservation.md)

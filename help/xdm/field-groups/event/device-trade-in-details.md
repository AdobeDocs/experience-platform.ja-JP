---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；デバイス；取引；取引；取引；
solution: Experience Platform
title: デバイス下取り詳細スキーマフィールドグループ
description: デバイス下取り詳細スキーマフィールドグループについて説明します。
exl-id: 744557be-0297-453f-9134-9d0f4ef2df4d
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 18%

---

# [!UICONTROL  デバイス下取り詳細 ] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL  デバイス下取り詳細 ] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 下取り価格、元のデバイス ID、新しいデバイス ID など、デバイスの下取りトランザクションを記述する単一フィールド（`deviceTradeInDetails`）が提供されます。

![ デバイス下取り詳細構造 ](../../images/field-groups/device-trade-in-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `tradeInValue` | [通貨](../../data-types/currency.md) | 取引されるデバイスの価格。 |
| `newDeviceID` | 文字列 | 取引される新しいデバイスの ID。 |
| `originalDeviceID` | 文字列 | 取引されるデバイスの ID。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.schema.json)

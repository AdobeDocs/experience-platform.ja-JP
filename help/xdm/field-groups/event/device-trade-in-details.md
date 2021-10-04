---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；デバイス；トレードイン；取引；
solution: Experience Platform
title: デバイスのトレードインの詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「 Device Trade-In Details 」スキーマフィールドグループの概要を説明します。
exl-id: 744557be-0297-453f-9134-9d0f4ef2df4d
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 5%

---

# [!UICONTROL Device Trade-In ] Detailsschema フィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL デバイスのトレードイ] ン [[!DNL XDM ExperienceEvent] ：クラスの標準スキーマフィールドグル](../../classes/experienceevent.md)ープを説明します。このフィールド (`deviceTradeInDetails`) には、デバイスの取引取引を示す 1 つのフィールド（取引額、元のデバイス ID、新しいデバイス ID など）が表示されます。

![デバイスのトレードインの詳細構造](../../images/field-groups/device-trade-in-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `tradeInValue` | [通貨](../../data-types/currency.md) | 取引されるデバイスの値。 |
| `newDeviceID` | 文字列 | 取引対象の新しいデバイスの ID。 |
| `originalDeviceID` | 文字列 | 取引されるデバイスの ID。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.schema.json)

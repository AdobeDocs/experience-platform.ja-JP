---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；デバイス；トレードイン；トレードイン；
solution: Experience Platform
title: Device Trade-In Details Schema Field Group
topic-legacy: overview
description: このドキュメントでは、「 Device Trade-In Details 」スキーマフィールドグループの概要を説明します。
source-git-commit: 2592d4f494d4d3dcfba63eb539498416fbdf6707
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 5%

---

# [!UICONTROL Device Trade-In ] Detailsschemaフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL デバイスのトレードイ] ン： [[!DNL XDM ExperienceEvent] クラスの標準スキーマフィールドグル](../../classes/experienceevent.md)ープを表します。このフィールド(`deviceTradeInDetails`)は、デバイスのトレードイントランザクションを表し、トレードイン値、元のデバイスID、新しいデバイスIDなどを示します。

![デバイスのトレードインの詳細構造](../../images/field-groups/device-trade-in-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `tradeInValue` | [通貨](../../data-types/currency.md) | 取引されるデバイスの値。 |
| `newDeviceID` | 文字列 | 取引対象の新しいデバイスのID。 |
| `originalDeviceID` | 文字列 | 取引されるデバイスのID。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-device-trade-in-details.schema.json)

---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；phoneNumber;xdm:phoneNumber；データタイプ；データタイプ；
solution: Experience Platform
title: 電話番号データタイプ
description: 電話番号 XDM データタイプについて説明します。
exl-id: b84e48f9-bbb4-4b8b-9476-4bc1c455ecfd
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 18%

---

# [!UICONTROL  電話番号 ] データタイプ

[!UICONTROL  電話番号 ] は、電話番号の詳細を説明する標準の XDM データタイプです。

<img src="../images/data-types/phone-number.png" width="600" /><br />

| プロパティ | 説明 |
| --- | --- |
| `extension` | 民間の交換機、オペレーター、スイッチボードから電話をかけるために使用される内部ダイヤル番号。 |
| `number` | 電話番号。電話番号は文字列で、意味のある文字（角括弧 `()`、ハイフン `-` など）や、サブダイヤル識別子を示す文字（内線 `x` 例：`1-353(0)18391111`、`+613 9403600x1234` など）を含める場合があることに注意してください。 |
| `primary` | これが個人のプライマリ電話番号かどうかを示すブール値。 アドレスやメールアドレスとは異なり、複数のプライマリ電話番号（通信チャネルごとに 1 つ）が存在する場合があります。 通信チャネルは、タイプ（親プロパティの名前で示される）によって定義されます。タイプには、`textMessaging`、`mobile`、`phone`、`home`、`work`、`unknown` および `fax` があります。 |
| `status` | 電話番号が現在使用できるかどうかを示します。 |
| `statusReason` | 現在のステータスの説明。 |
| `validity` | 電話番号の技術的な正確性のレベル。 |

{style="table-layout:auto"}

電話番号データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/phonenumber.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/phonenumber.schema.json)

---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;phoneNumber;xdm:phoneNumber;datatype;data-type;data type;
solution: Experience Platform
title: 電話番号のデータタイプ
topic: overview
description: このドキュメントでは、電話番号XDMデータタイプの概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 10%

---


# [!UICONTROL 電話番号] 、データ型

[!UICONTROL 電話番号] は、電話番号の詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/phone-number.png" width="600" /><br />

| プロパティ | 説明 |
| --- | --- |
| `extension` | 私用の交換所、オペレータ、または配電盤からの呼び出しに使用する内部ダイヤル番号。 |
| `number` | 電話番号。Note the phone number is a string and may include meaningful characters such as brackets `()`, hyphens `-`, or characters to indicate sub-dialing identifiers like extensions `x` for example, `1-353(0)18391111` or `+613 9403600x1234`. |
| `primary` | これが個人の主な電話番号であるかどうかを示すBoolean値です。 住所や電子メールアドレスとは異なり、複数の主電話番号が存在する場合があります。1つの通信チャネルにつき1つ。 通信チャネルは、タイプ（親プロパティの名前で示される）で定義されます。 `textMessaging`、 `mobile`、、 `phone`、 `home`、、 `work`、 `unknown`および `fax`、 |
| `status` | 電話番号を現在使用できるかどうかを示します。 |
| `statusReason` | 現在のステータスの説明。 |
| `validity` | 電話番号の技術的な正確性のレベル。 |

電話番号データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.schema.json)
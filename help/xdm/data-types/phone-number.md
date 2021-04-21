---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ;phoneNumber;xdm:phoneNumber；データ型；データ型；
solution: Experience Platform
title: 電話番号データタイプ
topic-legacy: overview
description: このドキュメントでは、電話番号XDMデータタイプの概要を説明します。
exl-id: b84e48f9-bbb4-4b8b-9476-4bc1c455ecfd
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 9%

---

# [!UICONTROL 電話番号] のデータ型

[!UICONTROL 電話番号] は、電話番号の詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/phone-number.png" width="600" /><br />

| プロパティ | 説明 |
| --- | --- |
| `extension` | 私用の交換所、オペレータ、または配電盤からの呼び出しに使用する内部ダイヤル番号。 |
| `number` | 電話番号。電話番号は文字列で、角括弧`()`、ハイフン`-`、拡張子`x`や`+613 9403600x1234`などの副ダイヤル識別子を示す文字を含めることができます。`1-353(0)18391111` |
| `primary` | これが個人の主な電話番号であるかどうかを示すBoolean値です。 住所や電子メールアドレスとは異なり、複数の主電話番号が存在する場合があります。1つの通信チャネルにつき1つ。 通信チャネルは、タイプ（親プロパティの名前で示される）で定義されます。`textMessaging`、`mobile`、`phone`、`home`、`work`、`unknown`、`fax`。 |
| `status` | 電話番号を現在使用できるかどうかを示します。 |
| `statusReason` | 現在のステータスの説明。 |
| `validity` | 電話番号の技術的な正確性のレベル。 |

電話番号データ型の詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；phoneNumber;xdm:phoneNumber；データ型；データ型；
solution: Experience Platform
title: 電話番号のデータタイプ
topic-legacy: overview
description: このドキュメントでは、電話番号XDMデータタイプの概要を説明します。
exl-id: b84e48f9-bbb4-4b8b-9476-4bc1c455ecfd
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 10%

---

# [!UICONTROL 電話番] 号データ型

[!UICONTROL 電話番] 号は、電話番号の詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/phone-number.png" width="600" /><br />

| プロパティ | 説明 |
| --- | --- |
| `extension` | 私的な交換機、オペレーター、または配電盤からの呼び出しに使用される内部ダイヤル番号。 |
| `number` | 電話番号。電話番号は文字列で、角括弧`()`、ハイフン`-`、内線番号`x`などのサブダイヤル識別子を示す文字（例：`1-353(0)18391111`や`+613 9403600x1234`）を含めることができます。 |
| `primary` | これが個人のプライマリ電話番号であるかどうかを示すBoolean値です。 アドレスや電子メールアドレスとは異なり、複数の主電話番号が存在する場合があります。通信チャネルごとに1つ。 通信チャネルは、タイプ（親プロパティの名前で示されます）で定義されます。`textMessaging`、`mobile`、`phone`、`home`、`work`、`unknown`、および`fax`です。 |
| `status` | 電話番号を現在使用できるかどうかを示します。 |
| `statusReason` | 現在のステータスの説明。 |
| `validity` | 電話番号の技術的な正確性のレベル。 |

{style=&quot;table-layout:auto&quot;}

電話番号のデータタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/phonenumber.schema.json)

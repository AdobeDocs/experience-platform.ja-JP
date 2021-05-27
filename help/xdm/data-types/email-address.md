---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；emailAddress;xdm:emailAddress；電子メール；電子メールアドレス；データ型；データ型；
solution: Experience Platform
title: 電子メールアドレスのデータタイプ
topic-legacy: overview
description: このドキュメントでは、 EメールアドレスXDMデータタイプの概要を説明します。
exl-id: 1364df42-f89f-4f48-bcda-5332f3828326
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 2%

---

# [!UICONTROL Eメールア] ドレスデータ型

[!UICONTROL Eメール] アドレスは、Eメールアドレスの詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/email-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822以降の標準で一般的に定義されるEメールの技術アドレス（例：`name@domain.com`）。 |
| `label` | 使用可能な追加の表示情報。 例えば、電子メールに`John Smith smithjr@company.uk`というMicrosoft Outlookのリッチアドレス表示がある場合、`John Smith`はこのフィールドに配置されます。 |
| `primary` | これが個人のプライマリ電子メールアドレスであるかどうかを示します。 1つのプロファイルに指定できるEメールアドレスは、一度に1つだけです。`primary` |
| `status` | 電子メールアドレスが現在使用可能かどうかを示します |
| `statusReason` | 現在の`status`の説明。 |
| `type` | アカウントと個人との関係（`work`や`personal`など）。 |

{style=&quot;table-layout:auto&quot;}


Eメールアドレスのデータタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.schema.json)

---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；emailAddress;xdm:emailAddress；電子メール；電子メールアドレス；データ型；データ型；データ型；
solution: Experience Platform
title: 電子メールアドレスのデータタイプ
topic-legacy: overview
description: このドキュメントでは、 E メールアドレス XDM データタイプの概要を説明します。
exl-id: 1364df42-f89f-4f48-bcda-5332f3828326
source-git-commit: 12c3f440319046491054b3ef3ec404798bb61f06
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 2%

---

# [!UICONTROL E メールア] ドレスデータ型

[!UICONTROL E メール] アドレスは、E メールアドレスの詳細を記述する標準の XDM データ型です。

<img src="../images/data-types/email-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822 以降の標準で一般的に定義される E メールの技術アドレス（例：`name@domain.com`）。 |
| `label` | 利用可能な追加の表示情報。 例えば、Microsoft Outlook のリッチアドレス表示が `John Smith smithjr@company.uk` の電子メールの場合、`John Smith` はこのフィールドに配置されます。 |
| `primary` | これが個人のプライマリ E メールアドレスであるかどうかを示します。 1 つのプロファイルに指定できる E メールアドレスは、一度に 1 つだけ `primary` です。 |
| `status` | 電子メールアドレスが現在使用可能かどうかを示します |
| `statusReason` | 現在の `status` の説明。 |
| `type` | アカウントと個人との関係（`work` や `personal` など）。 |

{style=&quot;table-layout:auto&quot;}


E メールアドレスのデータタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.schema.json)

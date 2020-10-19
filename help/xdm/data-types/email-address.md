---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;emailAddress;xdm:emailAddress;email;email address;datatype;data-type;data type;
solution: Experience Platform
title: 電子メールアドレスのデータ型
topic: overview
description: このドキュメントでは、Email Address XDMデータ型の概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---


# [!UICONTROL 電子メールアドレス] ・データ型

[!UICONTROL 電子メールアドレス] (email address)は、電子メールアドレスの詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/email-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822およびそれ以降の標準(例： `name@domain.com`)で一般的に定義されている電子メールの技術アドレス。 |
| `label` | 使用可能な追加の表示情報。 例えば、電子メールにMicrosoft Outlookのリッチアドレス表示が含まれている場合 `John Smith smithjr@company.uk`、このフィールド `John Smith` にはが配置されます。 |
| `primary` | これが個人のプライマリ電子メールアドレスであるかどうかを示します。 1人のプロファイルに指定できる電子メールアドレスは、特定の時点で1つの `primary` みです。 |
| `status` | 電子メールアドレスを現在使用できるかどうかを示します |
| `statusReason` | A description of the current `status`. |
| `type` | アカウントと個人との関連付け方法( `work` やなど `personal`)。 |


電子メールアドレスのデータ型について詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.schema.json)
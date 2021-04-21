---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；電子メールアドレス；xdm:emailAddress；電子メール；電子メールアドレス；データ型；データ型；
solution: Experience Platform
title: 電子メールアドレスのデータ型
topic-legacy: overview
description: このドキュメントでは、Email Address XDMデータ型の概要を説明します。
exl-id: 1364df42-f89f-4f48-bcda-5332f3828326
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 1%

---

# [!UICONTROL Email ] addressdata type

[!UICONTROL 電子メール] アドレスは、電子メールアドレスの詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/email-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822およびそれ以降の標準で一般的に定義されている電子メールの技術アドレス（例：`name@domain.com`）。 |
| `label` | 使用可能な追加の表示情報。 例えば、電子メールに`John Smith smithjr@company.uk`というMicrosoft Outlookのリッチアドレス表示が含まれている場合、`John Smith`はこのフィールドに配置されます。 |
| `primary` | これが個人のプライマリ電子メールアドレスであるかどうかを示します。 1人のプロファイルに指定できる電子メールアドレスは、特定の時点で1つの`primary`のみです。 |
| `status` | 電子メールアドレスを現在使用できるかどうかを示します |
| `statusReason` | 現在の`status`の説明。 |
| `type` | アカウントと個人との関連付け方法（`work`や`personal`など）。 |


電子メールアドレスのデータ型について詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/emailaddress.schema.json)

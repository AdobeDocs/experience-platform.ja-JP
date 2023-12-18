---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；emailAddress;xdm:emailAddress；電子メール；電子メールアドレス；データ型；データ型；
solution: Experience Platform
title: 電子メールアドレスのデータタイプ
description: Email Address XDM データタイプについて説明します。
exl-id: 1364df42-f89f-4f48-bcda-5332f3828326
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# [!UICONTROL 電子メールアドレス] データタイプ

[!UICONTROL 電子メールアドレス] は、電子メールアドレスの詳細を説明する標準の Experience Data Model(XDM) データ型です。

<img src="../images/data-types/email-address.png" width="450" /><br />

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822 以降の標準で一般的に定義される E メールの技術アドレス ( 例： `name@domain.com`) をクリックします。<br><br>XDM では、検証に合格するには、電子メールアドレスに有効なトップレベルドメインが含まれている必要があります。 次を参照してください。 [文書](https://data.iana.org/TLD/tlds-alpha-by-domain.txt) :Internet Assigned Numbers Authority(IANA) で定義された、有効なトップレベルドメインの完全なリスト。 |
| `label` | 使用可能な追加の表示情報。 例えば、E メールにMicrosoft Outlook のリッチアドレス表示が `John Smith smithjr@company.uk`, `John Smith` がこのフィールドに配置されます。 |
| `primary` | これが個人の主要電子メールアドレスかどうかを示します。 プロファイルに設定できるのは 1 つだけです `primary` 特定の時点での電子メールアドレス。 |
| `status` | 電子メールアドレスが現在使用可能かどうかを示します |
| `statusReason` | 現在の `status`. |
| `type` | アカウントと個人との関係 ( `work` または `personal`) をクリックします。 |

{style="table-layout:auto"}


電子メールアドレスのデータタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.schema.json)

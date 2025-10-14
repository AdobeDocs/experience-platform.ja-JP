---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；emailAddress;xdm:emailAddress；メール；メールアドレス；データタイプ；データタイプ；
solution: Experience Platform
title: メールアドレスのデータタイプ
description: メールアドレス XDM データタイプについて説明します。
exl-id: 1364df42-f89f-4f48-bcda-5332f3828326
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# [!UICONTROL &#x200B; メールアドレス &#x200B;] データタイプ

[!UICONTROL &#x200B; メールアドレス &#x200B;] は、メールアドレスの詳細を記述する標準の Experience Data Model （XDM）データタイプです。

![](../images/data-types/email-address.png){width=450}

| プロパティ | 説明 |
| --- | --- |
| `address` | RFC2822 および後続の標準で一般的に定義されているメールの技術的アドレス （例：`name@domain.com`）。<br><br>XDM では、検証に合格するために、メールアドレスに有効なトップレベルドメインが含まれている必要があります。 Internet Assigned Numbers Authority （IANA）が定義する有効な最上位ドメインの完全なリストについては、次の [&#x200B; ドキュメント &#x200B;](https://data.iana.org/TLD/tlds-alpha-by-domain.txt) を参照してください。 |
| `label` | 利用可能な追加の表示情報。 例えば、メールに `John Smith smithjr@company.uk` というMicrosoft Outlook リッチアドレス表示がある場合、`John Smith` はこのフィールドに配置されます。 |
| `primary` | これが個人のプライマリメールアドレスかどうかを示します。 プロファイルは、特定の時点で `primary` メールアドレスを 1 つのみ持つことができます。 |
| `status` | 電子メールアドレスが現在使用できるかどうかを示します |
| `statusReason` | 現在の `status` の説明。 |
| `type` | アカウントが人物に関わる方法（`work` や `personal` など）。 |

{style="table-layout:auto"}


メールアドレスデータタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/datatypes/demographic/emailaddress.schema.json)

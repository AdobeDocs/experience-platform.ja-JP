---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ブラウザー；ブラウザー詳細；データタイプ；データタイプ；データタイプ；
solution: Experience Platform
title: ブラウザーの詳細データタイプ
description: ブラウザー詳細 XDM データタイプについて説明します。
exl-id: c67ff8bc-0614-4422-9bb7-689b98d7086d
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 27%

---

# [!UICONTROL &#x200B; ブラウザーの詳細 &#x200B;] データタイプ

[!UICONTROL &#x200B; ブラウザーの詳細 &#x200B;] は、ブラウザーまたはアプリケーションに関する詳細を記述する標準の XDM データタイプです。

![](../images/data-types/browser-details.png){width=450}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acceptLanguage` | 文字列 | IETF 言語タグ （[RFC 5646](https://tools.ietf.org/html/rfc5646)）。 |
| `cookiesEnabled` | ブール値 | ユーザーの設定で Cookie の書き込みが許可されているかどうかを示します。 |
| `javaEnabled` | ブール値 | 観測元のデバイスで Java が有効になっているかどうかを示します。 |
| `javaScriptEnabled` | ブール値 | 観測元のデバイスでJavaScriptが有効になっているかどうかを示します。 |
| `javaScriptVersion` | 文字列 | 観察時にサポートされる JavaScript のバージョン。 |
| `javaVersion` | 文字列 | 観察時にサポートされる Java のバージョン。 |
| `name` | 文字列 | アプリケーションまたはブラウザーの名前。 |
| `quicktimeVersion` | 文字列 | 観察時にサポートされる Apple Quicktime のバージョン。 |
| `thirdPartyCookiesEnabled` | ブール値 | 観測元のデバイスでサードパーティ Cookie が有効になっているかどうかを示します。 |
| `userAgent` | 文字列 | クライアントリクエストからの HTTP ユーザーエージェント文字列。 |
| `vendor` | 文字列 | アプリケーションまたはブラウザーのベンダー。 |
| `version` | 文字列 | アプリケーションまたはブラウザーのバージョン。 |
| `viewportHeight` | 整数 | イベントが表示されたウィンドウ内の垂直方向のサイズ （ピクセル単位）。 Web ビューイベントの場合、これはブラウザービューポートの高さです。 |
| `viewportWidth` | 整数 | イベントが表示されたウィンドウ内の水平方向のサイズ （ピクセル単位）。 Web ビューイベントの場合、これはブラウザービューポートの幅です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.schema.json)

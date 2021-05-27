---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ブラウザー；ブラウザーの詳細；データ型；データ型；
solution: Experience Platform
title: ブラウザー詳細データタイプ
topic-legacy: overview
description: このドキュメントでは、ブラウザーの詳細XDMデータタイプの概要を説明します。
exl-id: c67ff8bc-0614-4422-9bb7-689b98d7086d
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 20%

---

# [!UICONTROL ブラウザーの] 詳細データタイプ

[!UICONTROL ブラウザ] ーの詳細は、ブラウザーまたはアプリケーションに関する詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/browser-details.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acceptLanguage` | 文字列 | IETF言語タグ([RFC 5646](https://tools.ietf.org/html/rfc5646))。 |
| `cookiesEnabled` | Boolean | ユーザーの設定でCookieの書き込みを許可するかどうかを示します。 |
| `javaEnabled` | Boolean | 観察元のデバイスでJavaが有効になっているかどうかを示します。 |
| `javaScriptEnabled` | Boolean | 観察元のデバイスでJavaScriptが有効になっているかどうかを示します。 |
| `javaScriptVersion` | 文字列 | 観察時にサポートされる JavaScript のバージョン。 |
| `javaVersion` | 文字列 | 観察時にサポートされる Java のバージョン。 |
| `name` | 文字列 | アプリケーション名またはブラウザー名。 |
| `quicktimeVersion` | 文字列 | 観察時にサポートされる Apple Quicktime のバージョン。 |
| `thirdPartyCookiesEnabled` | Boolean | 観察元のデバイスでサードパーティCookieが有効になっているかどうかを示します。 |
| `userAgent` | 文字列 | クライアントリクエストのHTTPユーザーエージェント文字列。 |
| `vendor` | 文字列 | アプリケーションまたはブラウザーのベンダー。 |
| `version` | 文字列 | アプリケーションまたはブラウザーのバージョン。 |
| `viewportHeight` | 整数 | イベントが表示されたウィンドウの垂直方向のサイズ（ピクセル単位）。 Webビューイベントの場合、これはブラウザーの表示の高さです。 |
| `viewportWidth` | 整数 | イベントが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Webビューイベントの場合、これはブラウザーの表示域の幅です。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.schema.json)

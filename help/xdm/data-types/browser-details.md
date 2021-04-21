---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；フィールド；スキーマ;スキーマ；ブラウザーの詳細；データ型；データ型；
solution: Experience Platform
title: ブラウザーの詳細データ型
topic-legacy: overview
description: このドキュメントでは、Browser Details XDMデータ型の概要を説明します。
exl-id: c67ff8bc-0614-4422-9bb7-689b98d7086d
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 19%

---

# [!UICONTROL ブラウザーの] 詳細データタイプ

[!UICONTROL ブラウザーの] 詳細は、ブラウザーまたはアプリケーションに関する詳細を記述する標準のXDMデータ型です。

<img src="../images/data-types/browser-details.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acceptLanguage` | 文字列 | IETF言語タグ([RFC 5646](https://tools.ietf.org/html/rfc5646))。 |
| `cookiesEnabled` | Boolean | ユーザーの設定でcookieの書き込みを許可するかどうかを示します。 |
| `javaEnabled` | ブール値 | 監視元のデバイスでJavaが有効になっているかどうかを示します。 |
| `javaScriptEnabled` | ブール値 | 監視が行われたデバイスでJavaScriptが有効になっているかどうかを示します。 |
| `javaScriptVersion` | 文字列 | 観察時にサポートされる JavaScript のバージョン。 |
| `javaVersion` | 文字列 | 観察時にサポートされる Java のバージョン。 |
| `name` | 文字列 | アプリケーションまたはブラウザーの名前。 |
| `quicktimeVersion` | 文字列 | 観察時にサポートされる Apple Quicktime のバージョン。 |
| `thirdPartyCookiesEnabled` | ブール値 | 監視元のデバイスでサードパーティCookieが有効になっているかどうかを示します。 |
| `userAgent` | 文字列 | クライアント要求のHTTPユーザーエージェント文字列。 |
| `vendor` | 文字列 | アプリケーションまたはブラウザーのベンダー。 |
| `version` | 文字列 | アプリケーションまたはブラウザーのバージョン。 |
| `viewportHeight` | 整数 | イベントが表示されたウィンドウの縦方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの高さです。 |
| `viewportWidth` | 整数 | イベントが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの幅です。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.schema.json)

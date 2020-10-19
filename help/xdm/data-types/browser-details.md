---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;fields;schemas;Schemas;browser;browser details;datatype;data-type;data type;
solution: Experience Platform
title: ブラウザーの詳細データタイプ
topic: overview
description: このドキュメントでは、Browser Details XDMデータ型の概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 21%

---


# [!UICONTROL ブラウザーの詳細] 、データタイプ

[!UICONTROL ブラウザーの詳細] は、ブラウザーやアプリケーションに関する詳細を説明する標準のXDMデータ型です。

<img src="../images/data-types/browser-details.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acceptLanguage` | 文字列 | An IETF language tag ([RFC 5646](https://tools.ietf.org/html/rfc5646)). |
| `cookiesEnabled` | Boolean | ユーザーの設定でcookieの書き込みを許可するかどうかを示します。 |
| `javaEnabled` | Boolean | 監視元のデバイスでJavaが有効になっているかどうかを示します。 |
| `javaScriptEnabled` | Boolean | 監視が行われたデバイスでJavaScriptが有効になっているかどうかを示します。 |
| `javaScriptVersion` | 文字列 | 観察時にサポートされる JavaScript のバージョン。 |
| `javaVersion` | 文字列 | 観察時にサポートされる Java のバージョン。 |
| `name` | 文字列 | アプリケーションまたはブラウザーの名前。 |
| `quicktimeVersion` | 文字列 | 観察時にサポートされる Apple Quicktime のバージョン。 |
| `thirdPartyCookiesEnabled` | Boolean | 監視元のデバイスでサードパーティCookieが有効になっているかどうかを示します。 |
| `userAgent` | 文字列 | クライアント要求のHTTPユーザーエージェント文字列。 |
| `vendor` | 文字列 | アプリケーションまたはブラウザーのベンダー。 |
| `version` | 文字列 | アプリケーションまたはブラウザーのバージョン。 |
| `viewportHeight` | 整数 | イベントが表示されたウィンドウの縦方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの高さです。 |
| `viewportWidth` | 整数 | イベントが表示されたウィンドウの水平方向のサイズ（ピクセル単位）。 Web表示イベントの場合、これはブラウザビューポートの幅です。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.schema.json)
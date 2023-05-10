---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；フィールド；スキーマ；スキーマ；ブラウザー；ブラウザーの詳細；データ型；データ型；
solution: Experience Platform
title: ブラウザー詳細データタイプ
description: このドキュメントでは、ブラウザーの詳細 XDM データタイプの概要を説明します。
exl-id: c67ff8bc-0614-4422-9bb7-689b98d7086d
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 23%

---

# [!UICONTROL ブラウザーの詳細] データタイプ

[!UICONTROL ブラウザーの詳細] は、ブラウザーまたはアプリケーションに関する詳細を記述する標準の XDM データ型です。

<img src="../images/data-types/browser-details.png" width="450" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `acceptLanguage` | 文字列 | IETF 言語タグ ([RFC 5646](https://tools.ietf.org/html/rfc5646)) をクリックします。 |
| `cookiesEnabled` | ブール値 | ユーザーの設定で Cookie の書き込みを許可するかどうかを示します。 |
| `javaEnabled` | ブール値 | 観測元のデバイスで Java が有効になったかどうかを示します。 |
| `javaScriptEnabled` | ブール値 | 観察元のデバイスで JavaScript が有効になったかどうかを示します。 |
| `javaScriptVersion` | 文字列 | 観察時にサポートされる JavaScript のバージョン。 |
| `javaVersion` | 文字列 | 観察時にサポートされる Java のバージョン。 |
| `name` | 文字列 | アプリケーションまたはブラウザーの名前。 |
| `quicktimeVersion` | 文字列 | 観察時にサポートされる Apple Quicktime のバージョン。 |
| `thirdPartyCookiesEnabled` | ブール値 | 観察元のデバイスでサードパーティ Cookie が有効になったかどうかを示します。 |
| `userAgent` | 文字列 | クライアントリクエストからの HTTP ユーザーエージェント文字列。 |
| `vendor` | 文字列 | アプリケーションまたはブラウザーのベンダー。 |
| `version` | 文字列 | アプリケーションまたはブラウザーのバージョン。 |
| `viewportHeight` | 整数 | イベントが表示されたウィンドウ内の垂直方向のサイズ（ピクセル単位）。 Web ビューポートイベントの場合、これはブラウザーの表示域の高さです。 |
| `viewportWidth` | 整数 | イベントが表示されたウィンドウ内の水平方向のサイズ（ピクセル単位）。 Web ビューポートイベントの場合、これはブラウザーの表示域の幅です。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/browserdetails.schema.json)

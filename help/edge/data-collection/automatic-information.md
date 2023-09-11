---
title: 自動的に収集された情報
description: Adobe Experience Platform Web SDK が自動的に収集するデータの概要です。
source-git-commit: 89b981104e3cbe597d1556484f4365866bf2a11d
workflow-type: tm+mt
source-wordcount: '331'
ht-degree: 32%

---

# 自動的に収集された情報

Adobe Experience Platform Web SDK は、初期設定の状態で一部の情報を自動的に収集します。 組織がこのデータを自動的に収集したくない場合は、 `context` オプションを [`configure` command](../fundamentals/configuring-the-sdk.md).

次から除外されたキーワード： `context` 配列がデータ収集に含まれていません。 次の場合、 `context` 配列が `configure` コマンドを使用すると、以下の表に示すすべてのデータが自動的に収集されます。

| 名前 | 説明 | `context` 配列キーワード | XDM パス | 値の例 |
| --- | --- | --- | --- | --- |
| 画面の高さ | 画面の高さ（ピクセル単位）。 | `device` | `events[].xdm.device.screenHeight` | `900` |
| 画面の幅 | 画面の幅（ピクセル単位）。 | `device` | `events[].xdm.device.screenWidth` | `1440` |
| 画面の向き | 画面の向き。 | `device` | `events[].xdm.device.screenOrientation` | `landscape` または `portrait` |
| 環境タイプ | エクスペリエンスが表示される環境のタイプ。 Adobe Experience Platform Web SDK は、このフィールドを常に `browser`. | `environment` | `events[].xdm.environment.type` | `browser` |
| ビューポートの高さ | ブラウザーのコンテンツ領域の高さをピクセル単位で指定します。 | `environment` | `events[].xdm.environment.browserDetails.viewportHeight` | `679` |
| ビューポートの幅 | ブラウザーのコンテンツ領域の幅をピクセル単位で指定します。 | `environment` | `events[].xdm.environment.browserDetails.viewportWidth` | `642` |
| SDK 名 | SDK の識別子。 このフィールドでは、URI を使用で、異なるソフトウェアライブラリで提供される ID 間の一意性を改善します。スタンドアロンライブラリを使用する場合、値は `https://ns.adobe.com/experience/alloy`. ライブラリをタグ拡張の一部として使用する場合、値は `https://ns.adobe.com/experience/alloy+reactor`. | | `events[].xdm.implementationDetails.name` | `https://ns.adobe.com/experience/alloy` |
| SDK のバージョン | スタンドアロンライブラリを使用する場合、値はライブラリのバージョンになります。 ライブラリをタグ拡張の一部として使用する場合、値は、ライブラリバージョンとタグ拡張バージョンを連結したものになります。 | | `events[].xdm.implementationDetails.version` | `2.1.0+2.1.3` |
| 環境 | データが収集された環境。 Adobe Experience Platform Web SDK は、このフィールドを常に `browser`. | | `events[].xdm.implementationDetails.environment` | `browser` |
| 現地時間 | エンドユーザーのローカルタイムスタンプ（簡易拡張 ISO 形式 [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6) のエンドユーザーのローカルタイムスタンプを使用）。 | `placeContext` | `events[].xdm.placeContext.localTime` | `YYYY-08-07T15:47:17.129-07:00` |
| ローカルタイムゾーンのオフセット | ユーザーが GMT からオフセットする時間（分）。 | `placeContext` | `events[].xdm.placeContext.localTimezoneOffset` | `360` |
| タイムスタンプ | エンドユーザーの UTC タイムスタンプ（簡易拡張 ISO 形式のエンドユーザーの UTC タイムスタンプ）。 [ISO 8601](https://datatracker.ietf.org/doc/html/rfc3339#section-5.6). | 常に含まれる | `events[].xdm.timestamp` | `YYYY-08-07T22:47:17.129Z` |
| 現在のページ URL | 現在のページの URL。 | `web` | `events[].xdm.web.webPageDetails.URL` | `https://example.com/index.html` |
| リファラー URL | 前に訪問したページの URL。 | `web` | `events[].xdm.web.webReferrer.URL` | `http://example.org/linkedpage.html` |

{style="table-layout:auto"}

---
title: Adobe Experience Platform Web SDK で自動的に収集される情報
description: Adobe Experience Platform SDK が自動的に収集する各情報の概要です。
keywords: 情報の収集；コンテキスト；設定；デバイス；screenHeight;screenHeight;screenOrientation;screenOrientation;screenWidth;screenWidth;environment;viewportHeight;viewportHeight;viewportWidth;viewportWidth;viewportWidth;crowserDetails；ブラウザー詳細；実装の詳細；実装の詳細；バージョン；placeContext;localTime;localTimezoneOffset ローカルタイムゾーンのオフセット；タイムスタンプ；web;url;webPageDetails;web ページの詳細；webReferrer;web Referrer;landscape;portrait;
exl-id: 901df786-df36-4986-9c74-a32d29c11b71
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 59%

---

# 自動的に収集された情報

Adobe Experience Platform Web SDK は、特別な設定をしなくても、多数の情報を自動的に収集します。 ただしこの情報は、必要に応じて、`configure` コマンドの `context` オプションで無効にすることができます。[SDK の設定を参照してください](../fundamentals/configuring-the-sdk.md)。以下に、その情報の一覧を示します。括弧内の名前は、コンテキストの設定時に使用する文字列を示します。

## デバイス（`device`）

デバイスに関する情報。ユーザーエージェント文字列からサーバサイドを参照できるデータは含みません。

### 画面の高さ

| **ペイロード内のパス：** | **例：** |
| ---------------------------------- | ------------ |
| `events[].xdm.device.screenHeight` | `900` |

画面の高さ（ピクセル単位）。

### 画面の向き

| **ペイロード内のパス：** | **可能な値：** |
| --------------------------------------- | ------------------------- |
| `events[].xdm.device.screenOrientation` | `landscape` または `portrait` |

画面の向き。

### 画面の幅

| **ペイロード内のパス：** | **例：** |
| --------------------------------- | ------------ |
| `events[].xdm.device.screenWidth` | `1440` |

画面の幅をピクセル単位で指定します。

## 環境（`environment`）

ブラウザー環境の詳細。

### 環境タイプ

ブラウザー

| **ペイロード内のパス：** | **例：** |
| ------------------------------- | ------------ |
| `events[].xdm.environment.type` | `browser` |

エクスペリエンスが表示される環境のタイプ。 Adobe Experience Platform Web SDK は、常に `browser` に設定します。

### ビューポートの高さ

| **ペイロード内のパス：** | **例：** |
| -------------------------------------------------------- | ------------ |
| `events[].xdm.environment.browserDetails.viewportHeight` | `679` |

ブラウザーのコンテンツ領域の高さをピクセル単位で指定します。

### ビューポートの幅

| **ペイロード内のパス：** | **例：** |
| ------------------------------------------------------- | ------------ |
| `events[].xdm.environment.browserDetails.viewportWidth` | `642` |

ブラウザーのコンテンツ領域の幅をピクセル単位で指定します。

## 実装の詳細

イベントの収集に使用した SDK に関する情報です。

### 名前

| **ペイロード内のパス：** | **例：** |
| ----------------------------------------- | --------------------------------------- |
| `events[].xdm.implementationDetails.name` | `https://ns.adobe.com/experience/alloy` |

ソフトウェア開発キット（SDK）の ID。このフィールドでは、URI を使用で、異なるソフトウェアライブラリで提供される ID 間の一意性を改善します。スタンドアロンライブラリを使用する場合、値は `https://ns.adobe.com/experience/alloy` です。 ライブラリをタグ拡張の一部として使用する場合、値は `https://ns.adobe.com/experience/alloy+reactor` です。

### バージョン

| **ペイロード内のパス：** | **例：** |
| -------------------------------------------- | ------------ |
| `events[].xdm.implementationDetails.version` | `0.11.0` |

スタンドアロンライブラリを使用する場合、の値は単にライブラリバージョンです。 ライブラリをタグ拡張の一部として使用する場合、これはライブラリバージョンであり、タグ拡張のバージョンは「+」で結合されています。 例えば、ライブラリのバージョンが 2.1.0 で、タグ拡張のバージョンが 2.1.3 の場合、値は `2.1.0+2.1.3` になります。

### 環境

| **ペイロード内のパス：** | **例：** |
| ------------------------------------------------ | ------------ |
| `events[].xdm.implementationDetails.environment` | `browser` |

データが収集された環境。 これは常に `browser` に設定されます。

## 場所コンテキスト（`placeContext`）

エンドユーザーの場所に関する限定的な情報。

### 現地時間

| **ペイロード内のパス：** | **例：** |
| ------------------------------------- | ------------------------------- |
| `events[].xdm.placeContext.localTime` | `2019-08-07T15:47:17.129-07:00` |

エンドユーザーのローカルタイムスタンプ（簡易拡張 ISO 形式 [ISO 8601](https://tools.ietf.org/html/rfc3339#section-5.6) のエンドユーザーのローカルタイムスタンプを使用）。

### ローカルタイムゾーンのオフセット

| **ペイロード内のパス：** | **例：** |
| ----------------------------------------------- | ------------ |
| `events[].xdm.placeContext.localTimezoneOffset` | `360` |

ユーザーが GMT からオフセットする時間（分）。

## タイムスタンプ

| **ペイロード内のパス：** | **例：** |
| ------------------------ | -------------------------- |
| `events[].xdm.timestamp` | `2019-08-07T22:47:17.129Z` |

イベントのタイムスタンプ。コンテキストのこの部分は削除できません。

エンドユーザーの UTC タイムスタンプ（簡易拡張 ISO 形式 [ISO 8601](https://tools.ietf.org/html/rfc3339#section-5.6) のエンドユーザーのローカルタイムスタンプを使用）。

## Web の詳細（`web`）

ユーザーが閲覧しているページの詳細。

### 現在のページ URL

| **ペイロード内のパス：** | **例：** |
| ------------------------------------- | ------------------------------------ |
| `events[].xdm.web.webPageDetails.URL` | `https://somesite.com/somepage.html` |

現在のページの URL。

### リファラー URL

| **ペイロード内のパス：** | **例：** |
| ---------------------------------- | ----------------------------------------- |
| `events[].xdm.web.webReferrer.URL` | `http://somereferrer.com/linkedpage.html` |

前に訪問したページの URL。

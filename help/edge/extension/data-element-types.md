---
title: Adobe Experience PlatformWeb SDK Extensionのデータ要素の種類
description: Adobe Experience Platform LaunchのAdobe Experience PlatformWeb SDK Extensionが提供する様々なデータ要素タイプについて説明します。
translation-type: tm+mt
source-git-commit: 69f2e6069546cd8b913db453dd9e4bc3f99dd3d9
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 78%

---


# データ要素タイプ

[Adobe Experience Platform Launch](https://experienceleague.adobe.com/docs/launch.html)の[Adobe Experience PlatformWeb SDK拡張機能](web-sdk-extension.md)に[アクションタイプ](action-types.md)を設定した後、データ要素のタイプを設定します。

このページでは、使用可能なデータ要素のタイプについて説明します。

## イベント結合 ID

このデータ要素を使用すると、イベント結合 ID が提供されます。このデータ要素には設定は必要ありません。提供されたデータ要素は、訪問者がページを離れるか、「イベント結合 ID をリセット」アクションタイプが使用されるまで変わりません。

## ID マップ

ID マップデータ要素を使用すると、他のデータ要素や指定した他の値から ID を作成できます。作成するすべての ID は、対応する名前空間に関連付ける必要があります。このデータ要素には、すべてのデフォルト名前空間と、作成したすべての名前空間を示すドロップダウンが表示されます。

![](./assets/identity-map-data-element.png)

## XDM オブジェクト

Adobe Experience Platform Web SDK に送信するデータは、すべて XDM 形式にする必要があります。XDM オブジェクトデータ要素を使用すると、データの形式を簡単に設定できます。このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、スキーマの構造が表示され、容易に入力できます。

![](./assets/XDM-object.png)

スキーマの特定のフィールド（`web.webPageDetails.URL` など）を開くと、一部の項目が自動的に収集されることに注意してください。複数の項目が自動的に収集されますが、必要に応じてそれらの項目を上書きすることもできます。すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報を入力するだけです。データがソリューションに送信される際に、入力しなかったデータはすべて除外されます。

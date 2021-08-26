---
title: Adobe Experience Platform Web SDK拡張機能のデータ要素のタイプ
description: Adobe Experience Platform Web SDKタグ拡張機能で提供される様々なデータ要素のタイプについて説明します。
exl-id: 3c2c257f-1fbc-4722-8040-61ad19aa533f
source-git-commit: 4caab19e1f58fc5cec5a3c56c43e47786d49c3dc
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 19%

---

# データ要素タイプ

[Adobe Experience Platform Web SDKタグ拡張機能](web-sdk-extension-configuration.md)で[アクションタイプ](action-types.md)を設定した後、データ要素タイプを設定します。

このページでは、使用可能なデータ要素のタイプについて説明します。


## イベント結合 ID

このデータ要素を使用すると、イベント結合 ID が提供されます。このデータ要素には設定は必要ありません。提供されたデータ要素は、訪問者がページを離れるか、「イベント結合 ID をリセット」アクションタイプが使用されるまで変わりません。

## ID マップ

IDマップを使用すると、Webページの訪問者のIDを設定できます。 IDマップは、_phone_&#x200B;や&#x200B;_email_&#x200B;などの名前空間で構成され、各名前空間には1つ以上の識別子が含まれます。 例えば、Webサイト上の個人が2つの電話番号を指定した場合、電話の名前空間には2つの識別子を含める必要があります。

[!UICONTROL IDマップ]データ要素で、各識別子に関する次の情報を提供します。

* **[!UICONTROL ID]**:訪問者を識別する値。例えば、識別子が&#x200B;_phone_&#x200B;名前空間に属する場合、[!UICONTROL ID]は&#x200B;_555-555-5555_&#x200B;になります。 この値は、通常、ページ上のJavaScript変数または他のデータから得られるので、ページデータを参照するデータ要素を作成し、[!UICONTROL IDマップ]データ要素内の[!UICONTROL ID]フィールドでデータ要素を参照することをお勧めします。 ページ上で実行する場合、ID値が設定された文字列以外の値の場合、識別子はIDマップから自動的に削除されます。
* **[!UICONTROL 認証状態]**:訪問者が認証されたかどうかを示す選択。
* **[!UICONTROL プライマリ]**:識別子を個人の主識別子として使用する必要があるかどうかを示す選択。プライマリとしてマークされた識別子がない場合は、ECIDがプライマリ識別子として使用されます。

IDマップを作成する際には、ECIDを指定しないでください。 SDKを使用する場合、ECIDはサーバーで自動的に生成され、IDマップに含まれます。

IDマップデータ要素は、多くの場合、[[!UICONTROL XDMオブジェクト]データ要素タイプ](#xdm-object)および[[!UICONTROL Set consent]アクションタイプ](action-types.md#set-consent)と組み合わせて使用されます。

詳しくは、[Adobe Experience Platform IDサービス](https://experienceleague.adobe.com/docs/experience-platform/identity/home.html?lang=ja)を参照してください。

![](./assets/identity-map-data-element.png)

## XDM オブジェクト {#xdm-object}

XDM形式を使用して、任意のデータをAdobe Experience Platform Web SDKに送信します。 XDM オブジェクトデータ要素を使用すると、データの形式を簡単に設定できます。このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、簡単に入力できるスキーマの構造が表示されます。

![](./assets/XDM-object.png)

`web.webPageDetails.URL`など、スキーマの特定のフィールドを開くと、一部の項目が自動的に収集されます。 複数の項目が自動的に収集される場合でも、必要に応じて項目を上書きできます。 すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報を入力します。 データがソリューションに送信される際に、入力されていない部分はすべて省略されます。

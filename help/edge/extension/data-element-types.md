---
title: Adobe Experience Platform Web SDK 拡張機能のデータ要素のタイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なデータ要素タイプについて説明します。
exl-id: 3c2c257f-1fbc-4722-8040-61ad19aa533f
source-git-commit: 4caab19e1f58fc5cec5a3c56c43e47786d49c3dc
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 19%

---

# データ要素タイプ

[Adobe Experience Platform Web SDK タグ拡張機能 ](web-sdk-extension-configuration.md) で [ アクションタイプ ](action-types.md) を設定した後、データ要素のタイプを設定します。

このページでは、使用可能なデータ要素のタイプについて説明します。


## イベント結合 ID

このデータ要素を使用すると、イベント結合 ID が提供されます。このデータ要素には設定は必要ありません。提供されたデータ要素は、訪問者がページを離れるか、「イベント結合 ID をリセット」アクションタイプが使用されるまで変わりません。

## ID マップ

ID マップを使用すると、Web ページの訪問者の ID を設定できます。 ID マップは、_phone_ や _email_ などの名前空間で構成され、各名前空間には 1 つ以上の識別子が含まれます。 例えば、Web サイトのユーザーが 2 つの電話番号を指定した場合、電話の名前空間には 2 つの識別子を含める必要があります。

[!UICONTROL ID マップ ] データ要素で、各識別子に関する次の情報を提供します。

* **[!UICONTROL ID]**:訪問者を識別する値。例えば、識別子が _phone_ 名前空間に属する場合、[!UICONTROL ID] は _555-555-5555_ になります。 この値は通常、ページ上の JavaScript 変数または他のデータから派生するので、ページデータを参照するデータ要素を作成し、[!UICONTROL ID マップ ] データ要素内の [!UICONTROL ID] フィールドでデータ要素を参照することをお勧めします。 ページ上で実行する場合、ID 値が設定された文字列以外の値の場合、識別子は ID マップから自動的に削除されます。
* **[!UICONTROL 認証状態]**:訪問者が認証されたかどうかを示す選択。
* **[!UICONTROL プライマリ]**:識別子を個人の主識別子として使用するかどうかを示す選択。プライマリとしてマークされた識別子がない場合は、ECID がプライマリ識別子として使用されます。

ID マップを作成する際は、ECID を指定しないでください。 SDK を使用する場合、ECID はサーバーで自動的に生成され、ID マップに含まれます。

ID マップデータ要素は、[[!UICONTROL XDM オブジェクト ] データ要素の種類 ](#xdm-object) および [[!UICONTROL Set consent] アクションの種類 ](action-types.md#set-consent) と組み合わせて使用されることが多いです。

詳しくは、[Adobe Experience Platform ID サービス ](https://experienceleague.adobe.com/docs/experience-platform/identity/home.html?lang=ja) を参照してください。

![](./assets/identity-map-data-element.png)

## XDM オブジェクト {#xdm-object}

XDM 形式を使用して、任意のデータをAdobe Experience Platform Web SDK に送信します。 XDM オブジェクトデータ要素を使用すると、データの形式を簡単に設定できます。このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、スキーマの構造が表示され、簡単に入力できます。

![](./assets/XDM-object.png)

`web.webPageDetails.URL` など、スキーマの特定のフィールドを開くと、一部の項目が自動的に収集されます。 複数の項目が自動的に収集される場合でも、必要に応じて、項目を上書きできます。 すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報を入力します。 データがソリューションに送信されると、入力されていないものはすべて省略されます。

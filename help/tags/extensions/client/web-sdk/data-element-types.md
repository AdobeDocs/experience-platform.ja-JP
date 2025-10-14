---
title: Adobe Experience Platform Web SDK 拡張機能のデータ要素タイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なデータ要素タイプについて説明します。
exl-id: 3c2c257f-1fbc-4722-8040-61ad19aa533f
source-git-commit: e34a9ee5b1a09ff3391e5b0e981215fefbc157fc
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 5%

---


# データ要素タイプ

[2&rbrace;Adobe Experience Platform Web SDK タグ拡張機能 &#x200B;](action-types.md) で &lbrace;action types[&#128279;](web-sdk-extension-configuration.md) を設定した後、データ要素タイプを設定する必要があります。 このページでは、使用可能なデータ要素タイプについて説明します。

## ID マップ {#identity-map}

ID マップを使用すると、web ページの訪問者の ID を確立できます。 ID マップは、`CRMID`、`Phone`、`Email` などの名前空間で構成され、各名前空間には 1 つ以上の識別子が含まれています。 例えば、web サイトの個人が 2 つの電話番号を提供している場合、電話の名前空間には 2 つの識別子を含める必要があります。

[!UICONTROL ID マップ &#x200B;] データ要素では、各識別子に対して次の情報を提供します。

* **[!UICONTROL ID]**：訪問者を識別する値。 例えば、識別子が _phone_ 名前空間に属する場合、[!UICONTROL ID] は _555-555-5555_ になります。 通常、この値はページ上のJavaScript変数や他のデータから派生するので、ページデータを参照するデータ要素を作成し、[!UICONTROL ID マップ &#x200B;] データ要素内の [!UICONTROL ID] フィールドでデータ要素を参照することをお勧めします。 ページ上で実行しているとき、ID 値が入力された文字列以外の場合、識別子は ID マップから自動的に削除されます。
* **[!UICONTROL 認証状態]**：訪問者が認証されているかどうかを示す選択。
* **[!UICONTROL プライマリ]**：識別情報を個人のプライマリ識別情報として使用するかどうかを示す選択項目。 プライマリとしてマークされている識別子がない場合、ECID がプライマリ識別子として使用されます。

![&#x200B; データ要素の編集画面を示す UI 画像。](assets/identity-map-data-element.png)

>[!TIP]
>
>Adobeでは、プライマリ ID などの、人物を表す ID を送信するこ `Luma CRM Id` をお勧めします。
>
>ID マップに人物識別子（例：`Luma CRM Id`）が含まれている場合、人物識別子はプライマリ識別子になります。 それ以外の場合は、`ECID` がプライマリ ID になります。

ID マップを作成する際は、[!DNL ECID] を指定しないでください。 SDK を使用する場合、[!DNL ECID] がサーバー上で自動的に生成され、ID マップに含まれます。

ID マップデータ要素は、多くの場合、[[!UICONTROL XDM オブジェクト &#x200B;] データ要素タイプ &#x200B;](#xdm-object) および [[!UICONTROL &#x200B; 同意を設定 &#x200B;] アクションタイプ &#x200B;](action-types.md#set-consent) と並行して使用されます。

詳しくは、[Adobe Experience Platform ID サービス &#x200B;](../../../../identity-service/home.md) を参照してください。

## XDM オブジェクト {#xdm-object}

XDM オブジェクトデータ要素を使用すると、データを XDM にフォーマットする方が簡単です。 このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、スキーマの構造が表示されるので、簡単に入力できます。

![XDM オブジェクト構造を示す UI 画像。](assets/XDM-object.png)

スキーマの特定のフィールド（`web.webPageDetails.URL` など）を開くと、一部の項目が自動的に収集されます。 自動的に収集される項目は複数ありますが、必要に応じて上書きできます。 すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報のみを入力します。 入力されていない部分は、データがソリューションに送信される際に省略されます。

## Variable {#variable}

**[!UICONTROL 変数]** データ要素を使用して、ペイロードオブジェクトを作成できます。 [!UICONTROL XDM] オブジェクトと [!UICONTROL Data] オブジェクトの両方がサポートされています。

* [!UICONTROL XDM] を選択した場合は、目的の [!UICONTROL &#x200B; サンドボックス &#x200B;] と [!UICONTROL &#x200B; スキーマ &#x200B;] を選択します。
* [!UICONTROL &#x200B; データ &#x200B;] を選択したら、目的のソリューションを選択します。 使用可能なソリューションには、[!UICONTROL Adobe Analytics] および [!UICONTROL Adobe Target] が含まれます。

![&#x200B; データ要素オプションを示すタグ UI の画像。](assets/variable-data-element.png)

このデータ要素を作成したら、[&#x200B; 変数を更新 &#x200B;](./action-types.md#update-variable) アクションを使用して変更できます。 準備が整ったら、このデータ要素を [&#x200B; イベントを送信 &#x200B;](./action-types.md#send-event) アクションに含めて、データをデータストリームに送信できます。

## メディア：エクスペリエンスの品質 {#quality-experience}

**[!UICONTROL エクスペリエンスの品質]** データ要素は、ストリーミングメディアイベントをAdobe Experience Platformに送信する際に役立ちます。 この要素は、メディアセッションの作成時に追加できます。追加したメディアイベントには、更新されたエクスペリエンス品質データが含まれます。

![&#x200B; エクスペリエンスデータ要素の品質の作成画面を示す UI 画像。](assets/qoe-data-element.png)

## 次の手順 {#next-steps}

[ECID へのアクセス &#x200B;](accessing-the-ecid.md) など、特定のユースケースについて説明します。

---
title: Adobe Experience Platform Web SDK 拡張機能のデータ要素タイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なデータ要素タイプについて説明します。
exl-id: 3c2c257f-1fbc-4722-8040-61ad19aa533f
source-git-commit: 8c652e96fa79b587c7387a4053719605df012908
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 6%

---


# データ要素タイプ

を設定した後 [アクションタイプ](action-types.md) が含まれる [Adobe Experience Platform Web SDK タグ拡張機能](web-sdk-extension-configuration.md)を設定する必要があります。 このページでは、使用可能なデータ要素タイプについて説明します。

## ID マップ {#identity-map}

ID マップを使用すると、web ページの訪問者の ID を確立できます。 ID マップは、次のような名前空間で構成されます `CRMID`, `Phone` または `Email`各名前空間には 1 つ以上の識別子が含まれます。 例えば、web サイトの個人が 2 つの電話番号を提供している場合、電話の名前空間には 2 つの識別子を含める必要があります。

が含まれる [!UICONTROL ID マップ] データ要素：識別子ごとに、次の情報を提供します。

* **[!UICONTROL ID]**：訪問者を識別する値。 例えば、識別子がに属する場合 _電話番号_ 名前空間、 [!UICONTROL ID] 次になることができます： _555-555-5555_. 通常、この値はページ上の JavaScript 変数やその他のデータから派生するので、ページデータを参照するデータ要素を作成してから、内のデータ要素を参照することをお勧めします [!UICONTROL ID] 内のフィールド [!UICONTROL ID マップ] データ要素。 ページ上で実行しているとき、ID 値が入力された文字列以外の場合、識別子は ID マップから自動的に削除されます。
* **[!UICONTROL 認証状態]**：訪問者が認証済みかどうかを示す選択。
* **[!UICONTROL プライマリ]**：識別子を個人のプライマリ識別子として使用する必要があるかどうかを示す選択。 プライマリとしてマークされている識別子がない場合、ECID がプライマリ識別子として使用されます。

![データ要素の編集画面を示す UI 画像。](assets/identity-map-data-element.png)

>[!TIP]
>
>Adobeでは、次のような、人物を表す ID を送信することをお勧めします `Luma CRM Id` プライマリ ID として使用します。
>
>ID マップに人物識別子が含まれる場合（例： `Luma CRM Id`）に設定すると、個人識別子がプライマリ識別子になります。 そうでない場合、 `ECID` はプライマリ ID になります。

を指定しないでください [!DNL ECID] id マップを作成する場合。 SDK を使用する場合、 [!DNL ECID] はサーバー上で自動的に生成され、id マップに含まれます。

ID マップデータ要素は、と並行して使用されることがよくあります [[!UICONTROL XDM オブジェクト] データ要素タイプ](#xdm-object) および [[!UICONTROL 同意を設定] アクションタイプ](action-types.md#set-consent).

詳細を読む： [Adobe Experience Platform ID サービス](../../../../identity-service/home.md).

## XDM オブジェクト {#xdm-object}

XDM オブジェクトデータ要素を使用すると、データを XDM にフォーマットする方が簡単です。 このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、スキーマの構造が表示されるので、簡単に入力できます。

![XDM オブジェクト構造を示す UI 画像。](assets/XDM-object.png)

スキーマの特定のフィールド（など）を開く場合に注意してください `web.webPageDetails.URL`、一部の項目は自動的に収集されます。 自動的に収集される項目は複数ありますが、必要に応じて上書きできます。 すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報のみを入力します。 入力されていない部分は、データがソリューションに送信される際に省略されます。

## Variable {#variable}

次を使用して、ペイロードオブジェクトを作成できます **[!UICONTROL 変数]** データ要素。 両方 [!UICONTROL XDM] および [!UICONTROL データ] オブジェクトがサポートされています。

* 選択した場合 [!UICONTROL XDM]、目的のを選択します [!UICONTROL Sandbox] および [!UICONTROL スキーマ].
* 選択した場合 [!UICONTROL データ]を選択し、目的のソリューションを選択します。 使用可能なソリューションは次のとおりです [!UICONTROL Adobe Analytics] および [!UICONTROL Adobe Target].

![データ要素オプションを示すタグ UI の画像。](assets/variable-data-element.png)

このデータ要素を作成したら、 [変数を更新](./action-types.md#update-variable) 変更するアクション。 準備が整ったら、このデータ要素を [イベントを送信](./action-types.md#send-event) データストリームにデータを送信するアクション。

## 次の手順 {#next-steps}

などの特定のユースケースについて説明します [ecid へのアクセス](accessing-the-ecid.md).

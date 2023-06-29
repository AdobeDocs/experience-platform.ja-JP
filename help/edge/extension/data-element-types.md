---
title: Adobe Experience Platform Web SDK 拡張機能のデータ要素タイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なデータ要素タイプについて説明します。
exl-id: 3c2c257f-1fbc-4722-8040-61ad19aa533f
source-git-commit: eef0b50b12b0e3be34ad519f2d106392c23b7d69
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 6%

---


# データ要素タイプ

設定後、 [アクションタイプ](action-types.md) 内 [Adobe Experience Platform Web SDK タグ拡張機能](web-sdk-extension-configuration.md)を設定する場合は、データ要素タイプを設定する必要があります。 このページでは、使用可能なデータ要素のタイプについて説明します。

## ID マップ {#identity-map}

ID マップを使用すると、Web ページの訪問者の ID を設定できます。 ID マップは、次のような名前空間で構成されます。 _phone_ または _電子メール_&#x200B;を作成し、各名前空間に 1 つ以上の識別子を含めます。 例えば、Web サイト上のユーザーが 2 つの電話番号を指定した場合、電話の名前空間には 2 つの識別子を含める必要があります。

内 [!UICONTROL ID マップ] データ要素に関連付ける場合、識別子ごとに次の情報が提供されます。

* **[!UICONTROL ID]**:訪問者を識別する値。 例えば、識別子が _phone_ 名前空間、 [!UICONTROL ID] may _555-555-5555_. この値は、通常、ページ上の JavaScript 変数または他のデータから派生するので、ページデータを参照するデータ要素を作成し、 [!UICONTROL ID] 内の [!UICONTROL ID マップ] データ要素。 ページ上で実行されているとき、ID 値が設定された文字列以外の値の場合、識別子は ID マップから自動的に削除されます。
* **[!UICONTROL 認証状態]**:訪問者が認証されたかどうかを示す選択。
* **[!UICONTROL プライマリ]**:識別子を個人の主識別子として使用する必要があるかどうかを示す選択。 プライマリとしてマークされている識別子がない場合、ECID がプライマリ識別子として使用されます。

![データ要素を編集画面を示す UI 画像。](./assets/identity-map-data-element.png)

次の項目は指定しないでください： [!DNL ECID] id マップを作成する際に使用します。 SDK を使用する場合、 [!DNL ECID] はサーバー上で自動的に生成され、id マップに含まれます。

ID マップデータ要素は、多くの場合、 [[!UICONTROL XDM オブジェクト] データ要素の種類](#xdm-object) そして [[!UICONTROL 同意の設定] アクションタイプ](action-types.md#set-consent).

詳細を表示 [Adobe Experience Platform Identity Service](../../identity-service/home.md).

## XDM オブジェクト {#xdm-object}

XDM オブジェクトデータ要素を使用すると、データの XDM への形式設定が簡単になります。 このデータ要素を初めて開いたときに、正しい Adobe Experience Platform サンドボックスとスキーマを選択してください。スキーマを選択すると、スキーマの構造が表示され、簡単に入力できます。

![XDM オブジェクト構造を示す UI 画像。](assets/XDM-object.png)

スキーマの特定のフィールド ( `web.webPageDetails.URL`を含めない場合、一部の項目は自動的に収集されます。 複数の項目が自動的に収集されますが、必要に応じて項目を上書きできます。 すべての値は、手動で入力することも、他のデータ要素を使用して入力することもできます。

>[!NOTE]
>
>収集したい情報のみを入力します。 データがソリューションに送信される際には、入力されなかった項目はすべて省略されます。

## Variable {#variable}

XDM オブジェクトを作成する別の方法は、 **[!UICONTROL 変数]** データ要素。 一方、XDM オブジェクトのデータ要素は、参照時に作成されます ( 例： `sendEvent` コマンド、 **[!UICONTROL 変数]** データ要素は、 [!UICONTROL 変数を更新] アクション。 データ要素を使用するには、適切なAdobe Experience Platformサンドボックスとスキーマを選択します。

![データ要素を作成画面を示す UI 画像。](assets/variable-data-element.png)

このデータ要素を作成したら、 [変数を更新](./action-types.md#update-variable) アクションを使用してデータ要素を変更できます。 次に、イベント送信アクション内で、XDM オプションに対して変数データ要素を使用します。

## 次の手順 {#next-steps}

次のような特定の使用例について説明します。 [ECID へのアクセス](accessing-the-ecid.md).

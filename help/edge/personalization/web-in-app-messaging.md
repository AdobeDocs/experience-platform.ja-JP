---
title: Web SDK での Web アプリ内メッセージサポートの設定
description: Web アプリ内メッセージをサポートするように Web SDK タグ拡張を設定する方法について説明します。
source-git-commit: a020f880be2606024c6a986dc468d70a2fbdc30f
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 0%

---


# Web SDK での Web アプリ内メッセージサポートの設定


アプリ内メッセージとは、Web アプリケーション内のユーザーに送信し、特定の目標地点に導く通知です。

これらの通知は、新機能の促進、特別オファーの提示、ユーザーのオンボーディングの促進など、様々な目的で使用できます。

アプリ内メッセージを使用すると、オーディエンスと効果的に関わり合い、オーディエンスをアプリケーションの重要な側面に導くことができます。

>[!IMPORTANT]
>
>Web アプリ内メッセージは、 [Adobe Journey Optimizer](https://experienceleague.adobe.com/docs/journey-optimizer/using/ajo-home.html?lang=ja) 機能：Web SDK を使用してパーソナライズされたコンテンツを配信します。
>
>Web アプリ内メッセージキャンペーンの設定方法について詳しくは、 [Adobe Journey Optimizerドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer/using/in-app/create-in-app-web.html).


## 前提条件 {#prerequisites}

### Web SDK タグ拡張機能のバージョン {#extension-version}

Web アプリ内メッセージ機能には、Web SDK タグ拡張機能の最新バージョンが必要です。

### Web アプリ内メッセージ用の CSP の設定 {#csp}

設定時に [Web アプリ内メッセージ](../personalization/web-in-app-messaging.md)の場合、CSP に次のディレクティブを含める必要があります。

```
default-src  blob:;
```

CSP の設定について詳しくは、 [専用ドキュメント](../fundamentals/configuring-a-csp.md).

## Web SDK タグ拡張機能を使用した Web アプリ内メッセージの設定 {#tag-extension}

詳しくは、 [Web SDK タグ拡張機能の設定ページ](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md) を参照して、以下で説明する設定の場所を確認してください。

次の条件が満たされた後 [インストール済み](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#install-the-web-sdk-tag-extension) Web SDK タグ拡張機能を使用する場合は、次の手順に従って Web アプリ内メッセージ用の拡張機能を設定します。

Adobe Analytics の **[!UICONTROL パーソナライズ]** セクションで、 **[!UICONTROL パーソナライゼーションストレージを有効にする]** オプション。 このオプションを使用すると、Web SDK は、ページの読み込み時にユーザーに表示されたエクスペリエンスを追跡できます。

![タグ拡張設定ページのパーソナライゼーションストレージオプションを示す画像。](assets/web-in-app-messaging/enable-personalization-storage.png)


Web アプリ内メッセージでは、次の 2 種類のトリガーをサポートしています。

* [Platform へのデータの送信](#send-data-platform)
* [メッセージの手動トリガー](#manual-trigger)

使用するトリガーに応じて Web SDK タグ拡張を設定するには、以下の節を参照してください。

### の設定手順 **[!UICONTROL Platform へのデータ送信]** トリガー {#send-data-platform}

Web SDK 拡張機能を含むタグプロパティを選択し、 [新しいルールの作成](../../tags/ui/managing-resources/rules.md##create-a-rule) を次の設定で使用します。

1. **[!UICONTROL 拡張機能]**：[!UICONTROL コア]
2. **[!UICONTROL イベントタイプ]**: [!UICONTROL 読み込まれたライブラリ（ページ上部）]

   ![イベント設定画面を示す画像。](assets/web-in-app-messaging/rule-configuration.png)

3. 選択 **[!UICONTROL 変更を保持]** イベント設定を保存します。

次に、作成したルールにアクションを追加する必要があります。

1. Adobe Analytics の [!DNL Actions] セクション、選択 **[!UICONTROL 追加]**.
   ![ルールの編集画面を示す画像。](assets/web-in-app-messaging/add-action.png)

2. 次を使用します。 **[!UICONTROL アクション]** 設定：
   * **[!UICONTROL 拡張]**: [!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL アクションタイプ]**: [!UICONTROL イベントを送信]

     ![アクション設定画面を示す画像。](assets/web-in-app-messaging/action-configuration.png)

3. 画面の右側で、 **[!UICONTROL パーソナライズ]** セクションで、 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]** オプション。
   ![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/render-visual-personalization.png)

4. 画面の右側で、 **[!UICONTROL 決定コンテキスト]** 「 」セクションで、 **[!UICONTROL キー]**/**[!UICONTROL 値]** アプリ内メッセージの対象として認定されるために、キャンペーン設定で使用したペア。
   ![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/decision-context.png)

5. 選択 **[!UICONTROL 変更を保持]** 設定を保存します。


次に、新しく作成したルールをタグプロパティライブラリに追加する必要があります。 これをおこなうには、に移動します。 **[!UICONTROL 公開フロー]** をクリックし、以前に作成したルールを選択します。

![ライブラリ画面を示す画像。](assets/web-in-app-messaging/add-rule-to-library.png)

ライブラリにルールを追加したら、「 」を選択します。 **[!UICONTROL 開発用に保存およびビルド]**.

![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/publish-flow.png)

設定プロセスが完了し、メッセージがユーザーに表示される準備が整いました。

### 手動設定を使用するためのトリガー手順 {#manual-trigger}

Web SDK 拡張機能を含むタグプロパティを選択し、 [新しいルールの作成](../../tags/ui/managing-resources/rules.md##create-a-rule) を次の設定で使用します。

1. **[!UICONTROL 拡張機能]**：[!UICONTROL コア]
2. **[!UICONTROL イベントタイプ]**: [!UICONTROL クリック]
3. ページ上の特定の要素のトリガー、および任意の CSS セレクターで識別子を設定します。

   ![イベント設定画面を示す画像。](assets/web-in-app-messaging/event-configuration-manual.png)


次に、作成したルールにアクションを追加する必要があります。

1. Adobe Analytics の [!DNL Actions] セクション、選択 **[!UICONTROL 追加]**.
   ![ルールの編集画面を示す画像。](assets/web-in-app-messaging/add-action.png)

2. 次を使用します。 **[!UICONTROL アクション]** 設定：
   * **[!UICONTROL 拡張]**: [!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL アクションタイプ]**: [!UICONTROL ルールセットを評価]

     ![アクション設定画面を示す画像。](assets/web-in-app-messaging/manual-trigger-action.png)

3. 画面の右側で、 **[!UICONTROL 視覚的なパーソナライゼーションの決定をレンダリング]** オプション。
   ![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/manual-trigger-render.png)


4. 画面の右側で、 **[!UICONTROL 決定コンテキスト]** 「 」セクションで、 **[!UICONTROL キー]**/**[!UICONTROL 値]** アプリ内メッセージの対象として認定されるために、キャンペーン設定で使用したペア。
   ![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/manual-trigger-decision-context.png)

5. 選択 **[!UICONTROL 変更を保持]** 設定を保存します。

次に、新しく作成したルールをタグプロパティライブラリに追加する必要があります。 これをおこなうには、に移動します。 **[!UICONTROL 公開フロー]** をクリックし、以前に作成したルールを選択します。

![ライブラリ画面を示す画像。](assets/web-in-app-messaging/add-rule-to-library.png)

ライブラリにルールを追加したら、「 」を選択します。 **[!UICONTROL 開発用に保存およびビルド]**.

![パーソナライゼーション設定画面を示す画像。](assets/web-in-app-messaging/publish-flow.png)

設定プロセスが完了し、メッセージがユーザーに表示される準備が整いました。

## Web SDK JavaScript ライブラリを使用した Web アプリ内メッセージの設定 {#js-library}

Web SDK タグ拡張機能を使用する代わりに、Web SDK JavaScript ライブラリから直接 Web アプリ内メッセージを設定することもできます。



Adobe Journey Optimizerからの Web アプリ内メッセージは、2 つの方法で表示できます。

### 方法 1：パーソナライゼーションコンテンツを自動的に取得する {#automatic}

Web SDK でページの読み込み時にパーソナライゼーションコンテンツを自動的に取得するには、 `sendEvent` コマンドを使用します。

```js
  alloy("sendEvent", {
      renderDecisions: true,
      personalization: {
          surfaces: ['#welcome']
      }
  });
```

### 方法 2：ユーザーのアクションに基づいて、パーソナライゼーションコンテンツを手動で取得する {#manual}

ユーザーが特定のアクションを実行した後にのみパーソナライゼーションコンテンツを表示するには、 `evaluateRulesets` コマンドを使用します。

この例では、ユーザーが **[!UICONTROL 購入する]** 」ボタンをクリックします。

```js
 alloy("evaluateRulesets", {
     renderDecisions: true,
     personalization: {
         decisionContext: {
             "userAction": "buy_now"
         }
     }
 });
```

### パーソナライゼーションストレージの設定 {#personalization-storage}

ユーザーに対して、設定された回数だけ、またはユーザーがページを訪問するたびに、アプリ内メッセージを `personalizationStorageEnabled` 設定オプション。

Adobe Analytics の [Web SDK の設定](../fundamentals/configuring-the-sdk.md) を設定します。 `personalizationStorageEnabled` オプションを選択します。

* `personalizationStorageEnabled: true` トリガー内メッセージを、 [Adobe Journey Optimizerキャンペーン](https://experienceleague.adobe.com/docs/journey-optimizer/using/in-app/create-in-app-web.html#configure-inapp).
* `personalizationStorageEnabled: false` トリガー読み込みのたびにアプリ内メッセージが表示されます。
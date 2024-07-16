---
title: Adobe Experience Platform Web SDK 拡張機能のアクションタイプ
description: Adobe Experience Platform Web SDK タグ拡張機能で提供される様々なアクションタイプについて説明します。
solution: Experience Platform
exl-id: a4bf0bb9-59b4-4c43-97e6-387768176517
source-git-commit: fb9f7757d77b221c733bbed5124fa576a6b02ed2
workflow-type: tm+mt
source-wordcount: '1264'
ht-degree: 1%

---


# アクションタイプ

[Adobe Experience Platform Web SDK タグ拡張機能 ](web-sdk-extension-configuration.md) を設定したら、アクションタイプを設定する必要があります。

ここでは、[Adobe Experience Platform Web SDK タグ拡張機能 ](web-sdk-extension-configuration.md) でサポートされているアクションタイプについて説明します。


## 応答を適用 {#apply-response}

Edge Networkからの応答に基づいて様々なアクションを実行する場合は、「**[!UICONTROL 応答を適用]**」アクションタイプを使用します。 このアクションタイプは、通常、サーバーがブラウザーに対して最初の呼び出しを行い、その呼び出しから応答を受け取ってEdge Networkーで Web SDK を初期化するハイブリッドデプロイメントで使用されます。

このアクションタイプを使用すると、ハイブリッドパーソナライゼーションのユースケースでクライアントの読み込み時間が短縮される可能性があります。

![ 「応答を適用」アクションタイプを示すExperience Platformユーザーインターフェイスの画像 ](assets/apply-response.png)

このアクションタイプは、次の設定オプションをサポートしています。

* **[!UICONTROL インスタンス]**：使用する Web SDK インスタンスを選択します。
* **[!UICONTROL Response headers]**:Edge Networkサーバーコールから返されたヘッダーキーと値を含むオブジェクトを返すデータ要素を選択します。
* **[!UICONTROL Response body]**:Edge Networkレスポンスによって提供された JSON ペイロードを含むオブジェクトを返すデータ要素を選択します。
* **[!UICONTROL ビジュアルパーソナライゼーションの決定をレンダリング]**:Edge Networkが提供するパーソナライゼーションコンテンツを自動的にレンダリングし、コンテンツをあらかじめ非表示にしてちらつきを防ぐには、このオプションを有効にします。

## イベントを送信 {#send-event}

Adobe[!DNL Experience Platform] にイベントを送信し、Adobe Experience Platformが送信されたデータを収集して、その情報に基づいて行動できるようにします。 インスタンスを選択します（複数のインスタンスがある場合）。 送信したいデータは、「**[!UICONTROL XDM データ]**」フィールドで送信できます。 XDM スキーマの構造に準拠する JSON オブジェクトを使用します。 このオブジェクトは、ページまたは **[!UICONTROL カスタムコード]****[!UICONTROL データ要素]** を使用して作成できます。

イベントを送信アクションタイプには、実装に応じて役立つ他のフィールドがいくつかあります。 これらのフィールドはすべてオプションであることに注意してください。

* **タイプ：** このフィールドでは、XDM スキーマに記録されるイベントタイプを指定できます。 詳細は、`sendEvent` コマンドの [`type`](/help/web-sdk/commands/sendevent/type.md) を参照してください。
* **データ：** XDM スキーマに一致しないデータは、このフィールドを使用して送信できます。 このフィールドは、Adobe Target プロファイルを更新したり、Target Recommendations属性を送信したりする場合に便利です。 詳細は、`sendEvent` コマンドの [`data`](/help/web-sdk/commands/sendevent/data.md) を参照してください。<!--- **Merge ID:** If you would like to specify a merge ID for your event, you can do so in this field. Please note that the solutions downstream are not able to merge your event data at this time. -->
* **データセット ID:** データストリームで指定したデータセット以外のデータセットにデータを送信する必要がある場合は、ここでデータセット ID を指定できます。
* **ドキュメントがアンロードされます：** ユーザーがページから移動してもイベントがサーバーに到達するようにするには、「**[!UICONTROL ドキュメントがアンロードされます]**」チェックボックスをオンにします。 これにより、イベントはサーバーに到達できますが、応答は無視されます。
* **ビジュアルパーソナライゼーション決定のレンダリング：** ページ上でパーソナライズされたコンテンツをレンダリングする場合は、「**[!UICONTROL ビジュアルパーソナライゼーション決定のレンダリング]** チェックボックスをオンにします。 必要に応じて、決定範囲やサーフェスを指定することもできます。 パーソナライズされたコンテンツのレンダリングについて詳しくは、[ パーソナライゼーションドキュメント ](/help/web-sdk/personalization/rendering-personalization-content.md#automatically-rendering-content) を参照してください。

## 同意を設定 {#set-consent}

ユーザーから同意を得たら、「同意を設定」アクションタイプを使用して、この同意をAdobe Experience Platform Web SDK に伝える必要があります。 現在、「Adobe」と「IAB TCF」の 2 種類の標準がサポートされています。[ 顧客の同意環境設定のサポート ](../../../../web-sdk/commands/setconsent.md) を参照してください。 Adobeバージョン 2.0 を使用する場合、データ要素の値のみがサポートされます。 同意オブジェクトに解決されるデータ要素を作成します。

このアクションでは、ID マップを含めて同意を得た後で ID を同期できるようにするオプションのフィールドも提供されます。 同期は、同意が「保留中」または「アウト」と設定されている場合に役立ちます。これは、同意呼び出しが最初に実行される可能性が高いからです。

## 変数を更新 {#update-variable}

イベントの結果として XDM オブジェクトを変更するには、このアクションを使用します。 このアクションは、イベント XDM オブジェクトを記録するために、後で **[!UICONTROL イベントを送信]** アクションから参照できるオブジェクトを作成することを目的としています。

このアクションタイプを使用するには、[variable](data-element-types.md#variable) データ要素を定義する必要があります。 変更する変数データ要素を選択すると、[XDM オブジェクト ](data-element-types.md#xdm-object) データ要素のエディターと同様のエディターが表示されます。

![](assets/update-variable.png)

エディターに使用される XDM スキーマは、[!UICONTROL  変数 ] データ要素で選択されたスキーマです。 左側のツリーでプロパティの 1 つをクリックし、右側の値を変更することで、オブジェクトの 1 つ以上のプロパティを設定できます。例えば、次のスクリーンショットでは、producedBy プロパティを、「Produced by data element」というデータ要素に設定しています。

![](assets/update-variable-set-property.png)

変数の更新アクションのエディターと XDM オブジェクトデータ要素のエディターにはいくつかの違いがあります。 まず、変数の更新アクションには、「xdm」というラベルの付いたルートレベル項目があります。 この項目をクリックすると、オブジェクト全体の設定に使用するデータ要素を指定できます。 次に、変数の更新アクションには、xdm オブジェクトからデータを消去するチェックボックスがあります。 左側のプロパティの 1 つをクリックし、右側のチェックボックスをオンにして値をクリアします。 これにより、変数の値を設定する前に現在の値がクリアされます。

## メディアイベントを送信 {#send-media-event}

メディアイベントをAdobe Experience PlatformまたはAdobe Analytics（あるいはその両方）に送信します。 このアクションは、web サイト上のメディアイベントを追跡する場合に役立ちます。 インスタンスを選択します（複数のインスタンスがある場合）。 このアクションには、トラッキングされるメディアセッションの一意の ID を表す `playerId` が必要です。 また、メディアセッションを開始する際には、**[!UICONTROL エクスペリエンスの品質]** と `playhead` データ要素も必要です。

![ メディアイベント送信画面を示す Platform UI 画像。](assets/send-media-event.png)

**[!UICONTROL メディアイベントを送信]** アクションタイプでは、次のプロパティをサポートしています。

* **[!UICONTROL インスタンス]**：使用されている Web SDK インスタンス。
* **[!UICONTROL メディアイベントタイプ]**：追跡するメディアイベントのタイプ。
* **[!UICONTROL プレーヤー ID]**：メディアセッションの一意の ID。
* **[!UICONTROL 再生ヘッド]**：メディア再生の現在の位置（秒単位）。
* **[!UICONTROL メディアセッションの詳細]**：メディア開始イベントを送信する場合、必要なメディアセッションの詳細を指定する必要があります。
* **[!UICONTROL チャプターの詳細]**：このセクションでは、チャプター開始メディアイベントを送信する際のチャプターの詳細を指定できます。
* **[!UICONTROL Advertisingの詳細]**: `AdBreakStart` イベントを送信する場合、必要な広告の詳細を指定する必要があります。
* **[!UICONTROL Advertising ポッドの詳細]**:`AdStart` イベントを送信する際の広告ポッドに関する詳細。
* **[!UICONTROL エラーの詳細]**：トラッキングされている再生エラーに関する詳細。
* **[!UICONTROL 状態更新の詳細]**：更新されているプレーヤーの状態。
* **[!UICONTROL カスタムメタデータ]**：追跡するメディアイベントに関するカスタムメタデータです。
* **[!UICONTROL エクスペリエンスの品質]**：トラッキングされるエクスペリエンスデータのメディア品質。

## Media Analytics トラッカーを取得 {#get-media-analytics-tracker}

このアクションは、従来の Media Analytics API を取得するために使用されます。 アクションを設定してオブジェクト名が指定されると、従来の Media Analytics API がそのウィンドウオブジェクトに書き出されます。 何も指定されない場合、現在の Media JS ライブラリと同様に `window.Media` に書き出されます。

![Media Analytics トラッカーアクションタイプの取得を示す Platform UI 画像。](assets/get-media-analytics-tracker.png)

## ID でリダイレクト {#redirect-with-identity}

このアクションタイプを使用して、現在のページの ID を他のドメインに共有します。 このアクションは、**[!UICONTROL クリック]** イベントタイプおよび値比較条件で使用するように設計されています。 このアクションタイプの使用方法について詳しくは、[Web SDK 拡張機能を使用した URL への ID の追加 ](../../../../web-sdk/commands/appendidentitytourl.md#extension) を参照してください。

## 次の手順 {#next-steps}

この記事を読むことで、アクションの設定方法に関する理解を深めることができました。 次に、[ データ要素タイプの設定 ](data-element-types.md) 方法について説明します。

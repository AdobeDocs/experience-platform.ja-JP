---
title: イベントを送信
description: Adobe Experience Platform Edge Networkにデータを送信します。
source-git-commit: d6aea91d6989775ff5b6038b216ed2518f4a7d98
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 0%

---

# イベントを送信

**[!UICONTROL Send event]** アクションは、Adobe Experience Platform Edge Network上のデータストリームにペイロードを送信します。 これは、データ収集とパーソナライゼーションのコア機能です。ほとんどすべての組織は、このアクションを Web SDK実装の一環として使用しています。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Send event]** に設定します。

## 一般フィールド

![ 「イベントを送信」アクションタイプのインスタンス設定を示すExperience Platform タグ UI 画像。](../assets/instance-settings.png)

* **[!UICONTROL Instance]**：アクションが適用されるSDK インスタンス。 実装で 1 つのSDK インスタンスを使用している場合、このドロップダウンメニューは無効になります。
* **[!UICONTROL Use guided events]**：特定のユースケースを有効にするために、特定のフィールドを自動的に入力または非表示にするには、このオプションを有効にします。 この設定は、それぞれの目的に合わせてアクションを設定する際に、使用可能なオプションのノイズを軽減するのに役立ち、Adobeの [ 上位/下位のページイベント ](/help/collection/use-cases/personalization/top-bottom-page-events.md) に関するベストプラクティスに従います。 このチェックボックスをオンにすると、次のラジオボタンの表示がトリガーされます。
   * **[!UICONTROL Request personalization]**:Adobe Analytics イベントを記録せずに、パーソナライゼーションに関する最新の決定を取得します。 最も一般的には、ページの上部で呼び出されます。 このラジオボタンを選択すると、次のフィールドが設定されます。
      * [!UICONTROL Type] は [!UICONTROL Decisioning Proposition Fetch] にロックされています
      * [!UICONTROL Render visual personalization decisions] は有効にロックされています
      * [!UICONTROL Automatically send a display event] は無効にロックされています
   * **[!UICONTROL Collect analytics]**：パーソナライゼーションに関する決定を取得せずにイベントを記録します。 最も一般的には、ページの下部で呼び出されます。 このラジオボタンを選択すると、次のフィールドが設定されます。
      * [!UICONTROL Include rendered propositions] は有効にロックされています

## データフィールド

![ 「イベントを送信」アクションタイプのデータ要素設定を示すExperience Platform タグ UI 画像。](../assets/data.png)

* **[!UICONTROL Type]**：イベントタイプ。 事前に定義された値のセットから選択するか、独自の値を定義できます。 詳しくは、[ 使用可能な値 `eventType`](/help/xdm/classes/experienceevent.md#accepted-values-for-eventtype) を参照してください。 このフィールドと同等のJavaScript ライブラリは [`eventType`](/help/collection/js/commands/sendevent/eventtype.md) です。
* **[!UICONTROL XDM]**:Adobeに送信する XDM ペイロード。 このフィールドでは、[XDM オブジェクト ](../data-element-types.md#xdm-object) または [ 変数 ](../data-element-types.md#variable) を使用できます。 複数の XDM オブジェクトにデータを入力するルールがある場合は、[ 結合オブジェクト ](../../core/overview.md#merged-objects) を使用してそれらを組み合わせることができます。
* **[!UICONTROL Data]**:Adobeに送信するデータペイロード。 一部のアプリやサービスは、Adobe AnalyticsやAdobe Targetなどの XDM スキーマに準拠する必要がありません。 このフィールドには [ 変数 ](../data-element-types.md#variable) データ要素タイプを使用します。
* **[!UICONTROL Include rendered propositions]**：このチェックボックスを有効にすると、このイベントを表示イベントとして使用できるようになります。これには、「表示イベントを自動的に送信」がオフの場合にレンダリングされる提案も含まれます。 `_experience.decisioning` XDM フィールドには、レンダリングされたパーソナライゼーションに関する情報が入力されます。
* **[!UICONTROL Document will unload]**：ユーザーがページから移動した場合でも、イベントがサーバーに到達するようにするには、このチェックボックスを有効にします。 この設定を使用すると、イベントがサーバーに到達できるようになりますが、Edge Networkからの応答は無視されます。
* **[!UICONTROL Merge ID]** _（非推奨）_:`eventMergeId` XDM フィールドに入力します。

## パーソナライゼーションフィールド

![ 「イベントを送信」アクションタイプのPersonalization設定を示すExperience Platform タグ UI 画像。](../assets/personalization-settings.png)

* **[!UICONTROL Scopes]**：パーソナライゼーションから明示的にリクエストする範囲の配列。 範囲を手動で入力することも、データ要素を指定することもできます。 範囲を手動で入力すると、各フィールドは 1 つの範囲を表します。 アクションに範囲を追加するには、「**[!UICONTROL Add scope]**」を選択します。
* **[!UICONTROL Surfaces]**: イベントでクエリするサーフェスの配列。 詳しくは、Adobe Journey Optimizer ドキュメントの [Web エクスペリエンスの作成 ](https://experienceleague.adobe.com/docs/journey-optimizer/using/web/create-web.html) を参照してください。 サーフェスを手動で入力すると、各フィールドは 1 つのサーフェスを表します。 **[!UICONTROL Add surface]** を選択して、アクションにさらにサーフェスを追加します。
* **ビジュアルパーソナライゼーションの決定をレンダリング：** 有効にすると、ページ上のパーソナライズされたコンテンツをレンダリングできるチェックボックス。 詳しくは、[ パーソナライズされたコンテンツのレンダリング ](/help/collection/use-cases/personalization/rendering-personalization-content.md#automatically-rendering-content) を参照してください。
* **[!UICONTROL Request default personalization]**: ページ全体の範囲とデフォルトサーフェスをリクエストするかどうかを制御します。 デフォルトでは、ページ読み込みの最初の `sendEvent` 呼び出し時に自動的に要求されます。 これらのラジオボタンに相当するJavaScript ライブラリは [`requestDefaultPersonalization`](/help/collection/js/commands/sendevent/personalization.md) です。 次のオプションから選択できます。
   * **[!UICONTROL Automatic]**：デフォルトの動作です。 まだリクエストされていない場合にのみ、デフォルトのパーソナライゼーションをリクエストします。
   * **[!UICONTROL Enabled]**：ページ範囲とデフォルトサーフェスを明示的にリクエストします。 これにより、SPA ビューのキャッシュが更新されます。
   * **[!UICONTROL Disabled]**：ページ範囲とデフォルトサーフェスのリクエストを明示的に抑制します。
* **[!UICONTROL Decision context]**：オンデバイス判定のAdobe Journey Optimizer ルールセットを評価する際に使用されるキー値マップ。 決定コンテキストは手動で、またはデータ要素を通じて提供できます。

## Advertising フィールド

![ イベントを送信アクションの広告の設定を示すExperience Platform タグ UI](../assets/send-event-advertising.png)

* **[!UICONTROL Request default advertising data]**: ライブラリが広告情報を XDM ペイロードに追加するタイミング（または追加する場合）を決定します。 次のオプションから選択できます。
   * **[!UICONTROL Automatic]**：イベントの発生時に使用可能なすべての広告データがイベントペイロードに追加されます。
   * **[!UICONTROL Wait]**：広告データを受信するまでイベントの送信を遅延します。
   * **[!UICONTROL Disabled]**：イベントペイロードに広告データを追加しないでください。 実装でAdobe AnalyticsまたはCustomer Journey Analyticsを使用しない場合は、このオプションを選択します。

## データストリーム設定の上書き

このコマンドは、データストリーム設定の上書きをサポートし、このデータを受信するアプリとサービスを制御できます。 個々のコマンドとタグ拡張機能設定内の両方でデータストリーム設定の上書きを設定した場合、個々のコマンドが優先されます。 詳しくは、[ データストリーム設定の上書き ](../configure/configuration-overrides.md) を参照してください。

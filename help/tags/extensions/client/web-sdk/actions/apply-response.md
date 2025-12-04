---
title: 応答を適用
description: Edge Networkからの応答に基づいてアクションを実行します。
source-git-commit: f87e6a0e969aa0924656cdb2ea56aa79d2d7c841
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# 応答を適用

「**[!UICONTROL Apply response]**」アクションタイプを使用すると、Edge Networkからの応答に基づいて様々なアクションを実行できます。 このアクションタイプは、通常、サーバーがEdge Networkに対して最初の呼び出しを行い、その呼び出しから応答を受け取ってブラウザーで web SDKを初期化するハイブリッドデプロイメントで使用されます。 このアクションタイプを使用すると、ハイブリッドパーソナライゼーションのユースケースでクライアントの読み込み時間を短縮できます。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]** に移動して、目的のルールを選択します。
1. 「[!UICONTROL Actions]」で、既存のアクションを選択するか、アクションを作成します。
1. [!UICONTROL Extension] ドロップダウンフィールドを **[!UICONTROL Adobe Experience Platform Web SDK]** に設定し、[!UICONTROL Action type] を **[!UICONTROL Apply response]** に設定します。

![ 「応答を適用」アクションタイプを示すExperience Platform ユーザーインターフェイスの画像 ](../assets/apply-response.png)

## ユースケース

* **データ収集とパーソナライゼーションの手動分割**: レンダリング決定を [ に設定した ](send-event.md) イベントを送信 `false`トリガーを実行し、「イベントを完了」ルールでプロミスをキャッチすることができます。 このルール内の最初のアクションは、「応答を適用」です。 このワークフローを使用すると、組織自身のコードが他の作業を完了するまで DOM 操作を遅延できます。
* **Web SDKの外部から受信したEdgeの応答**：別のライブラリを使用してEdge Networkと通信する場合、このアクションを使用して、Web SDKがEdge Networkからの応答を引き続き処理できるようにします。

## 使用可能なフィールド

このアクションタイプは、次の設定オプションをサポートしています。

* **[!UICONTROL Instance]**：アクションが適用されるSDK インスタンス。 実装で 1 つのSDK インスタンスを使用している場合、このドロップダウンメニューは無効になります。
* **[!UICONTROL Response headers]**:Edge Network サーバーコールから返されたヘッダーキーと値を含むオブジェクトを返すデータ要素を選択します。
* **[!UICONTROL Response body]**:Edge Network レスポンスによって提供される JSON ペイロードを含むオブジェクトを返すデータ要素を選択します。
* **[!UICONTROL Render visual personalization decisions]**:Edge Networkが提供するパーソナライゼーションコンテンツを自動的にレンダリングし、コンテンツをあらかじめ非表示にしてちらつきを防ぐには、このオプションを有効にします。

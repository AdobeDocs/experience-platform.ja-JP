---
title: メタピクセル拡張機能の概要
description: Adobe Experience Platformの Meta Pixel タグ拡張について説明します。
source-git-commit: 87376172f89858bfa883084461544a2c50ba5009
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 2%

---

# [!DNL Meta Pixel] 拡張機能の概要

[[!DNL Meta Pixel]](https://developers.facebook.com/docs/meta-pixel/) は、Web サイト上の訪問者のアクティビティを追跡するための、JavaScript ベースの分析ツールです。 追跡する訪問者のアクション（コンバージョンと呼ばれる）が、に送信されます。 [[!DNL Ads Manager]](https://www.facebook.com/business/tools/ads-manager) 広告やキャンペーン、コンバージョンファネルなどの効果の測定に使用できます。

この [!DNL Meta Pixel] タグ拡張機能を使用すると、 [!DNL Pixel] の機能は、クライアント側のタグライブラリで使用できます。 このドキュメントでは、拡張機能をインストールし、 [ルール](../../../ui/managing-resources/rules.md).

<!-- (To include when Conversions API extension doc is published)
>[!NOTE]
>
>If you are trying to send server-side events to [!DNL Meta] rather than from the client side, use the [[!DNL Meta Conversions API] extension](../../server/meta/overview.md) instead.
-->

## 前提条件

拡張機能を使用するには、有効な [!DNL Meta] ～を利用できる口座 [!DNL Ads Manager]. 特に、 [新しい [!DNL Meta Pixel]](https://www.facebook.com/business/help/952192354843755) をコピーします。 [!DNL Pixel ID] そのため、拡張機能はアカウントに設定できます。 既に [!DNL Meta Pixel]の場合は、代わりに ID を使用できます。

## 拡張機能のインストール

をインストールするには、以下を実行します。 [!DNL Meta Pixel] 拡張機能に移動し、データ収集 UI またはExperience PlatformUI に移動して、「 」を選択します。 **[!UICONTROL タグ]** をクリックします。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、「 」を選択します。 **[!UICONTROL 拡張機能]** 左側のナビゲーションで、 **[!UICONTROL カタログ]** タブをクリックします。 を検索します。 [!UICONTROL メタピクセル] カードを選択し、 **[!UICONTROL インストール]**.

![この [!UICONTROL インストール] ボタンを選択しています [!UICONTROL メタピクセル] 拡張機能を使用して、データ収集 UI に追加できます。](../../../images/extensions/client/meta/install.png)

表示される設定ビューで、 [!DNL Pixel] 拡張機能をアカウントにリンクするために以前にコピーした ID。 ID を直接入力に貼り付けることも、代わりに既存のデータ要素を選択することもできます。

>[!TIP]
>
>データ要素を使用すると、 [!DNL Pixel] ビルド環境など、他の要因に応じて使用される ID。 付録の [異なる [!DNL Pixel] 異なる環境の ID](#id-data-element) を参照してください。

オプションで、拡張機能に関連付けるイベント ID を指定することもできます。 これは、 [!DNL Meta Pixel] そして [!DNL Meta Conversions API]. 詳しくは、 [!DNL Meta] ドキュメント [重複の処理 [!DNL Pixel] および [!DNL Conversions API] イベント](https://developers.facebook.com/docs/marketing-api/conversions-api/deduplicate-pixel-and-server-events/) 」を参照してください。

終了したら、「 」を選択します。 **[!UICONTROL 保存]**

![この [!DNL Pixel] 拡張機能の設定表示でデータ要素として指定された ID。](../../../images/extensions/client/meta/configure.png)

拡張機能がインストールされ、タグルールで様々なアクションを使用できるようになりました。

## タグルールの設定 {#rule}

[!DNL Meta Pixel] は、事前定義済みの一連のを受け入れます [標準イベント](https://www.facebook.com/business/help/402791146561655)それぞれに独自のコンテキストと受け入れ可能なプロパティが含まれます。 で指定されるルールアクション [!DNL Pixel] 拡張機能はこれらのイベントタイプに関連付けられ、送信されるイベントを簡単に分類して設定できます。 [!DNL Meta] タイプに応じて

デモ用に、この節では、ページビューイベントをに送信するルールの作成方法を示します。 [!DNL Meta].

新しいタグルールの作成を開始し、必要に応じてその条件を設定します。 ルールのアクションを選択する場合は、「 **[!UICONTROL メタピクセル]** 拡張機能に対して、「 」を選択します。 **[!UICONTROL ページビューを送信]** （アクションタイプ）。

![この [!UICONTROL ページビューを送信] データ収集 UI のルールに対して選択されているアクションタイプ。](../../../images/extensions/client/meta/select-action.png)

これ以上の設定は必要ありません。 [!UICONTROL ページビューを送信] アクション。 選択 **[!UICONTROL 変更を保持]** をクリックして、ルール設定にアクションを追加します。 ルールに問題がない場合は、「 」を選択します。 **[!UICONTROL ライブラリに保存]**.

最後に、新しいタグを公開します。 [ビルド](../../../ui/publishing/builds.md) ライブラリへの変更を有効にします。

## 確認 [!DNL Meta] はデータを受信しています

更新されたビルドを Web サイトにデプロイしたら、ブラウザーでコンバージョンイベントを生成し、それらのイベントがに表示されるかどうかを確認することで、データが期待どおりに送信されているかどうかを確認できます。 [[!DNL Meta Events Manager]](https://www.facebook.com/business/help/898185560232180).

## 次の手順

このガイドでは、にデータを送信する方法について説明しました [!DNL Meta] の使用 [!DNL Meta Pixel] タグ拡張。 Experience Platformのタグについて詳しくは、 [タグの概要](../../../home.md).

## 付録：別の [!DNL Pixel] 異なる環境の ID {#id-data-element}

実稼動環境を維持しながら、開発環境またはステージング環境で実装をテストする場合 [!DNL Meta Pixel] analytics を無効にした場合は、データ要素を使用して、適切な [!DNL Pixel] 使用されている環境に応じた ID。

これをおこなうには、 [!UICONTROL カスタムコード] データ要素 ( [[!UICONTROL コア] 拡張](../core/overview.md)) と [`turbine` 自由変数](../../../extension-dev/turbine.md). データ要素の JavaScript コードで、 `turbine` オブジェクトを使用して現在の環境ステージを検索し、適切な [!DNL Pixel] 結果に基づく ID。

次の例では、架空の実稼動 ID を返します `exampleProductionKey` （実稼動環境で使用される場合）と、異なる ID `exampleTestKey` （他の環境が使用される場合） このコードを実装する場合は、各値を実際の実稼動環境およびテスト環境に置き換えます [!DNL Pixel] ID。

```js
return (turbine.environment.stage === "production" ? 'exampleProductionKey' : 'exampleTestKey');
```


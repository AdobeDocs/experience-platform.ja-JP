---
title: Meta Pixel 拡張機能の概要
description: Adobe Experience Platformのメタピクセルタグ拡張機能について説明します。
exl-id: c5127bbc-6fe7-438f-99f1-6efdbe7d092e
source-git-commit: 24001da61306a00d295bf9441c55041e20f488c0
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 0%

---

# [!DNL Meta Pixel] 拡張機能の概要

[[!DNL Meta Pixel]](https://developers.facebook.com/docs/meta-pixel/) は、web サイト上の訪問者のアクティビティを追跡できる、JavaScript ベースの分析ツールです。 追跡した訪問者アクション（コンバージョンと呼ばれます）は [[!DNL Ads Manager]](https://www.facebook.com/business/tools/ads-manager) に送信され、広告、キャンペーン、コンバージョンファネルなどの有効性を測定するために使用できます。

[!DNL Meta Pixel] タグ拡張機能を使用すると、クライアントサイドのタグライブラリで [!DNL Pixel] の機能を活用できます。 このドキュメントでは、拡張機能をインストールし、その機能を [ ルール ](../../../ui/managing-resources/rules.md) で使用する方法について説明します。

## 前提条件

拡張機能を使用するには、[!DNL Ads Manager] へのアクセス権を持つ有効な [!DNL Meta] アカウントが必要です。 特に、アカウントに拡張機能を設定できるように、[ 新しく作成  [!DNL Meta Pixel]](https://www.facebook.com/business/help/952192354843755) してその [!DNL Pixel ID] をコピーする必要があります。 既存の [!DNL Meta Pixel] がある場合は、その ID を代わりに使用できます。

[!DNL Meta Pixel] と [!DNL Meta Conversions API] を組み合わせて使用し、クライアント側とサーバー側のそれぞれから同じイベントを共有して送信することを強くお勧めします。[!DNL Meta Pixel] で取得されなかったイベントの回復に役立つ可能性があるからです。 サーバーサイド実装に統合する手順については、[[!DNL Meta Conversions API]  イベント転送用の拡張機能 ](../../client/meta/overview.md) に関するガイドを参照してください。 サーバーサイド拡張機能を使用するには、組織が [ イベント転送 ](../../../ui/event-forwarding/overview.md) にアクセスできる必要があります。

## 拡張機能のインストール

[!DNL Meta Pixel] 拡張機能をインストールするには、データ収集 UI またはExperience PlatformUI に移動し、左側のナビゲーションから **[!UICONTROL タグ]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 [!UICONTROL Meta Pixel] カードを検索し、「**[!UICONTROL インストール]**」を選択します。

![ データ収集 UI の [!UICONTROL  メタピクセル ] 拡張機能に対して選択されている「[!UICONTROL  インストール ]」ボタン。](../../../images/extensions/client/meta/install.png)

表示される設定ビューで、拡張機能をアカウントにリンクするには、以前にコピーした [!DNL Pixel] ID を指定する必要があります。 ID を入力に直接貼り付けることも、代わりに既存のデータ要素を選択することもできます。

>[!TIP]
>
>データ要素を使用すると、ビルド環境などの他の要因に応じて、使用する [!DNL Pixel] ID を動的に変更できます。 詳しくは、[ 異なる環境での異なる  [!DNL Pixel] ID の使用 ](#id-data-element) に関する付録の節を参照してください。

オプションで、拡張機能に関連付けるイベント ID を指定することもできます。 これは、[!DNL Meta Pixel] と [!DNL Meta Conversions API] の間で同一のイベントの重複を排除するために使用されます。 詳しくは、[!DNL Conversions API] 拡張機能の概要の [ イベントの重複排除 ](../../server/meta/overview.md#event-deduplication) の節を参照してください。

終了したら「**[!UICONTROL 保存]**」を選択します

![ 拡張機能の設定ビューでデータ要素として提供された [!DNL Pixel] ID。](../../../images/extensions/client/meta/configure.png)

拡張機能がインストールされ、タグルールで様々なアクションを使用できるようになりました。

## タグルールの設定 {#rule}

[!DNL Meta Pixel] れぞれ独自のコンテキストと受け入れられるプロパティを持つ、事前定義済みの [ 標準イベント ](https://www.facebook.com/business/help/402791146561655) のセットを受け入れます。 [!DNL Pixel] 拡張機能が提供するルールアクションは、これらのイベントタイプに関連付けられます。これにより、タイプに従って [!DNL Meta] に送信するイベントを簡単に分類および設定できます。

この節では、デモンストレーションを目的として、ページビューイベントを [!DNL Meta] に送信するルールの作成方法を説明します。

新しいタグルールの作成を開始し、必要に応じてその条件を設定します。 ルールのアクションを選択する場合、拡張機能で **[!UICONTROL メタピクセル]** を選択してから、アクションタイプで **[!UICONTROL ページビューを送信]** を選択します。

![ データ収集 UI でルールに対して選択されている [!UICONTROL  ページビューを送信 ] アクションタイプ ](../../../images/extensions/client/meta/select-action.png)

[!UICONTROL  ページビューを送信 ] アクションを実行するために必要な追加の設定はありません。 「**[!UICONTROL 変更を保持]**」を選択して、ルール設定にアクションを追加します。 ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

最後に、新しいタグ [build](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## データを受信 [!DNL Meta] ていることを確認します。

更新したビルドが web サイトにデプロイされたら、ブラウザーでコンバージョンイベントを生成し、そのイベントが [[!DNL Meta Events Manager]](https://www.facebook.com/business/help/898185560232180) に表示されるかどうかを確認することで、データが期待どおりに送信されているかどうかを確認できます。

## 次の手順

このガイドでは、[!DNL Meta Pixel] タグ拡張機能を使用して [!DNL Meta] にデータを送信する方法について説明しました。 サーバーサイドのイベントも [!DNL Meta] に送信することを計画している場合は、[[!DNL Conversions API]  イベント転送拡張機能 ](../../server/meta/overview.md) のインストールと設定に進むことができます。

Experience Platformのタグについて詳しくは、[ タグの概要 ](../../../home.md) を参照してください。

## 付録：環境ごとに異なる [!DNL Pixel] ID を使用する {#id-data-element}

実稼動環境と分析を変えないまま、開発環境またはステージング環境で実装をテストす [!DNL Meta Pixel] 場合は、データ要素を使用すると、使用する環境に応じて適切な [!DNL Pixel] ID を動的に選択できます。

これを実現するには、[!UICONTROL  カスタムコード ] データ要素（[[!UICONTROL  コア ] 拡張機能 ](../core/overview.md) が提供）を [`turbine` の自由変数 ](../../../extension-dev/turbine.md) と組み合わせて使用します。 データ要素のJavaScript コードで、`turbine` オブジェクトを使用して現在の環境ステージを見つけ、結果に基づいて適切な [!DNL Pixel] ID を返します。

次の例では、実稼動環境で使用された場合は偽の実稼動 ID `exampleProductionKey` を返し、他の環境が使用された場合は別の ID `exampleTestKey` を返します。 このコードを実装する際には、それぞれの値を実際の実稼動環境の値に置き換えて、[!DNL Pixel] ID をテストします。

```js
return (turbine.environment.stage === "production" ? 'exampleProductionKey' : 'exampleTestKey');
```

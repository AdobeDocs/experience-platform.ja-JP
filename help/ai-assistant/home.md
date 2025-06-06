---
title: Adobe Experience Platformの AI アシスタントの概要
description: AI アシスタント、そのニュアンスとユースケース、およびAdobe Experience PlatformとReal-Time Customer Data Platformを使用してワークフローを迅速化する方法について説明します。
exl-id: cfd4ac22-fff3-4b50-bbc2-85b6328f603c
source-git-commit: e90333d09585c8aa0ef176dcfc4717e86364fd54
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 6%

---

# Adobe Experience Platformの AI アシスタント

次のビデオは、AI アシスタントについて理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/3429845?learn=on)

Adobe Experience Platformの AI アシスタントについては、このドキュメントを参照してください。

Adobe Experience Platformの AI アシスタントは、Adobe アプリケーションでのワークフローの高速化に使用できる対話型エクスペリエンスです。 AI アシスタントを使用すると、製品の知識をより深く理解したり、問題をトラブルシューティングしたり、情報を検索して運用インサイトを見つけたりできます。 AI アシスタントは、Experience Platform、Real-Time Customer Data Platform、Adobe Journey Optimizer、Customer Journey Analyticsをサポートします。

![ 初めてユーザーエクスペリエンスがトリガーされる AI アシスタント インターフェイス ](./images/ai-assistant-full.png)

>[!IMPORTANT]
>
>AI アシスタントを使用するには、[ ユーザー使用許諾契約 ](https://www.adobe.com/jp/legal/licenses-terms/adobe-dx-gen-ai-user-guidelines.html) に同意する必要があります。 ユーザー契約には、パブリックベータ版の契約も含まれています。 これにより、ベータ版の容量で展開する際に、追加の AI アシスタント機能を使用できます。

+++選択してユーザー契約インターフェイスを表示します

![ ユーザー使用許諾契約の最初のページ ](./images/user-agreement-1.png)

![ ユーザー契約書の最後のページ ](./images/user-agreement-2.png)

+++

## AI アシスタントについて {#understanding-ai-assistant}

AI アシスタントは、データベースにクエリを実行し、データベースのデータを人間が読み取れる回答に変換することで、送信された質問に応答します。

基になるデータのこの内部表現は、**[!DNL Knowledge Graph]** とも呼ばれ、特定の回答の概念、データ、メタデータの包括的な Web です。

[!DNL Knowledge Graph] は、クエリが送信されるたびに参照されるサブグラフで構成されます。

* 顧客の運用インサイト。
* 様々なメタストアにわたる顧客の運用インサイト。
* Experience Leagueのドキュメント。

AI アシスタントをクエリする前に考慮すべき質問には、次の 2 つのクラスがあります。

### 製品に関する知識 {#product-knowledge}

製品に関する知識とは、Experience Leagueのドキュメントに基づいた概念やトピックを指します。 製品に関する知識の質問は、次のサブグループでさらに指定できます。

| 製品に関する知識 | 例 |
| --- | --- |
| 先を見越した学習 | <ul><li>ID とプライマリキーまたは外部キーの違いは何ですか。</li><li>類似オーディエンスとは</li></ul> |
| 検出を開く | <ul><li>このデータセットを書き出すにはどうすればよいですか？</li><li>ヘルスケア関連の顧客向けのスキーマはありますか？</li></ul> |
| トラブルシューティング | <ul><li>Adobeが所有するプロファイルのスキーマを有効にできないのはなぜですか？</li><li>セグメントを削除できないのはなぜですか？</li></ul> |

{style="table-layout:auto"}

AI アシスタントの製品に関する知識について詳しくは、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3438032/?learn=on)

### 運用上のインサイト {#operational-insights}

運用インサイトとは、カウント、ルックアップ、系列の影響を含む、メタデータオブジェクト（属性、オーディエンス、データフロー、データセット、宛先、ジャーニー、スキーマ、ソース）に関して AI Assistant が生成する回答を指します。 サンドボックス内のデータは参照されません。

* データセットはいくつ持っていますか？
* 使用されたことのないスキーマ属性の数。
* アクティブ化されているオーディエンスはどれですか？

次のドメインで、運用に関するインサイトについて AI アシスタントに質問できます。

| ドメイン | サポートされるメタデータ | サポートされていないメタデータ |
| --- | --- | --- |
| 属性 | <ul><li>属性名の検索</li><li>属性 – スキーマの関係</li><li>属性 – データセットの関係</li><li>属性 – オーディエンスの関係</li><li>属性 – 宛先の関係</li></ul> | <ul><li>属性クラス</li><li>監査</li><li>非推奨ステータス</li><li>ラベル</li><li>属性に格納される値</li></ul> |
| オーディエンス | <ul><li>オーディエンス数</li><li>オーディエンスタイプ（ストリーミングまたはバッチ）</li><li>作成日/変更日</li><li>アクティベーションステータス</li><li>プロファイル数</li><li>オーディエンスを複製</li><li>オーディエンス定義検索</li><li>オーディエンス – オーディエンスの関係</li><li>オーディエンス – 属性関係</li><li>オーディエンス – データセット関係</li><li>オーディエンス – 宛先関係</li><li>名前検索</li><li>名前と ID 検索 | <ul><li>オーディエンスの重複</li><li>Audience Activation</li><li>オーディエンス – キャンペーン関係</li><li>監査</li><li>作成/変更</li><li>ラベル</li><li>プロファイル選定トレンド</li></ul> |
| データフロー | <ul><li>データフロー数</li><li>データフローステータス</li><li>データフロー – データセットの関係</li><li>データフロー – ソースの関係</li></ul> | <ul><li>作成/変更</li><li>データフローバッチ関係</li><li>プロファイル数の取り込み</li></ul> |
| データセット | <ul><li>データセット数</li><li>プロファイル有効化ステータス</li><li>作成日/変更日</li><li>データセット – スキーマの関係</li><li>データセット – オーディエンスの関係</li><li>データセット – 属性関係</li><li>データセット – データフロー関係</li><li>名前検索 </li><li>名前と ID 検索</li></ul> | <ul><li>監査</li><li>作成者</li><li>データセット – バッチ関係</li><li>データセットの作成/変更</li><li>データセットサイズ</li><li>プロファイル数</li><li>行数</li><li>値検索</li></ul> |
| 宛先 | <ul><li>設定済みの宛先数</li><li>宛先 – オーディエンスの関係</li><li>宛先属性の関係</li></ul> | <ul><li>アカウントの設定</li><li>アカウント資格情報</li><li>一意のプロファイルがアクティブ化されました</li></ul> |
| ジャーニー | <ul><li>カウント</li><li>名前検索</li><li>名前と ID 検索</li><li>ジャーニーステータス</li><li>トリガーステータス（オーディエンスとイベント）</li><li>作成日/変更日</li><li>繰り返し頻度</li></ul> | <ul><li>属性 – ジャーニーの関係</li><li>監査</li><li>作成/変更</li><li>作成者</li><li>イベント</li><li>ジャーニー - データセット</li><li>ジャーニー - スキーマ</li><li>オファー</li><li>プロファイル選定トレンド</li><li>ステップイベント</li></ul> |
| スキーマ | <ul><li>スキーマ数</li><li>作成日/変更日</li><li>スキーマ – 属性関係</li><li>スキーマ – データセットの関係</li><li>スキーマ – オーディエンスの関係</li><li>プロファイル有効化ステータス</li><li>名前検索</li><li>名前と ID 検索</li></ul> | <ul><li>監査</li><li>作成/変更</li><li>作成者</li><li>フィールドグループ</li><li>ID</li><li>ID 名前空間</li><li>ラベル</li><li>プロファイル数</li></ul> |
| ソース | <ul><li>アカウント数</li><li>アカウントステータス</li><li>各アカウントのアクティブ/非アクティブなデータフロー</li><li>Source コネクタ – データフローの関係</li><li>Source アカウント – データフローの関係</li></ul> | <ul><li>アカウント資格情報</li><li>アカウントの設定</li><li>データ取得指標</li><li>プロファイル数</li><li>Source - バッチ関係</li></ul> |

{style="table-layout:auto"}

オペレーショナルインサイトの質問については、回答が現在の UI の状態を反映していない場合があります。 これらの質問を裏付けるデータは、24 時間ごとに 1 回更新されます。 例えば、Real-Time CDPで日中に行った変更内容は、夜間にデータストアに同期され、午前中にユーザーからの質問に使用できるようになります。 オブジェクトに関連する特定のデータを照会するには、サンドボックスにログインする必要があります。

AI アシスタントの操作インサイトについて詳しくは、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3444033?learn=on&enablevpops&captions=jpn)

### 機能の範囲 {#feature-scope}

現在、AI アシスタントの範囲は次のとおりです。

* [ 製品ナレッジ ](./home.md#product-knowledge):AI アシスタントは、Experience Platform、Real-Time Customer Data Platform、Adobe Journey Optimizerの製品ナレッジに関する質問に回答できます。 また、Customer Journey Analyticsの UI を通じてのみ、Customer Journey Analyticsの製品知識のトピックを掘り下げることもできます。
* [ 運用インサイト ](./home.md#operational-insights)：属性、オーディエンス、データフロー、データセット、宛先、ジャーニー、スキーマおよびソースのデータオブジェクトに関する運用インサイトについて、AI アシスタントに質問できます。

## 次の手順

これで、AI アシスタントの概要を理解できたので、ワークフローの実行中に AI アシスタントを続行して使用できます。 詳しくは、次のドキュメントを参照してください。

* [AI アシスタント UI ガイド](./ui-guide.md)
* [機能へのアクセス](./access.md)
* [質問ガイド](./questions.md)
* [AI アシスタントでのプライバシー、セキュリティ、ガバナンス](./privacy.md)
* [FAQ](./faq.md)

---
keywords: エクスペリエンスプラットフォーム、トラブルシューティング、データサイエンスワークスペース、人気のある話題
solution: Experience Platform
title: Data サイエンス Workspace のトラブルシューティングガイド
topic-legacy: Troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace に関するよくある質問に対する回答を示します。
exl-id: fbc5efdc-f166-4000-bde2-4aa4b0318b38
source-git-commit: ec42d80e695ccf57c10c539ae1b5104c7948c473
workflow-type: tm+mt
source-wordcount: '1470'
ht-degree: 18%

---

# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe エクスペリエンスプラットフォームについてよく寄せられる質問に対する回答を示し [!DNL Data Science Workspace] ます。 一般的な api に関する質問やトラブルシューティングについ [!DNL Platform] ては、『 [ Adobe エクスペリエンス Platform API のトラブルシューティングガイド』を参照してください ](../landing/troubleshooting.md) 。

## JupyterLab のクエリー状態が実行状態のままになります。

一部のメモリ不足の状態では、1つのセルが実行中の状態であることを示すことができます。 例えば、大きなデータセットをクエリーする場合や、複数の2つのクエリーを実行する場合は、dataframe オブジェクトを保存するために使用可能なメモリが不足しています。 このような状況で表示されるインジケーターはいくつかあります。 1つは、セルの横のアイコンで示されているように、セルに「実行中」と表示されているにもかかわらず、カーネルはアイドル状態に [`*`] なります。 さらに、一番下のバーには、使用している RAM の量が表示されます。

![利用可能な ram](./images/jupyterlab/user-guide/allocate-ram.png)

データの読み取り中に、割り当てられたメモリの使用量が大きくなるまでメモリが増大する可能性があります。 最大メモリ容量に達し、カーネルが再起動すると同時に、メモリは解放されます。 これにより、このシナリオで使用されているメモリは、再起動前には、割り当てられた最大 RAM に非常に近くなります。

この問題を解決するには、JupyterLab の右上にある歯車アイコンを選択し、スライドショーのスライダーを右側に移動し、「更新プログラムの追加」を選択して **** RAM を割り当てます。 さらに、複数のクエリーを実行し、RAM 値が最大割り当て量に近づいている場合は、前のクエリーの結果が必要な場合を除き、カーネルを再起動して、使用可能な RAM 容量を再設定します。 これにより、現在のクエリーに使用できる RAM の最大容量が設定されます。

![より多くの ram を割り当てます。](./images/jupyterlab/user-guide/notebook-gpu-config.png)

最大メモリ容量 (RAM) を割り当てる場合に、この問題が発生しても、クエリを修正して、データセットのサイズを小さくすると、列またはデータの範囲が小さくなります。 全データを使用するには、Spark ノートブックを活用することをお勧めします。

## [!DNL JupyterLab] 環境は、に読み込まれていません。 [!DNL Google Chrome]

>[!IMPORTANT]
>
>この問題は解決されましたが、現在も Google Chrome .x には存在しています。 Chrome ブラウザーが最新の状態であることを確認してください。

[!DNL Google Chrome]バージョン80のブラウザーでは、すべてのサードパーティ製 cookie はデフォルトでブロックされています。このポリシーを使用 [!DNL JupyterLab] しても、Adobe エクスペリエンスプラットフォームでの読み込みができない場合があります。

この問題を修正するには、次の手順を実行します。

ブラウザーで [!DNL Chrome] 、右上に移動して「設定」を選択し **** ます (または、address bar に「chrome://settings/」をコピーしてペーストすることもできます)。 次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

![Chrome の詳細設定](./images/faq/chrome-advanced.png)

「**プライバシーとセキュリティ**」セクションが表示されます。次に、「**サイトの設定**」をクリックし、「**Cookie とサイトデータ**」をクリックします。

![Chrome の詳細設定](./images/faq/privacy-security.png)

![Chrome の詳細設定](./images/faq/cookies.png)

最後に、「サードパーティCookieのブロック」を「オフ」に切り替えます。

![Chrome の詳細設定](./images/faq/toggle-off.png)

>[!NOTE]
>
>別の方法として、サードパーティの cookie を無効にして * を追加することもでき [ ます。]ds.adobe.net 許可リストに表示されます。

アドレスバーの「chrome://flags/」に移動します。右側のドロップダウンメニューを使用して、「*SameSite by default cookies*」というフラグを探して無効にします。

![SameSite フラグを無効化](./images/faq/samesite-flag.png)

手順 2 の後、ブラウザーを再起動するように求められます。再起動した後は、 [!DNL Jupyterlab] アクセス可能であることが必要です。

## Safari でアクセスできないのはなぜ [!DNL JupyterLab] ですか?

Safari の初期設定では、サードパーティの cookie は無効化されます。 &lt; 12. [!DNL Jupyter]バーチャルマシンのインスタンスが親フレームとは異なるドメインに存在するため、現在の Adobe エクスペリエンスプラットフォームでは、サードパーティのクッキーが有効になっていることが必要です。サードパーティのクッキーを有効にするか、などの別のブラウザーに切り替えてください [!DNL Google Chrome] 。

Safari 12 については、ユーザーエージェントを「」または「」に切り替える必要があり [!DNL Chrome] [!DNL Firefox] ます。 ユーザーエージェントを切り替えるには、Safari メニューを開き、 ** 「環境設定」を選択し **** ます。 環境設定ウィンドウが表示されます。

![Safari の環境設定](./images/faq/preferences.png)

Safari 設定ダイアログボックスで、「詳細設定」を選択し **** ます。 次に、 *「メニューバーの開発メニューを表示」チェックボックスをオンにし* ます。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari advanced](./images/faq/advanced.png)

次に、トップナビゲーションバーから、開発メニューを選択し **** ます。 **開発** ドロップダウンで、ユーザーエージェント上をマウスでポイント **** します。**[!DNL Chrome]** **[!DNL Firefox]** 使用するユーザーエージェントストリングを選択できます。

![メニューの作成](./images/faq/user-agent.png)

## でファイルをアップロードまたは削除しようとすると、「403禁止] メッセージが表示されるのはなぜ [!DNL JupyterLab] ですか?

または Plus などの広告禁止ソフトウェアがブラウザーに表示されている場合は [!DNL Ghostery] [!DNL AdBlock] 、各広告宣伝ソフトウェアで、ドメイン &quot;\ * adobe.net&quot; [!DNL JupyterLab] を通常どおりに動作させることができなければなりません。 これは [!DNL JupyterLab] 、virtual machines はドメインとは異なるドメイン上で実行されるからです [!DNL Experience Platform] 。

## なぜ一部が [!DNL Jupyter Notebook] スクランブルされたり、コードとして表示されなかったりします。

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## カスタムライブラリをインストールするにはどうすればいい [!DNL Python] ですか?

カーネルは、 [!DNL Python] 多くの一般的な machine ラーニングライブラリとともに事前にインストールされています。 ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

事前にインストールされているライブラリの完全なリストについ [!DNL Python] ては、 [ JupyterLab のユーザーガイドの付録の項を参照してください ](./jupyterlab/overview.md#supported-libraries) 。

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## カーネルのクラスターリソースを設定することはできますか、それとも [!DNL Spark] [!DNL JupyterLab] [!DNL Spark] PySpark ますか?

ノートブックの最初のセルに次のブロックを追加することで、リソースを設定できます。

```python
%%configure -f 
{
    "numExecutors": 10,
    "executorMemory": "8G",
    "executorCores":4,
    "driverMemory":"2G",
    "driverCores":2,
    "conf": {
        "spark.cores.max": "40"
    }
}
```

クラスターリソースの設定について詳しくは [!DNL Spark] 、構成可能なプロパティのリストが含まれて [ ](./jupyterlab/overview.md#kernels) います。

## より大きなデータセットに対して特定のタスクを実行しようとするとエラーが発生するのはなぜですか?

このようなエラーが表示される場合は、 `Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.` 通常、ドライバーまたはエグゼキュータのメモリが不足しています。 [ ](./jupyterlab/access-notebook-data.md) データの制限と、大規模なデータセットでのタスクの実行方法について詳しくは、JupyterLab のマニュアルを参照してください。通常、このエラーは、からからに変更することにより解決でき `mode` `interactive` `batch` ます。

さらに、大きな Spark/PySpark データセットを作成している間は、書き込みコードを実行する前にデータをキャッシュすることで、パフォーマンスを大幅に向上させる `df.cache()` ことができます。

<!-- remove this paragraph at a later date once the sdk is updated -->

データの読み取り中に問題が発生し、データに変形を適用している場合は、変換前にデータをキャッシュしてください。 データをキャッシュすることによって、ネットワーク全体での読み取りが回避されます。 最初に、データを読み取ります。 次に、データをキャッシュに保存 `df.cache()` します。 最後に、変換を実行します。

## データの読み取りと書き込みに Spark/PySpark のノートパソコンが長い時間がかかっているのはなぜですか?

を使用するなど、データに対する変換を実行している場合は、 `fit()` 変換が何度も実行される可能性があります。 パフォーマンスを向上させるには、を実行する前に、を使用してデータをキャッシュし `df.cache()` `fit()` ます。 これにより、変換は1回のみ実行され、ネットワーク全体での読み取りが回避されます。

**推奨注文:** まずデータを読み取ります。 次に、変換を実行してから、データをキャッシュし `df.cache()` ます。 最後に、を実行 `fit()` します。

## Spark/PySpark ノートブックの実行に失敗する理由

以下のいずれかのエラーが表示される場合は、次のいずれかを行います。

- ジョブはステージエラーにより中止されました...各パーティションに同数のエレメントがある場合は、zip RDDs のみを使用できます。
- リモート RPC クライアント、その他のメモリエラー。
- データセットの読み取りと書き込みのパフォーマンスが低下します。

データを書き込む前に、データがキャッシュされていることを確認してください ( `df.cache()` )。 ノートパソコンでコードを実行すると、のよう `df.cache()` なアクションに `fit()` よってノートブックのパフォーマンスが大幅に改善されます。 `df.cache()`データセットを書き込む前にを使用することで、一度だけ実行されるようになります。

## [!DNL Docker Hub] データサイエンスワークスペースの制限の制限

2020年11月20日時点では、Docker Hub の匿名および無料認証の使用に関するレート制限が有効になりました。 匿名ユーザーおよび無料 [!DNL Docker Hub] ユーザーは、6時間ごとに100コンテナイメージのプル要求に制限されます。 これらの変更によって影響を受ける場合は、またはというエラーメッセージが表示され `ERROR: toomanyrequests: Too Many Requests.` `You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits.` ます。

現時点では、この制限が適用されるのは、6時間以内に100ノートブックをレシピに作成している場合、または、頻繁に拡大/縮小されるデータ科学ワークスペース内に Spark ベースのノートブックを使用している場合のみです。 ただし、このようなクラスターは、idling アウトされるまで2時間の間はアクティブなままになるので、これはめったにありません。 これにより、クラスターがアクティブになったときに必要なプル数を減らすことができます。 上記のいずれかのエラーが表示された場合は、 [!DNL Docker] 制限値がリセットされるまで待つ必要があります。

レート制限について詳しくは [!DNL Docker Hub] 、dockerhub のマニュアルを参照してください [ ](https://www.docker.com/increase-rate-limits) 。 この問題を解決するには、その解決策が今後のリリースで期待されるようになりました。

---
keywords: Experience Platform;トラブルシューティング;データサイエンスワークスペース;人気のトピック
solution: Experience Platform
title: データサイエンスワークスペーストラブルシューティングガイド
description: このドキュメントでは、Adobe Experience Platform データサイエンスワークスペースに関するよくある質問に対する回答を示します。
exl-id: fbc5efdc-f166-4000-bde2-4aa4b0318b38
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '1470'
ht-degree: 100%

---

# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Data Science Workspace] に関するよくある質問に対する回答を示します。[!DNL Platform] API 全般に関する質問とトラブルシューティングについては、[Adobe Experience Platform API トラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## JupyterLab Notebook クエリステータスが実行状態でスタックする

メモリ不足の状態でセルが無期限に実行状態にあることを JupyterLab Notebook が示す場合があります。例えば、大規模なデータセットに対してクエリを実行したり、後に続く複数のクエリを実行したりすると、結果のデータフレームオブジェクトを格納するために JupyterLab Notebook が使用可能なメモリを使い果たすことがあります。このような状況で見られる指標がいくつかあります。まず、セルの横にある [`*`] アイコンによってセルが実行中であることが示されていても、カーネルはアイドル状態になります。さらに、下部のバーには、使用された／使用可能な RAM の量が示されます。

![使用可能な RAM](./images/jupyterlab/user-guide/allocate-ram.png)

データの読み取り中に、割り当てられたメモリの最大量に達するまでメモリが増加する可能性があります。最大メモリに達し、カーネルが再起動するとすぐに、メモリが解放されます。つまり、このシナリオで使用されているメモリはカーネルの再起動により非常に少なく表示される可能性がありますが、再起動の直前には、メモリは割り当てられた最大 RAM に非常に近くなっています。

この問題を解決するには、JupyterLab の右上にある歯車アイコンを選択し、スライダーを右にスライドしてから、「**[!UICONTROL 設定を更新]**」を選択して RAM を割り当てます。さらに、複数のクエリを実行していて、RAM の値が、割り当てられた最大量に近づいている場合は、以前のクエリの結果が必要でない限り、カーネルを再起動して使用可能な RAM の量をリセットします。これにより、現在のクエリで使用できる RAM の最大量が確保されます。

![割り当てられる RAM の増加](./images/jupyterlab/user-guide/notebook-gpu-config.png)

最大量のメモリ（RAM）を割り当ててもこの問題が発生する場合は、データの列または範囲を減らすことで、より小さなサイズのデータセットに対して動作するようにクエリを変更することができます。すべてのデータを使用するには、Spark ノートブックを活用することをお勧めします。

## [!DNL Google Chrome] で [!DNL JupyterLab] 環境が読み込まれない

>[!IMPORTANT]
>
>この問題は解決しましたが、Google Chrome 80.x ブラウザーでは引き続き発生する可能性があります。 Chrome ブラウザーが最新であることを確認してください。

[!DNL Google Chrome] ブラウザーバージョン 80.x では、すべてのサードパーティ Cookie がデフォルトでブロックされます。このポリシーにより、[!DNL JupyterLab] が Adobe Experience Platform 内に読み込まれなくなる可能性があります。

この問題を修正するには、次の手順を実行します。

[!DNL Chrome] ブラウザーで、右上に移動して、「**設定**」を選択します（または、アドレスバーに「chrome://settings/」をコピー＆ペーストすることもできます）。次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

![Chrome の詳細設定](./images/faq/chrome-advanced.png)

「**プライバシーとセキュリティ**」セクションが表示されます。次に、「**サイトの設定**」をクリックし、「**Cookie とサイトデータ**」をクリックします。

![Chrome の詳細設定](./images/faq/privacy-security.png)

![Chrome の詳細設定](./images/faq/cookies.png)

最後に、「サードパーティ Cookie をブロック」を「オフ」に切り替えます。

![Chrome の詳細設定](./images/faq/toggle-off.png)

>[!NOTE]
>
>または、サードパーティ Cookie を無効にして、[*.]ds.adobe.net を許可リストに追加することもできます。

アドレスバーの「chrome://flags/」に移動します。右側のドロップダウンメニューを使用して、「*SameSite by default cookies*」というフラグを検索して無効にします。

![SameSite フラグを無効化](./images/faq/samesite-flag.png)

手順 2 の後、ブラウザーを再起動するように求められます。再起動すると、 [!DNL Jupyterlab] にアクセスできるようになります。

## Safari で [!DNL JupyterLab] にアクセスできないのはなぜですか？

Safari では、Safari 12 より前の場合、サードパーティ Cookie がデフォルトで無効になっています。[!DNL Jupyter] 仮想マシンインスタンスはその親フレームとは異なるドメインに存在するので、Adobe Experience Platform では現在、サードパーティ Cookie を有効にする必要があります。サードパーティ Cookie を有効にするか、[!DNL Google Chrome] などの別のブラウザーに切り替えてください。

Safari 12 の場合、ユーザーエージェントを「[!DNL Chrome]」または「[!DNL Firefox]」に切り替える必要があります。ユーザーエージェントを切り替えるには、*Safari* メニューを開き、「**環境設定**」を選択します。環境設定ウィンドウが表示されます。

![Safari 環境設定](./images/faq/preferences.png)

Safari 環境設定ウィンドウで、「**詳細**」を選択します。次に、「*メニューバーに“開発”メニューを表示*」ボックスを選択します。この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari の詳細設定](./images/faq/advanced.png)

次に、上部のナビゲーションバーから、**開発**&#x200B;メニューを選択します。**開発**&#x200B;ドロップダウン内で、**ユーザエージェント**&#x200B;にポインタを合わせます。使用したい **[!DNL Chrome]** または **[!DNL Firefox]** のユーザーエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## [!DNL JupyterLab] でファイルをアップロードまたは削除しようとすると、「403 Forbidden」というメッセージが表示されるのはなぜですか？

ブラウザーで [!DNL Ghostery] や [!DNL AdBlock] Plus などの広告ブロックソフトウェアが有効になっている場合、[!DNL JupyterLab] が正常に動作するには、ドメイン「\*.adobe.net」が各広告ブロックソフトウェアで許可されている必要があります。これは、[!DNL JupyterLab] 仮想マシンが [!DNL Experience Platform] ドメインとは別のドメインで実行されるからです。

## [!DNL Jupyter Notebook] の一部が暗号化されたように見える、つまりコードとして表示されないのはなぜですか？

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## カスタム [!DNL Python] ライブラリをインストールするにはどうすればよいですか？

[!DNL Python] カーネルには、よく利用される多くの機械学習ライブラリがプリインストールされています。ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

プリインストールされている [!DNL Python] ライブラリの完全なリストについては、[JupyterLab ユーザーガイドの付録](./jupyterlab/overview.md#supported-libraries)を参照してください。

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## [!DNL JupyterLab] [!DNL Spark] または PySpark カーネルに [!DNL Spark] クラスターリソースを設定できますか？

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

設定可能なプロパティの完全なリストなど、[!DNL Spark] クラスターリソースの設定について詳しくは、[JupyterLab ユーザーガイド](./jupyterlab/overview.md#kernels)を参照してください。

## 大きなデータセットに対して特定のタスクを実行しようとするとエラーが表示されるのはなぜですか？

エラーが表示され、その理由が `Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.` などの場合は、通常、ドライバーまたはエグゼキューターのメモリが不足していることを意味します。 データ制限や大きなデータセットに対してタスクを実行する方法について詳しくは、JupyterLab ノートブックの[データアクセス](./jupyterlab/access-notebook-data.md)に関するドキュメントを参照してください。通常、このエラーは `mode` を `interactive` から `batch` に変更することで解決できます。

さらに、大きな Spark／PySpark データセットを書き込む際に、データをキャッシュ（`df.cache()`）してから書き込みコードを実行すると、パフォーマンスが大幅に向上します。

<!-- remove this paragraph at a later date once the sdk is updated -->

データの読み取り中に問題が発生し、データに変換を適用する場合は、変換する前にデータをキャッシュしてみてください。データをキャッシュすると、ネットワーク全体での複数の読み取りを防ぐことができます。 まず、データを読み取ります。 次に、データをキャッシュ（`df.cache()`）します。最後に、変換を実行します。

## Spark／PySpark ノートブックがデータの読み書きに時間がかかるのはなぜですか？

`fit()` を使用するなど、データに対して変換を実行する場合は、変換が複数回実行されている可能性があります。パフォーマンスを向上させるには、`df.cache()` を使用してデータをキャッシュしてから、`fit()` を実行します。これにより、変換が 1 回だけ実行され、ネットワーク全体での複数の読み取りを防ぐことができます。

**推奨手順：**&#x200B;まず、データを読み取ります。 次に、変換を実行した後に、データをキャッシュ（`df.cache()`）します。最後に、`fit()` を実行します。 

## Spark／PySpark ノートブックの実行に失敗するはなぜですか？

次のいずれかのエラーが表示される場合：

- ステージの失敗によりジョブが中止されました … 各パーティション内の要素数が同一の RDD のみを圧縮できます。
- リモート RPC クライアントの関連付けが解除され、その他のメモリエラーが発生しました。
- データセットの読み取り時や書き込み時のパフォーマンスが低下しています。

データをキャッシュして（`df.cache()`）から書き込んでいるかを確認します。ノートブックでコードを実行する場合は、`fit()` などのアクションの前に `df.cache()` を使用すると、ノートブックのパフォーマンスが大幅に向上します。 データセットに書き込む前に `df.cache()` を使用することで、変換が複数回ではなく 1 回だけ実行されるようになります。

## データサイエンスワークスペースにおける [!DNL Docker Hub] の制限事項

2020年11月20日（PT）をもって、Docker Hub の匿名および無償での認証済み使用に対するレート制限が発効しました。 匿名および無償の [!DNL Docker Hub] ユーザーは、6 時間ごとに 100 件のコンテナイメージプルリクエストまでに制限されます。 これらの変更の影響を受ける場合は、「`ERROR: toomanyrequests: Too Many Requests.`」または「`You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits.`」というエラーメッセージが表示されます。

現在、この制限が組織に影響するのは、ノートブックからレシピの作成を 6 時間以内に 100 件行おうとしている場合や、頻繁に規模を拡大／縮小する Spark ベースのノートブックをデータサイエンスワークスペース内で使用している場合のみです。 ただし、これらが動作するクラスタはアイドル状態になるまで 2 時間はアクティブなままなので、このようなことは起こりそうにありません。 これにより、クラスターがアクティブな場合に必要なプルの数が減ります。 上記のエラーのいずれかが発生した場合は、 [!DNL Docker] のレート制限がリセットされるまで待つ必要があります。

[!DNL Docker Hub] のレート制限について詳しくは、[DockerHub ドキュメント](https://www.docker.com/increase-rate-limits)を参照してください。アドビでは、この問題の解決に現在鋭意取り組んでおり、今後のリリースで解決策が提供される見込みです。

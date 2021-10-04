---
keywords: Experience Platform；トラブルシューティング；Data Science Workspace；よく読まれるトピック
solution: Experience Platform
title: Data Science Workspace トラブルシューティングガイド
topic-legacy: Troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace に関するよくある質問に対する回答を示します。
exl-id: fbc5efdc-f166-4000-bde2-4aa4b0318b38
source-git-commit: c2c2b1684e2c2c3c76dc23ad1df720abd6c4356c
workflow-type: tm+mt
source-wordcount: '1165'
ht-degree: 23%

---

# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Data Science Workspace] に関するよくある質問に対する回答を示します。 [!DNL Platform] API に関する一般的な質問とトラブルシューティングについては、[Adobe Experience Platform API のトラブルシューティングガイド ](../landing/troubleshooting.md) を参照してください。

## [!DNL JupyterLab] 環境が  [!DNL Google Chrome]

>[!IMPORTANT]
>
>この問題は解決しましたが、Google Chrome 80.x ブラウザーには引き続き表示されます。 Chrome ブラウザーが最新であることを確認してください。

[!DNL Google Chrome] ブラウザーのバージョン 80.x では、すべてのサードパーティ Cookie がデフォルトでブロックされます。 このポリシーにより、Adobe Experience Platform内で [!DNL JupyterLab] が読み込まれなくなる場合があります。

この問題を修正するには、次の手順を実行します。

[!DNL Chrome] ブラウザーで、右上の「**設定**」を選択します ( または、アドレスバーに「chrome://settings/」をコピーして貼り付けることもできます )。 次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

![Chrome の詳細設定](./images/faq/chrome-advanced.png)

「**プライバシーとセキュリティ**」セクションが表示されます。次に、「**サイトの設定**」をクリックし、「**Cookie とサイトデータ**」をクリックします。

![Chrome の詳細設定](./images/faq/privacy-security.png)

![Chrome の詳細設定](./images/faq/cookies.png)

最後に、「サードパーティCookieのブロック」を「オフ」に切り替えます。

![Chrome の詳細設定](./images/faq/toggle-off.png)

>[!NOTE]
>
>または、サードパーティ Cookie を無効にして [*を追加することもできます。]ds.adobe.net を許可リストに追加します。

アドレスバーの「chrome://flags/」に移動します。右側のドロップダウンメニューを使用して、「*SameSite by default cookies*」というフラグを探して無効にします。

![SameSite フラグを無効化](./images/faq/samesite-flag.png)

手順 2 の後、ブラウザーを再起動するように求められます。再起動後は、[!DNL Jupyterlab] にアクセスできるようになります。

## Safari で [!DNL JupyterLab] にアクセスできないのはなぜですか。

Safari は、Safari ではデフォルトで 12 未満のサードパーティ Cookie を無効にします。 [!DNL Jupyter] 仮想マシンインスタンスは親フレームとは異なるドメインに存在するので、現在、Adobe Experience Platformではサードパーティ Cookie を有効にする必要があります。 サードパーティ Cookie を有効にするか、[!DNL Google Chrome] など別のブラウザーに切り替えてください。

Safari 12 の場合は、ユーザーエージェントを「[!DNL Chrome]」または「[!DNL Firefox]」に切り替える必要があります。 ユーザーエージェントを切り替えるには、まず *Safari* メニューを開き、**環境設定** を選択します。 プリファレンスウィンドウが表示されます。

![Safari の環境設定](./images/faq/preferences.png)

Safari の環境設定ウィンドウで、「**詳細**」を選択します。 次に、「*メニューバーに開発メニューを表示*」ボックスをオンにします。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari の詳細](./images/faq/advanced.png)

次に、上部ナビゲーションバーから **開発** メニューを選択します。 **開発** ドロップダウンで、**ユーザーエージェント** の上にマウスポインターを置きます。 使用する **[!DNL Chrome]** または **[!DNL Firefox]** ユーザーエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## [!DNL JupyterLab] 内のファイルをアップロードまたは削除しようとすると、「403 Forbidden」というメッセージが表示されるのはなぜですか？

ブラウザが [!DNL Ghostery] や [!DNL AdBlock] Plus などの広告ブロックソフトウェアで有効になっている場合、[!DNL JupyterLab] が正常に動作するには、各広告ブロックソフトウェアでドメイン「\*.adobe.net」を許可する必要があります。 これは、[!DNL JupyterLab] 仮想マシンが [!DNL Experience Platform] ドメインとは異なるドメインで実行されるためです。

## [!DNL Jupyter Notebook] の一部が乱れて見えるのはなぜですか、それともコードとして表示されないのですか。

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## カスタムの [!DNL Python] ライブラリをインストールする方法を教えてください。

[!DNL Python] カーネルは、多くの一般的な機械学習ライブラリと共にプリインストールされています。 ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

事前インストールされた [!DNL Python] ライブラリの完全なリストについては、JupyterLab ユーザーガイドの [ 付録の節 ](./jupyterlab/overview.md#supported-libraries) を参照してください。

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## [!DNL JupyterLab] [!DNL Spark] または PySpark カーネルに対して [!DNL Spark] クラスターリソースを設定できますか？

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

設定可能なプロパティの完全なリストを含む、[!DNL Spark] クラスターリソースの設定について詳しくは、『JupyterLab ユーザーガイド ](./jupyterlab/overview.md#kernels)』を参照してください。[

## 大規模なデータセットに対して特定のタスクを実行しようとするとエラーが発生するのはなぜですか。

`Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.` などの理由でエラーが発生した場合は、通常、ドライバーまたは実行者のメモリ不足を意味します。 データ制限の詳細と大規模なデータセットでのタスクの実行方法については、 JupyterLab ノートブック [ データアクセス ](./jupyterlab/access-notebook-data.md) のドキュメントを参照してください。 通常、このエラーは `mode` を `interactive` から `batch` に変更することで解決できます。

また、大きな Spark/PySpark データセットを書き込む際に、書き込みコードを実行する前にデータ (`df.cache()`) をキャッシュすると、パフォーマンスが大幅に向上します。

<!-- remove this paragraph at a later date once the sdk is updated -->

データの読み取り中に問題が発生し、データに変換を適用している場合は、変換の前にデータをキャッシュしてみてください。 データをキャッシュすると、ネットワークを介した複数の読み取りを防ぐことができます。 まずデータを読み取ります。 次に、データをキャッシュ (`df.cache()`) します。 最後に、変換を実行します。

## Spark/PySpark ノートブックでデータの読み取りと書き込みに時間がかかるのはなぜですか？

`fit()` を使用するなど、データに対して変換を実行する場合、変換が複数回実行されている可能性があります。 パフォーマンスを向上させるには、`fit()` を実行する前に `df.cache()` を使用してデータをキャッシュします。 これにより、変換が 1 回だけ実行され、ネットワークを介した複数の読み取りを防ぐことができます。

**推奨順序：** まずデータを読み取ります。次に、変換を実行し、次にデータをキャッシュ (`df.cache()`) します。 最後に、`fit()` を実行します。

## Spark/PySpark ノートブックが動作しないのはなぜですか？

次のいずれかのエラーが発生した場合：

- ステージエラーのためジョブが中止されました…各パーティション内の要素数が同じ RDD のみを zip できます。
- リモート RPC クライアントが関連付けを解除し、その他のメモリエラーが発生しました。
- データセットの読み取りと書き込みの際のパフォーマンスが低下。

データを書き込む前に、データ (`df.cache()`) をキャッシュしていることを確認してください。 ノートブックでコードを実行する場合、`fit()` のようなアクションの前に `df.cache()` を使用すると、ノートブックのパフォーマンスが大幅に向上します。 データセットを書き込む前に `df.cache()` を使用すると、変換が複数回ではなく、1 回だけ実行されるようになります。

## [!DNL Docker Hub] Data Science Workspace の制限

2020 年 11 月 20 日をもって、Docker Hub の匿名での無料認証使用に関するレート制限が施行されました。 匿名ユーザーと無料 [!DNL Docker Hub] ユーザーは、6 時間ごとに 100 個のコンテナイメージプルリクエストに制限されています。 これらの変更の影響を受ける場合は、次のエラーメッセージが表示されます。`ERROR: toomanyrequests: Too Many Requests.` または `You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits.`。

現在、この制限は、6 時間以内に 100 個のノートブックをレシピに作成しようとしている場合、または頻繁に拡大/縮小する Data Science Workspace 内で Spark ベースのノートブックを使用している場合にのみ、組織に影響します。 ただし、このような状況は考えられません。クラスタ上で実行されるクラスタは、アイドルアウト前の 2 時間アクティブのままになるからです。 これにより、クラスターがアクティブな場合に必要なプル数が減ります。 上記のエラーが発生した場合は、[!DNL Docker] 制限がリセットされるまで待つ必要があります。

[!DNL Docker Hub] レート制限の詳細については、[DockerHub のドキュメント ](https://www.docker.com/increase-rate-limits) を参照してください。 この解決策は、今後のリリースで検討中で、予想されるものです。

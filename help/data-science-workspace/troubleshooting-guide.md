---
keywords: Experience Platform；トラブルシューティング；Data Science Workspace；よく読まれるトピック
solution: Experience Platform
title: Data Science Workspaceトラブルシューティングガイド
topic-legacy: Troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace に関するよくある質問に対する回答を示します。
exl-id: fbc5efdc-f166-4000-bde2-4aa4b0318b38
source-git-commit: c2c2b1684e2c2c3c76dc23ad1df720abd6c4356c
workflow-type: tm+mt
source-wordcount: '1165'
ht-degree: 23%

---

# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform [!DNL Data Science Workspace]に関するよくある質問に対する回答を示します。 [!DNL Platform] APIに関する一般的な質問とトラブルシューティングについては、[Adobe Experience Platform APIのトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## [!DNL JupyterLab] 環境が  [!DNL Google Chrome]

>[!IMPORTANT]
>
>この問題は解決しましたが、Google Chrome 80.xブラウザーには引き続き存在する可能性があります。 Chromeブラウザーが最新であることを確認してください。

[!DNL Google Chrome]ブラウザーのバージョン80.xでは、すべてのサードパーティCookieがデフォルトでブロックされます。 このポリシーは、Adobe Experience Platform内で[!DNL JupyterLab]が読み込まれない可能性があります。

この問題を修正するには、次の手順を実行します。

[!DNL Chrome]ブラウザーで、右上に移動し、**設定**&#x200B;を選択します(または、アドレスバーに「chrome://settings/」をコピーして貼り付けることもできます)。 次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

![Chrome の詳細設定](./images/faq/chrome-advanced.png)

「**プライバシーとセキュリティ**」セクションが表示されます。次に、「**サイトの設定**」をクリックし、「**Cookie とサイトデータ**」をクリックします。

![Chrome の詳細設定](./images/faq/privacy-security.png)

![Chrome の詳細設定](./images/faq/cookies.png)

最後に、「サードパーティCookieのブロック」を「オフ」に切り替えます。

![Chrome の詳細設定](./images/faq/toggle-off.png)

>[!NOTE]
>
>または、サードパーティCookieを無効にして[*を追加します。]ds.adobe.netを許可リストに追加します。

アドレスバーの「chrome://flags/」に移動します。右側のドロップダウンメニューを使用して、「*SameSite by default cookies*」というフラグを探して無効にします。

![SameSite フラグを無効化](./images/faq/samesite-flag.png)

手順 2 の後、ブラウザーを再起動するように求められます。再起動後は、[!DNL Jupyterlab]にアクセスできるようになります。

## Safariで[!DNL JupyterLab]にアクセスできないのはなぜですか？

Safariは、SafariではデフォルトでサードパーティCookieを無効にします（12未満）。 [!DNL Jupyter]仮想マシンインスタンスは親フレームとは異なるドメインに存在するので、現在、Adobe Experience PlatformではサードパーティCookieを有効にする必要があります。 サードパーティCookieを有効にするか、[!DNL Google Chrome]など別のブラウザーに切り替えてください。

Safari 12の場合は、ユーザーエージェントを「[!DNL Chrome]」または「[!DNL Firefox]」に切り替える必要があります。 ユーザーエージェントを切り替えるには、まず&#x200B;*Safari*&#x200B;メニューを開き、**環境設定**&#x200B;を選択します。 プリファレンスウィンドウが表示されます。

![Safariの環境設定](./images/faq/preferences.png)

Safariの環境設定ウィンドウで、「**詳細**」を選択します。 次に、「*メニューバーに開発メニューを表示*」ボックスをオンにします。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safariの詳細設定](./images/faq/advanced.png)

次に、上部ナビゲーションバーから&#x200B;**開発**&#x200B;メニューを選択します。 **開発**&#x200B;ドロップダウンで、**ユーザーエージェント**&#x200B;の上にマウスポインターを置きます。 使用する&#x200B;**[!DNL Chrome]**&#x200B;または&#x200B;**[!DNL Firefox]**&#x200B;ユーザーエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## [!DNL JupyterLab]内のファイルをアップロードまたは削除しようとすると、「403 Forbidden」というメッセージが表示されるのはなぜですか？

ブラウザーが[!DNL Ghostery]や[!DNL AdBlock] Plusなどの広告ブロックソフトウェアで有効になっている場合、[!DNL JupyterLab]が正常に動作するには、各広告ブロックソフトウェアでドメイン「\*.adobe.net」を許可する必要があります。 これは、[!DNL JupyterLab]仮想マシンが[!DNL Experience Platform]ドメインとは異なるドメインで実行されるためです。

## [!DNL Jupyter Notebook]の一部が乱れて見えるのはなぜですか、それともコードとしてレンダリングされないのですか。

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## カスタムの[!DNL Python]ライブラリをインストールする方法を教えてください。

[!DNL Python]カーネルは、多くの一般的な機械学習ライブラリと共にプリインストールされています。 ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

プリインストールされた[!DNL Python]ライブラリの完全なリストについては、JupyterLabユーザーガイドの[付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## [!DNL JupyterLab] [!DNL Spark]またはPySparkカーネルに対して[!DNL Spark]クラスターリソースを設定できますか？

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

設定可能なプロパティの完全なリストを含む、[!DNL Spark]クラスターリソースの設定について詳しくは、『JupyterLabユーザーガイド](./jupyterlab/overview.md#kernels)』を参照してください。[

## 大規模なデータセットに対して特定のタスクを実行しようとするとエラーが発生するのはなぜですか？

`Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.`などの理由でエラーが発生した場合は、通常、ドライバーまたは実行者のメモリが不足しています。 データ制限と大規模なデータセットでのタスクの実行方法について詳しくは、 JupyterLabノートブック[データアクセス](./jupyterlab/access-notebook-data.md)のドキュメントを参照してください。 通常、このエラーは`mode`を`interactive`から`batch`に変更することで解決できます。

また、大きなSpark/PySparkデータセットを書き込む際に、書き込みコードを実行する前にデータ(`df.cache()`)をキャッシュすると、パフォーマンスが大幅に向上します。

<!-- remove this paragraph at a later date once the sdk is updated -->

データの読み取り中に問題が発生し、データに変換を適用する場合は、変換の前にデータをキャッシュしてみてください。 データをキャッシュすると、ネットワーク全体での複数の読み取りを防ぐことができます。 まず、データを読み取ります。 次に、データをキャッシュ(`df.cache()`)します。 最後に、変換を実行します。

## Spark/PySparkノートブックがデータの読み取りと書き込みに長くかかるのはなぜですか？

`fit()`の使用など、データに対して変換を実行する場合、変換が複数回実行されている可能性があります。 パフォーマンスを向上させるには、`fit()`を実行する前に`df.cache()`を使用してデータをキャッシュします。 これにより、変換は1回だけ実行され、ネットワークを介した複数の読み取りを防ぐことができます。

**推奨順序：** データの読み取りから開始します。次に、変換を実行し、次にデータをキャッシュ(`df.cache()`)します。 最後に、`fit()`を実行します。

## Spark/PySparkノートブックが動作しないのはなぜですか？

次のいずれかのエラーが表示される場合：

- ステージエラーのためジョブが中止されました…各パーティション内の要素数が同じRDDのみをzipできます。
- リモートRPCクライアントが関連付けを解除し、その他のメモリエラーが発生しました。
- データセットの読み取りと書き込みの際のパフォーマンスが低下。

データを書き込む前に、データ(`df.cache()`)をキャッシュしていることを確認してください。 ノートブックでコードを実行する場合、`fit()`などのアクションの前に`df.cache()`を使用すると、ノートブックのパフォーマンスを大幅に向上できます。 データセットを書き込む前に`df.cache()`を使用すると、変換は複数回ではなく、1回だけ実行されます。

## [!DNL Docker Hub] Data Science Workspaceの制限

2020年11月20日をもって、Docker Hubの匿名での認証済み使用に関するレート制限が施行されました。 匿名ユーザーと無料[!DNL Docker Hub]ユーザーは、6時間ごとに100個のコンテナイメージプルリクエストに制限されます。 これらの変更の影響を受ける場合は、次のエラーメッセージが表示されます。`ERROR: toomanyrequests: Too Many Requests.`または`You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits.`。

現在、この制限は、6時間以内に100個のノートブックをレシピに作成しようとしている場合、または頻繁に拡大/縮小されるData Science Workspace内でSparkベースのノートブックを使用している場合にのみ、組織に影響します。 ただし、これらのクラスタは、アイドルアウトする前に2時間アクティブなままになるので、これは考えられません。 これにより、クラスターがアクティブな場合に必要なプル数が減ります。 上記のエラーが発生した場合は、[!DNL Docker]制限がリセットされるまで待つ必要があります。

[!DNL Docker Hub]レート制限について詳しくは、[DockerHubのドキュメント](https://www.docker.com/increase-rate-limits)を参照してください。 このためのソリューションは、今後のリリースで使用され、予想されるものです。

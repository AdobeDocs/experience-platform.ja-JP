---
keywords: Experience Platform；トラブルシューティング；Data Science Workspace；よく読まれるトピック
solution: Experience Platform
title: Data Science Workspaceトラブルシューティングガイド
topic-legacy: Troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace に関するよくある質問に対する回答を示します。
exl-id: fbc5efdc-f166-4000-bde2-4aa4b0318b38
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 29%

---

# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform[!DNL Data Science Workspace]に関するよくある質問に対する回答を提供します。 [!DNL Platform] APIに関する一般的な質問とトラブルシューティングについては、[Adobe Experience PlatformAPIトラブルシューティングガイド](../landing/troubleshooting.md)を参照してください。

## [!DNL JupyterLab] 環境が  [!DNL Google Chrome]

>[!IMPORTANT]
>
>この問題は解決しましたが、Google Chrome 80.xブラウザーには引き続き存在する可能性があります。 Chromeブラウザーが最新の状態であることを確認してください。

[!DNL Google Chrome]ブラウザーバージョン80.xでは、サードパーティCookieはすべてデフォルトでブロックされます。 このポリシーは、[!DNL JupyterLab]がAdobe Experience Platform内で読み込まれるのを防ぐことができます。

この問題を修正するには、次の手順を実行します。

[!DNL Chrome]ブラウザーで、右上の&#x200B;**設定**&#x200B;を選択します(または、アドレスバーに「chrome://settings/」をコピーして貼り付けることができます)。 次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

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

手順 2 の後、ブラウザーを再起動するように求められます。再起動後は、[!DNL Jupyterlab]にアクセスできるようにする必要があります。

## Safariで[!DNL JupyterLab]にアクセスできないのはなぜですか。

Safariでは、Safari &lt; 12で、デフォルトでサードパーティcookieが無効になっています。 [!DNL Jupyter]仮想マシンインスタンスは親フレームとは異なるドメインに存在するので、現在、Adobe Experience Platformではサードパーティcookieを有効にする必要があります。 サードパーティCookieを有効にするか、[!DNL Google Chrome]など別のブラウザーに切り替えてください。

Safari 12の場合は、ユーザーエージェントを&#39;[!DNL Chrome]&#39;または&#39;[!DNL Firefox]&#39;に切り替える必要があります。 ユーザーエージェントを切り替えるには、*Safari*&#x200B;メニューを開き、**環境設定**&#x200B;を選択します。 プリファレンスウィンドウが表示されます。

![Safariの環境設定](./images/faq/preferences.png)

Safariの環境設定ウィンドウで、「**詳細**」を選択します。 次に、*メニューバーの「開発メニューを表示*」ボックスをオンにします。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari advanced](./images/faq/advanced.png)

次に、上部ナビゲーションバーで「**開発**」メニューを選択します。 「**開発**」ドロップダウンで、**ユーザーエージェント**&#x200B;の上にマウスポインターを置きます。 使用する&#x200B;**[!DNL Chrome]**&#x200B;または&#x200B;**[!DNL Firefox]**&#x200B;ユーザーエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## [!DNL JupyterLab]内のファイルをアップロードまたは削除しようとすると&#39;403 Forbidden&#39;メッセージが表示されるのはなぜですか。

[!DNL Ghostery]や[!DNL AdBlock] Plusなどの広告ブロックソフトウェアを使用してブラウザを有効にしている場合、[!DNL JupyterLab]が正常に動作するには、各広告ブロックソフトウェアでドメイン「\*.adobe.net」を許可する必要があります。 これは、[!DNL JupyterLab]仮想マシンが[!DNL Experience Platform]ドメインとは異なるドメインで実行されているためです。

## [!DNL Jupyter Notebook]の一部がスクランブルに見えるのはなぜですか、それともコードとして表示されないのはなぜですか。

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## カスタム[!DNL Python]ライブラリのインストール方法

[!DNL Python]カーネルは、多くの一般的な機械学習ライブラリと共にプリインストールされています。 ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

プリインストールされた[!DNL Python]ライブラリの完全なリストについては、『JupyterLabユーザーガイド](./jupyterlab/overview.md#supported-libraries)』の[付録の節を参照してください。

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## [!DNL JupyterLab] [!DNL Spark]やPySparkカーネルに対して[!DNL Spark]クラスタリソースを設定することは可能ですか？

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

構成可能なプロパティの完全なリストを含む[!DNL Spark]クラスタリソースの構成について詳しくは、[JupyterLab User Guide](./jupyterlab/overview.md#kernels)を参照してください。

## 大きなデータセットに対して特定のタスクを実行しようとするとエラーが発生するのはなぜですか。

`Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.`などの理由でエラーが発生した場合は、通常、ドライバまたはエクゼキュータのメモリ不足を意味します。 データ制限の詳細および大きなデータセットに対するタスクの実行方法については、JupyterLabノートブック[データアクセス](./jupyterlab/access-notebook-data.md)のドキュメントを参照してください。 通常、このエラーは`mode`を`interactive`から`batch`に変更することで解決できます。

## [!DNL Docker Hub] data Science Workspaceでの制限

2020年11月20日現在、Docker Hubの匿名および無料の認証使用に対するレート制限が有効になりました。 匿名ユーザーおよび無料[!DNL Docker Hub]ユーザーは、6時間ごとに100個のコンテナイメージのプル要求に制限されます。 次の変更の影響を受ける場合は、次のエラーメッセージが表示されます。`ERROR: toomanyrequests: Too Many Requests.`または`You have reached your pull rate limit. You may increase the limit by authenticating and upgrading: https://www.docker.com/increase-rate-limits.`。

現在、この制限は、6時間以内にレシピ用の100ノートブックを作成しようとする場合、またはData Science Workspace内でSparkベースのノートブックを使用していて、頻繁に拡大/縮小している場合にのみ、組織に影響します。 ただし、これらのクラスタは、アイドリングアウトする前に2時間アクティブなままになるため、このような状況は考えにくくなります。 これにより、クラスターがアクティブな場合に必要なプルの数が減ります。 上記のエラーが発生した場合は、[!DNL Docker]制限がリセットされるまで待つ必要があります。

[!DNL Docker Hub]レート制限について詳しくは、[DockerHubドキュメント](https://www.docker.com/increase-rate-limits)を参照してください。 この問題の解決策は現在検討中で、今後のリリースで予定されています。

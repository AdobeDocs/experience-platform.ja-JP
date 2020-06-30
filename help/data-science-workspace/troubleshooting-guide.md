---
keywords: Experience Platform;troubleshooting;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspaceトラブルシューティングガイド
topic: Troubleshooting
translation-type: tm+mt
source-git-commit: 1e5526b54f3c52b669f9f6a792eda0abfc711fdd
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---


# [!DNL Data Science Workspace] トラブルシューティングガイド

このドキュメントでは、Adobe Experience Platformに関するよくある質問に対する回答を示 [!DNL Data Science Workspace]します。 APIに関する一般的な質問とトラブルシューティングについては、 [!DNL Platform] Adobe Experience PlatformAPIトラブルシューティングガイドを参照してください [](../landing/troubleshooting.md)。

## [!DNL JupyterLab] 環境が [!DNL Google Chrome]

>[!IMPORTANT] この問題は解決しましたが、Google Chrome 80.xブラウザーには引き続き存在する可能性があります。 Chromeブラウザーが最新の状態であることを確認してください。

ブラウザーバージョン80.xでは、 [!DNL Google Chrome] サードパーティCookieはすべてデフォルトでブロックされます。 このポリシーは、Adobe Experience Platform内での読み込み [!DNL JupyterLab] を妨げる可能性があります。

この問題を修正するには、次の手順を実行します。

ブラウザーで [!DNL Chrome] 右上に移動し、「 **設定** 」を選択します(または、アドレスバーに「chrome://settings/」をコピーして貼り付けることができます)。 次に、ページの下部までスクロールし、「 **詳細** 」ドロップダウンをクリックします。

![chrome advanced](./images/faq/chrome-advanced.png)

「 *プライバシーとセキュリティ* 」セクションが表示されます。 次に、「 **サイト設定** 」をクリックし、「 **Cookie」とサイトデータをクリックします**。

![chrome advanced](./images/faq/privacy-security.png)

![chrome advanced](./images/faq/cookies.png)

最後に、「サードパーティcookieをブロック」を「オフ」に切り替えます。

![chrome advanced](./images/faq/toggle-off.png)

>[!NOTE] または、サードパーティCookieを無効にして、 [*を追加します。]ds.adobe.netを許可リストに追加します。

アドレスバーの「chrome://flags/」に移動します。 右側のドロップダウンメニューを使用して、「SameSite by default cookies」という名前のフラグを検索して無効に *し* ます。

![samesiteフラグを無効にする](./images/faq/samesite-flag.png)

手順2の後、ブラウザーを再起動するように求められます。 再起動後は、アクセス [!DNL Jupyterlab] 可能にする必要があります。

## Safariでアクセスできないのはなぜ [!DNL JupyterLab] ですか。

Safariでは、Safari &lt; 12で、デフォルトでサードパーティcookieが無効になっています。 仮想マシンインスタンスは親フレームとは異なるドメインに存在するため、現在、Adobe Experience Platformではサードパーティcookieが有効になっている必要があります。 [!DNL Jupyter] サードパーティCookieを有効にするか、などの別のブラウザーに切り替えてくだ [!DNL Google Chrome]さい。

Safari 12の場合は、ユーザーエージェントを「[!DNL Chrome]」または「[!DNL Firefox]」に切り替える必要があります。 ユーザーエージェントを切り替えるには、 *Safari* 開始を開いて「 **環境設定**」を選択します。 プリファレンスウィンドウが表示されます。

![Safariの環境設定](./images/faq/preferences.png)

Safariの環境設定ウィンドウで、「 **詳細」を選択します**。 次に、メニューバーの *「現像を表示」メニュー* ・ボックスを選択します。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari advanced](./images/faq/advanced.png)

次に、上部ナビゲーションバーで **開発** メニューを選択します。 「 *開発* 」ドロップダウンで、「 *ユーザーエージェント*」の上にカーソルを置きます。 使用する **[!DNL Chrome]** または **[!DNL Firefox]** ユーザエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## でファイルをアップロードまたは削除しようとすると「403 Forbidden」というメッセージが表示されるのはなぜで [!DNL JupyterLab]すか。

ブラウザーが、 [!DNL Ghostery] または [!DNL AdBlock] Plusなどの広告ブロックソフトウェアで有効になっている場合、通常どおりに動作するには、各広告ブロックソフトウェアでドメイン「\*.adobe.net」を許可する必要 [!DNL JupyterLab] があります。 これは、 [!DNL JupyterLab] 仮想マシンがドメインとは異なるドメインで実行されるため [!DNL Experience Platform] です。

## 見た目の一部がスクランブルしているか、コードとしてレンダリングされないのはなぜですか。 [!DNL Jupyter Notebook]

これは、問題のセルが誤って「コード」から「マークダウン」に変更された場合に発生する可能性があります。 コードセルにフォーカスがある状態で、キーの組み合わせ **ESC+M** を押すと、セルの種類がMarkdownに変わります。 セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。 セルの種類をコードに変更するには、変更するセルを開始します。 次に、セルの現在のタイプを示すドロップダウンをクリックし、「コード」を選択します。

![](./images/faq/code_type.png)

## カスタム [!DNL Python] ライブラリのインストール方法

この [!DNL Python] カーネルは、多くの一般的な機械学習ライブラリと共にプリインストールされています。 ただし、追加のカスタムライブラリは、コードセル内で次のコマンドを実行することでインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

プリインストールされた [!DNL Python] ライブラリの完全なリストについては、『JupyterLab User Guide [』の](./jupyterlab/overview.md#supported-libraries)付録の節を参照してください。

## カスタムPySparkライブラリをインストールできますか。

残念ながら、PySparkカーネル用の追加のライブラリはインストールできません。 ただし、アドビのカスタマーサービス担当者に連絡して、カスタムPySparkライブラリをインストールしてもらうことができます。

PySparkの事前インストールされたライブラリのリストについては、『JupyterLab User Guide [』の](./jupyterlab/overview.md#supported-libraries)付録の節を参照してください。

## PySparkカーネル用に [!DNL Spark] クラスタリソースを設定でき [!DNL JupyterLab] ま [!DNL Spark] すか。

ノートブックの最初のセルに次のブロックを追加して、リソースを構成できます。

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

構成可能なプロパティの完全なリストなど、 [!DNL Spark] クラスタリソースの構成の詳細については、『 [JupyterLab User Guide](./jupyterlab/overview.md#kernels)』を参照してください。
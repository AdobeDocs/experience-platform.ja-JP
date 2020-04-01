---
keywords: Experience Platform;troubleshooting;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspaceトラブルシューティングガイド
topic: Troubleshooting
translation-type: tm+mt
source-git-commit: 1f756e7bc71c9ff227757aee64af29e0772c24af

---


# Data Science Workspaceトラブルシューティングガイド

このドキュメントでは、Adobe Experience Platform Data Science Workspaceに関するよくある質問に対する回答を示します。 プラットフォームAPIに関する一般的な質問とトラブルシューティングについては、 [Adobe Experience Platform APIのトラブルシューティングガイドを参照してくださ](../landing/troubleshooting.md)い。

## JupyterLab環境がGoogle Chromeで読み込まれない

Google Chromeブラウザーの最新バージョン80.xへのアップデートにより、すべてのサードパーティCookieがデフォルトでブロックされます。 この新しいポリシーにより、JupyterLabがAdobe Experience Platform内で読み込まれなくなる可能性があります。

>[!NOTE] これは一時的な問題です。 サードパーティCookieへの依存関係は、今後のリリースで削除されるように設定されています。

この問題を解決するには、次の手順を実行します。

Chromeブラウザーで、右上の「設定」を選択します **** (または、アドレスバーに「chrome://settings/」をコピーして貼り付けることができます)。 次に、ページの下部までスクロールし、[詳細]ドロップダウンをクリ **ックします** 。

![Chrome Advanced](./images/faq/chrome-advanced.png)

「プライバ *シーとセキュリティ* 」セクションが表示されます。 次に、[サイトの設定] **をクリックし** 、[Cookieとサ **イトデータ]をクリックします**。

![Chrome Advanced](./images/faq/privacy-security.png)

![Chrome Advanced](./images/faq/cookies.png)

最後に、「サードパーティcookieのブロック」を「オフ」に切り替えます。

![Chrome Advanced](./images/faq/toggle-off.png)

>[!NOTE] または、サードパーティCookieとホワイトリスト [*を無効にします。]ds.adobe.net

アドレスバーの「chrome://flags/」に移動します。 右側のドロップダウンメニューを使用し *て、「SameSite by default cookies」という名前の* フラグを検索し、無効にします。

![samesiteフラグを無効にする](./images/faq/samesite-flag.png)

手順2の後、ブラウザーを再起動するように求められます。 再起動後、Jupyterlabはアクセス可能になります。

## SafariでJupyterLabにアクセスできないのはなぜですか。

Safariは、デフォルトでサードパーティcookieを無効にします。 Jupyter仮想マシンインスタンスは親フレームとは異なるドメインに存在するので、現在、Adobe Experience Platformではサードパーティcookieを有効にする必要があります。 サードパーティCookieを有効にするか、Google Chromeなどの別のブラウザーに切り替えてください。

## JupyterLabでファイルをアップロードまたは削除しようとすると、「403 Forbidden」というメッセージが表示されるのはなぜですか。

ブラウザがGhosteryやAdBlock Plusなどの広告ブロックソフトウェアで有効になっている場合、JupyterLabが正常に動作するには、ドメイン「\*.adobe.net」が各広告ブロックソフトウェアにホワイトリストに登録されている必要があります。 これは、JupyterLab仮想マシンがExperience Platformドメインとは異なるドメインで実行されるためです。

## ジュピター・ノートブックの一部が乱れて見えるのはなぜですか、それともコードとして表示されないのですか。

これは、問題のセルが誤って「コード」から「マークダウン」に変更された場合に発生する可能性があります。 コードセルにフォーカスがある間、キーの組み合わせ **ESC+Mを押すと** 、セルの種類がMarkdownに変更されます。 セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。 セルの種類をコードに変更するには、開始で変更するセルを選択します。 次に、セルの現在のタイプを示すドロップダウンをクリックし、「コード」を選択します。

![](./images/faq/code_type.png)

## カスタムPythonライブラリのインストール方法

Pythonカーネルは、多くの一般的な機械学習ライブラリと共にプリインストールされています。 ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

Pythonライブラリの完全なリストについては、『 JupyterLab User Guide [』の付録の節を参照してください](./jupyterlab/overview.md#supported-libraries)。

## カスタムPySparkライブラリをインストールできますか？

残念ながら、PySparkカーネル用の追加のライブラリはインストールできません。 ただし、アドビのカスタマーサービス担当者に連絡して、カスタムPySparkライブラリをインストールしてもらうことができます。

PySparkの事前インストールリストについては、『JupyterLab User Guide [』の付録の節を参照してください](./jupyterlab/overview.md#supported-libraries)。

## JupyterLab SparkとPySparkカーネルのどちらにSparkクラスタリソースを設定できますか？

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

設定可能なプロパティの完全なリストを含む、Sparkクラスタリソースの設定について詳しくは、 [JupyterLab User Guideを参照してください](./jupyterlab/overview.md#pyspark-spark-execution-resource)。
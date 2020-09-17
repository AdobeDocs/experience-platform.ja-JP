---
keywords: Experience Platform;troubleshooting;Data Science Workspace;popular topics
solution: Experience Platform
title: Data Science Workspace トラブルシューティングガイド
topic: Troubleshooting
description: このドキュメントでは、Adobe Experience Platform Data Science Workspace に関するよくある質問に対する回答を示します。
translation-type: tm+mt
source-git-commit: 76e598c743df320e4b3cb821e118749fe7304d9c
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 37%

---


# [!DNL Data Science Workspace] トラブルシューティングガイド

This document provides answers to frequently asked questions about Adobe Experience Platform [!DNL Data Science Workspace]. For questions and troubleshooting regarding [!DNL Platform] APIs in general, see the [Adobe Experience Platform API troubleshooting guide](../landing/troubleshooting.md).

## [!DNL JupyterLab] 環境が [!DNL Google Chrome]

>[!IMPORTANT]
>
>この問題は解決しましたが、Google Chrome 80.xブラウザーには引き続き存在する可能性があります。 Chromeブラウザーが最新の状態であることを確認してください。

ブラウザーバージョン80.xでは、 [!DNL Google Chrome] サードパーティCookieはすべてデフォルトでブロックされます。 This policy can prevent [!DNL JupyterLab] from loading within Adobe Experience Platform.

この問題を修正するには、次の手順を実行します。

In your [!DNL Chrome] browser, navigate to the top-right and select **Settings** (alternatively you can copy and paste &quot;chrome://settings/&quot; in the address bar). 次に、ページの下部までスクロールし、**詳細設定**&#x200B;ドロップダウンをクリックします。

![Chrome の詳細設定](./images/faq/chrome-advanced.png)

「*プライバシーとセキュリティ*」セクションが表示されます。次に、「**サイトの設定**」をクリックし、「**Cookie とサイトデータ**」をクリックします。

![Chrome の詳細設定](./images/faq/privacy-security.png)

![Chrome の詳細設定](./images/faq/cookies.png)

最後に、「サードパーティCookieのブロック」を「オフ」に切り替えます。

![Chrome の詳細設定](./images/faq/toggle-off.png)

>[!NOTE]
>
>Alternatively, you could disable third-party cookies and add [*.]ds.adobe.netを許可リストに追加します。

アドレスバーの「chrome://flags/」に移動します。右側のドロップダウンメニューを使用して、「*SameSite by default cookies*」というフラグを探して無効にします。

![SameSite フラグを無効化](./images/faq/samesite-flag.png)

手順 2 の後、ブラウザーを再起動するように求められます。After you relaunch, [!DNL Jupyterlab] should be accessible.

## Why am I unable to access [!DNL JupyterLab] in Safari?

Safariでは、Safari &lt; 12で、デフォルトでサードパーティcookieが無効になっています。 Because your [!DNL Jupyter] virtual machine instance resides on a different domain than its parent frame, Adobe Experience Platform currently requires that third-party cookies be enabled. Please enable third-party cookies or switch to a different browser such as [!DNL Google Chrome].

Safari 12の場合は、ユーザーエージェントを「[!DNL Chrome]」または「[!DNL Firefox]」に切り替える必要があります。 ユーザーエージェントを切り替えるには、 *Safari* 開始を開いて「 **環境設定**」を選択します。 プリファレンスウィンドウが表示されます。

![Safariの環境設定](./images/faq/preferences.png)

Safariの環境設定ウィンドウで、「 **詳細」を選択します**。 次に、メニューバーの *「現像を表示」メニュー* ・ボックスを選択します。 この手順が完了したら、環境設定ウィンドウを閉じることができます。

![Safari advanced](./images/faq/advanced.png)

次に、上部ナビゲーションバーで **開発** メニューを選択します。 「 *開発* 」ドロップダウンで、「 *ユーザーエージェント*」の上にカーソルを置きます。 使用する **[!DNL Chrome]** または **[!DNL Firefox]** ユーザエージェント文字列を選択できます。

![開発メニュー](./images/faq/user-agent.png)

## Why am I seeing a &#39;403 Forbidden&#39; message when trying to upload or delete a file in [!DNL JupyterLab]?

If your browser is enabled with advertisement blocking software such as [!DNL Ghostery] or [!DNL AdBlock] Plus, the domain &quot;\*.adobe.net&quot; must be allowed in each advertisement blocking software for [!DNL JupyterLab] to operate normally. This is because [!DNL JupyterLab] virtual machines run on a different domain than the [!DNL Experience Platform] domain.

## Why do some parts of my [!DNL Jupyter Notebook] look scrambled or do not render as code?

これは、問題のセルが誤って「Code」から「Markdown」に変更された場合に発生する可能性があります。コードセルにフォーカスがある間に **Esc + M** キーを押すと、セルの種類が Markdown に変更されます。セルの種類は、選択したセルのノートブックの上部にあるドロップダウンインジケーターで変更できます。セルの種類をコードに変更するには、まず、変更するセルを選択します。次に、セルの現在の種類を示すドロップダウンをクリックし、「Code」を選択します。

![](./images/faq/code_type.png)

## How do I install custom [!DNL Python] libraries?

The [!DNL Python] kernel comes pre-installed with many popular machine learning libraries. ただし、コードセル内で次のコマンドを実行すると、追加のカスタムライブラリをインストールできます。

```shell
!pip install {LIBRARY_NAME}
```

For a complete list of pre-installed [!DNL Python] libraries, see the [appendix section of the JupyterLab User Guide](./jupyterlab/overview.md#supported-libraries).

## カスタム PySparkライブラリをインストールできますか？

残念ながら、PySpark カーネルの追加ライブラリはインストールできません。ただし、アドビのカスタマーサービス担当者に連絡して、カスタム PySpark ライブラリをインストールしてもらうことができます。

事前インストールされている PySpark ライブラリのリストについては、[JupyterLab ユーザーガイドの付録の節](./jupyterlab/overview.md#supported-libraries)を参照してください。

## Is it possible to configure [!DNL Spark] cluster resources for [!DNL JupyterLab] [!DNL Spark] or PySpark kernel?

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

For more information on [!DNL Spark] cluster resource configuration, including the complete list of configurable properties, see the [JupyterLab User Guide](./jupyterlab/overview.md#kernels).

## 大きなデータセットに対して特定のタスクを実行しようとするとエラーが発生するのはなぜですか。

例えば、 `Reason: Remote RPC client disassociated. Likely due to containers exceeding thresholds, or network issues.` This is the driver or a executor is running of memory. データ制限の詳細および大きなデータセットでタスクを実行する方法については、JupyterLab Notebooks [data access](./jupyterlab/access-notebook-data.md) documentationを参照してください。 通常、このエラーは、をからに変更することで解決でき `mode` ま `interactive``batch`す。
---
title: Jupyter ノートブックをクエリサービスに接続
description: Jupyter Notebook をAdobe Experience Platform Query Service に接続する方法を説明します。
source-git-commit: af37fe3be6b9645965b7477b9b85c5e11fe6fbae
workflow-type: tm+mt
source-wordcount: '615'
ht-degree: 3%

---

# 接続 [!DNL Jupyter Notebook] クエリサービスへ

このドキュメントでは、接続に必要な手順を説明します [!DNL Jupyter Notebook] をAdobe Experience Platform Query Service と連携させることができます。

## はじめに

このガイドでは、 [!DNL Jupyter Notebook] そしてそのインターフェイスに詳しい ダウンロードするには [!DNL Jupyter Notebook] 詳しくは、 [公式 [!DNL Jupyter Notebook] ドキュメント](https://jupyter.org/).

接続に必要な資格情報を取得するには [!DNL Jupyter Notebook] をExperience Platformするには、 [!UICONTROL クエリ] ワークスペース（Platform UI 内） 現在、 [!UICONTROL クエリ] ワークスペース。

>[!TIP]
>
>[!DNL Anaconda Navigator] は、共通のを簡単にインストールして起動できるデスクトップグラフィカルユーザーインターフェイス (GUI) です [!DNL Python] 次のようなプログラム： [!DNL Jupyter Notebook]. また、コマンドラインコマンドを使用せずに、パッケージ、環境、チャネルを管理する場合にも役立ちます。
>以下が可能です。 [アプリケーションの優先バージョンのインストール](https://docs.anaconda.com/anaconda/install/) 彼らのウェブサイトから
>ガイド付きのインストール手順に従います。 Anaconda Navigator のホーム画面で、 **[!DNL Jupyter Notebook]** をクリックして、サポートされているアプリケーションのリストからプログラムを起動します。
>![この [!DNL Anaconda Navigator] ホーム画面 [!DNL Jupyter Notebook] ハイライト表示されました。](../images/clients/jupyter-notebook/anaconda-navigator-home.png)
>詳しくは、 [公式ドキュメント](https://docs.anaconda.com/anaconda/navigator/).

## Launch [!DNL Jupyter Notebook]

新しい [!DNL Jupyter Notebook] Web アプリケーションで、 **[!DNL New]** ドロップダウンが表示されます。 **[!DNL Python 3]** をクリックして、新しいノートブックを作成します。 この [!DNL Notebook] エディタが表示されます。

![この [!DNL Jupiter Notebook] 「ファイル」タブで [!DNL New] ドロップダウンと [!DNL Python] 3 個がハイライト表示されています。](../images/clients/jupyter-notebook/new-notebook.png)

最初の行に [!DNL Notebook] エディターで、次の値を入力します。 `pip install psycopg2-binary` を選択し、 **[!DNL Run]** コマンドバーから。 成功メッセージが入力行の下に表示されます。

>[!IMPORTANT]
>
>接続を形成するには、このプロセスの一部として、 **[!DNL Run]** コードの各行を実行します。

![この [!DNL Notebook] install libraries コマンドをハイライト表示した UI。](../images/clients/jupyter-notebook/install-library.png)

次に、 [!DNL PostgreSQL] のデータベースアダプタ [!DNL Python]. 値を入力： `import psycopg2`を選択し、 **[!DNL Run]**. このプロセスに関する成功メッセージはありません。 エラーメッセージが表示されない場合は、次の手順に進みます。

![この [!DNL Notebook] インポートデータベースドライバーコードが強調表示された UI](../images/clients/jupyter-notebook/import-dbdriver.png)

次に、値を入力して、Adobe Experience Platformの資格情報を指定する必要があります。 `conn = psycopg2.connect("{YOUR_CREDENTIALS}")`. 接続資格情報は、 [!UICONTROL クエリ] セクションの [!UICONTROL 資格情報] 」タブを使用します。 方法に関するドキュメントを参照してください。 [組織の資格情報を検索](../ui/credentials.md) を参照してください。

詳細を繰り返し入力する手間を省くために、サードパーティクライアントを使用する場合は、期限切れでない資格情報を使用することをお勧めします。 手順については、ドキュメントを参照してください。 [有効期限のない資格情報を生成して使用する方法](../ui/credentials.md#non-expiring-credentials).

>[!IMPORTANT]
>
>Platform UI から資格情報をコピーする場合は、資格情報の追加の形式がないことを確認します。 プロパティと値の間に 1 つのスペースを入れた 1 行で記述します。 資格情報は引用符で囲まれ、 **not** コンマで区切られます。

![この [!DNL Notebook] 接続資格情報が強調表示された UI。](../images/clients/jupyter-notebook/provide-credentials.png)

お使いの [!DNL Jupyter Notebook] インスタンスがクエリサービスに接続されました。

## クエリ実行の例

これで、 [!DNL Jupyter Notebook] クエリサービスでは、 [!DNL Notebook] 入力 次の例では、単純なクエリを使用してプロセスを示しています。

次の値を入力します。

```console
cur = conn.cursor()
cur.execute('''{YOUR_QUERY_HERE}''')
data = [r for r in cur]
```

次に、パラメーター (`data` 上記の例で ) は、クエリの結果を書式設定されていない応答に表示します。

![この [!DNL Notebook] ノートブック内で SQL 結果を返して表示するコマンドを含む UI。](../images/clients/jupyter-notebook/example-query.png)

より人間が読みやすい方法で結果を書式設定するには、次のコマンドを使用します。

- `colnames = [desc[0] for desc in cur.description]`
- `import pandas as pd`
- `import numpy as np`

これらのコマンドは、成功メッセージを生成しません。 エラーメッセージがない場合は、関数を使用して、SQL クエリの結果を表形式で出力できます。

![SQL 結果のフォーマットに必要なコマンド。](../images/clients/jupyter-notebook/format-results-commands.png)

を入力し、 `df.head()` 関数を使用して、表形式化されたクエリ結果を確認します。

![SQL クエリの結果を表にした [!DNL Jupyter Notebook].](../images/clients/jupyter-notebook/format-results-output.png)

## 次の手順

これで、クエリサービスに接続し、 [!DNL Jupyter Notebook] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。

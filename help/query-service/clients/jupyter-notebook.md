---
title: Jupyter ノートブックをクエリサービスに接続
description: Jupyter Notebook をAdobe Experience Platform Query Service に接続する方法を説明します。
exl-id: 358eab67-538f-4ada-931f-783b92db4a1c
source-git-commit: 1af89160cbf5b689396921869fec6c30a5bcfff0
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 10%

---

# クエリサービスへの [!DNL Jupyter Notebook] の接続

このドキュメントでは、接続に必要な手順を説明します [!DNL Jupyter Notebook] をAdobe Experience Platform Query Service と連携させることができます。

## はじめに

このガイドでは、 [!DNL Jupyter Notebook] そしてそのインターフェイスに詳しい [!DNL Jupyter Notebook] をダウンロードするには、または詳細情報については、[公式 [!DNL Jupyter Notebook] ドキュメント](https://jupyter.org/)を参照してください。

[!DNL Jupyter Notebook] を Experience Platform に接続するために必要な資格情報を取得するには、Platform UI のクエリワークスペースにアクセスできる必要があります。現在、 [!UICONTROL クエリ] ワークスペース。

>[!TIP]
>
>[!DNL Anaconda Navigator] は、共通のを簡単にインストールして起動できるデスクトップグラフィカルユーザーインターフェイス (GUI) です [!DNL Python] 次のようなプログラム： [!DNL Jupyter Notebook]. また、コマンドラインコマンドを使用せずに、パッケージ、環境、チャネルを管理する場合にも役立ちます。
>Web サイト上のガイド付きインストールプロセスに従って、 [アプリケーションの優先バージョンのインストール](https://docs.anaconda.com/anaconda/install/).
>Anaconda Navigator のホーム画面で、 **[!DNL Jupyter Notebook]** をクリックして、サポートされているアプリケーションのリストからプログラムを起動します。
>詳しくは、 [アナコンダ公式文書](https://docs.anaconda.com/anaconda/navigator/).

手順については、Jupyter の公式ドキュメントを参照してください。 [コマンドラインインターフェイスからノートブックを実行する](https://docs.jupyter.org/en/latest/running.html#how-do-i-open-a-specific-notebook) (CLI)。

## Launch [!DNL Jupyter Notebook]

新しい [!DNL Jupyter Notebook] Web アプリケーションで、 **[!DNL New]** UI からのドロップダウン ( **[!DNL Python 3]** をクリックして、新しいノートブックを作成します。 この [!DNL Notebook] エディタが表示されます。

最初の行に [!DNL Notebook] エディターで、次の値を入力します。 `pip install psycopg2-binary` を選択し、 **[!DNL Run]** コマンドバーから。 成功メッセージが入力行の下に表示されます。

>[!IMPORTANT]
>
>接続を形成するには、このプロセスの一部として、 **[!DNL Run]** コードの各行を実行します。

次に、 [!DNL PostgreSQL] のデータベースアダプタ [!DNL Python]. 値を入力： `import psycopg2`を選択し、 **[!DNL Run]**. このプロセスに関する成功メッセージはありません。 エラーメッセージが表示されない場合は、次の手順に進みます。

次に、値を入力して、Adobe Experience Platformの資格情報を指定する必要があります。 `conn = psycopg2.connect("{YOUR_CREDENTIALS}")`. 接続資格情報は、 [!UICONTROL クエリ] セクションの [!UICONTROL 資格情報] 」タブを使用します。 方法に関するドキュメントを参照してください。 [組織の資格情報を検索](../ui/credentials.md) を参照してください。

詳細を繰り返し入力する手間を省くために、サードパーティクライアントを使用する場合は、期限切れでない資格情報を使用することをお勧めします。 手順については、ドキュメントを参照してください。 [有効期限のない資格情報を生成して使用する方法](../ui/credentials.md#non-expiring-credentials).

>[!IMPORTANT]
>
>Platform UI から資格情報をコピーする場合、資格情報を追加の形式にする必要はありません。 1 行で指定でき、プロパティと値の間には 1 つのスペースを入れます。 資格情報は引用符で囲まれ、 **not** コンマで区切られます。

```python
conn = psycopg2.connect('''sslmode=require host=<YOUR_HOST_CREDENTIAL> port=80 dbname=prod:all user=<YOUR_ORGANIZATION_ID> password=<YOUR_PASSWORD>''')"
```

お使いの [!DNL Jupyter Notebook] インスタンスがクエリサービスに接続されました。

## クエリ実行の例

これで、 [!DNL Jupyter Notebook] クエリサービスでは、 [!DNL Notebook] 入力 次の例では、単純なクエリを使用してプロセスを示しています。

次の値を入力します。

```python
cur = conn.cursor()
cur.execute('''<YOUR_QUERY_HERE>''')
data = [r for r in cur]
```

次に、パラメーター (`data` 上記の例で ) は、クエリの結果を書式設定されていない応答に表示します。

より人間が読みやすい方法で結果を書式設定するには、次のコマンドを使用します。

- `colnames = [desc[0] for desc in cur.description]`
- `import pandas as pd`
- `import numpy as np`
- `df = pd.DataFrame(samples,columns=colnames)`
- `df.fillna(0,inplace=True)`

これらのコマンドは、成功メッセージを生成しません。 エラーメッセージがない場合は、関数を使用して、SQL クエリの結果を表形式で出力できます。

を入力し、 `df.head()` 関数を使用して、表形式化されたクエリ結果を確認します。

## 次の手順

これで、クエリサービスに接続し、 [!DNL Jupyter Notebook] クエリを書き込みます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。

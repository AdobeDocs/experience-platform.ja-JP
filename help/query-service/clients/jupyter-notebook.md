---
title: Jupyter Notebook のクエリサービスへの接続
description: Jupyter Notebook をAdobe Experience Platform クエリサービスに接続する方法を説明します。
exl-id: 358eab67-538f-4ada-931f-783b92db4a1c
source-git-commit: 1af89160cbf5b689396921869fec6c30a5bcfff0
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 4%

---

# クエリサービスへの [!DNL Jupyter Notebook] の接続

このドキュメントでは、[!DNL Jupyter Notebook] をAdobe Experience Platform クエリサービスに接続するために必要な手順について説明します。

## はじめに

このガイドでは、[!DNL Jupyter Notebook] へのアクセス権を既に持ち、そのインターフェイスについて理解している必要があります。 [!DNL Jupyter Notebook] をダウンロードするには、または詳細情報については、[official [!DNL Jupyter Notebook] documentation](https://jupyter.org/) を参照してください。

[!DNL Jupyter Notebook] を Platform に接続するために必要な資格情報を取得するには、Experience Platform UI の [!UICONTROL  クエリ ] ワークスペースにアクセスできる必要があります。 現在、[!UICONTROL  クエリ ] ワークスペースにアクセスできない場合は、組織の管理者にお問い合わせください。

>[!TIP]
>
>[!DNL Anaconda Navigator] は、[!DNL Jupyter Notebook] などの一般的な [!DNL Python] プログラムのインストールと起動を簡単に行えるデスクトップ グラフィカル ユーザーインターフェイス （GUI）です。 また、コマンドラインコマンドを使用せずにパッケージ、環境およびチャネルを管理する場合にも役立ちます。
>Web サイトのガイド付きインストールプロセスに従って、[ 好みのバージョンのアプリケーションをインストール ](https://docs.anaconda.com/anaconda/install/) します。
>Anaconda Navigator のホーム画面で、サポートされているアプリケーションのリストから **[!DNL Jupyter Notebook]** を選択して、プログラムを起動します。
>詳しくは、[ アナコンダの公式ドキュメント ](https://docs.anaconda.com/anaconda/navigator/) を参照してください。

Jupyter の公式ドキュメントには、[ コマンドラインインターフェイスからノートブックを実行する ](https://docs.jupyter.org/en/latest/running.html#how-do-i-open-a-specific-notebook) （CLI）手順が記載されています。

## Launch [!DNL Jupyter Notebook]

新しい [!DNL Jupyter Notebook] web アプリケーションを開いたら、UI から **[!DNL New]** ドロップダウンを選択し、続いて「**[!DNL Python 3]**」を選択して、新しいノートブックを作成します。 [!DNL Notebook] エディターが表示されます。

[!DNL Notebook] エディターの最初の行に値 `pip install psycopg2-binary` を入力し、コマンドバーから **[!DNL Run]** を選択します。 成功メッセージが入力行の下に表示されます。

>[!IMPORTANT]
>
>接続を形成するためのこのプロセスの一環として、コードの各行を実行する **[!DNL Run]** を選択する必要があります。

次に、[!DNL Python] 用の [!DNL PostgreSQL] データベース アダプタをインポートします。 値を `import psycopg2` と入力し、「**[!DNL Run]**」を選択します。 このプロセスの成功メッセージはありません。 エラーメッセージが表示されない場合は、次の手順に進みます。

ここで、値 `conn = psycopg2.connect("{YOUR_CREDENTIALS}")` を入力してAdobe Experience Platform資格情報を指定する必要があります。 接続資格情報は、Platform UI の「[!UICONTROL  資格情報 ]」タブの下の「[!UICONTROL  クエリ ]」セクションにあります。 手順について詳しくは、[ 組織の資格情報の検索 ](../ui/credentials.md) 方法に関するドキュメントを参照してください。

サードパーティクライアントを使用して詳細を繰り返し入力する手間を省く場合は、有効期限のない資格情報を使用することをお勧めします。 [ 有効期限のない資格情報の生成方法と使用方法 ](../ui/credentials.md#non-expiring-credentials) の手順については、ドキュメントを参照してください。

>[!IMPORTANT]
>
>Platform UI から資格情報をコピーする場合、資格情報の追加の書式設定は必要ありません。 プロパティと値の間に 1 つのスペースを空けて、1 行で指定できます。 資格情報は引用符で囲み、コンマで **区切り** はありません。

```python
conn = psycopg2.connect('''sslmode=require host=<YOUR_HOST_CREDENTIAL> port=80 dbname=prod:all user=<YOUR_ORGANIZATION_ID> password=<YOUR_PASSWORD>''')"
```

これで、[!DNL Jupyter Notebook] インスタンスがクエリサービスに接続されました。

## クエリ実行の例

[!DNL Jupyter Notebook] をクエリサービスに接続したので、[!DNL Notebook] 入力を使用してデータセットに対してクエリを実行できます。 次の例では、単純なクエリを使用してプロセスを示しています。

次の値を入力します。

```python
cur = conn.cursor()
cur.execute('''<YOUR_QUERY_HERE>''')
data = [r for r in cur]
```

次に、パラメーター（上記の例では `data`）を呼び出して、クエリの結果をフォーマットされていない応答で表示します。

人間が読みやすい方法で結果を書式設定するには、次のコマンドを使用します。

- `colnames = [desc[0] for desc in cur.description]`
- `import pandas as pd`
- `import numpy as np`
- `df = pd.DataFrame(samples,columns=colnames)`
- `df.fillna(0,inplace=True)`

これらのコマンドは、成功メッセージを生成しません。 エラーメッセージがない場合は、関数を使用して SQL クエリの結果をテーブル形式で出力できます。

`df.head()` 関数を入力および実行して、表形式のクエリ結果を確認します。

## 次の手順

クエリサービスに接続したので、[!DNL Jupyter Notebook] を使用してクエリを記述できます。 クエリの書き込みおよび実行方法について詳しくは、『[クエリ実行ガイド](../best-practices/writing-queries.md)』を参照してください。

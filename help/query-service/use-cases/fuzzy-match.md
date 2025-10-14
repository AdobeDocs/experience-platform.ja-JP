---
title: クエリサービスのあいまい一致
description: 選択した文字列にほぼ一致することで、複数のデータセットからの結果を組み合わせるExperience Platform データに対して一致を実行する方法を説明します。
exl-id: ec1e2dda-9b80-44a4-9fd5-863c45bc74a7
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 0%

---

# クエリサービスのあいまい一致

Adobe Experience Platform データに対して「ファジー」一致を使用すると、最も可能性の高い近似一致を返すことができ、同じ文字を含む文字列を検索する必要はありません。 これにより、データをより柔軟に検索でき、時間と労力を節約してデータにアクセスしやすくなります。

あいまい一致は、検索文字列を一致させるために再書式設定を試みるのではなく、2 つのシーケンス間の類似性の割合を分析し、類似性の割合を返します。 [[!DNL FuzzyWuzzy]](https://pypi.org/project/fuzzywuzzy/) の関数は、[!DNL regex] や [!DNL difflib] に比べてより複雑な状況で文字列を一致させるのに役立つので、このプロセスにお勧めします。

このユースケースで示す例では、2 つの異なる旅行代理店データセットをまたいでホテルの部屋検索から類似した属性を照合することに焦点を当てています。 このドキュメントでは、大きな異なるデータソースからの類似度に基づいて文字列を照合する方法を示しています。 この例では、あいまい一致で Luma と Acme の旅行代理店の部屋の機能の検索結果を比較しています。

## はじめに {#getting-started}

このプロセスの一部として機械学習モデルのトレーニングが必要なため、このドキュメントでは 1 つ以上の機械学習環境に関する実務知識を前提としています。

この例では、[!DNL Python] と [!DNL Jupyter Notebook] 開発環境を使用します。 使用できるオプションは多数ありますが、[!DNL Jupyter Notebook] れはオープンソースの web アプリケーションであり、計算要件が低いので、推奨されます。 これは、[&#x200B; 公式 Jupyter サイト &#x200B;](https://jupyter.org/) からダウンロードできます。

開始する前に、必要なライブラリを読み込む必要があります。 [!DNL FuzzyWuzzy] は、[!DNL difflib] ライブラリの上に構築されたオープンソース [!DNL Python] ライブラリで、文字列の照合に使用されます。 [!DNL Levenshtein Distance] を使用して、シーケンスとパターンの違いを計算します。 [!DNL FuzzyWuzzy] には以下の要件があります。

- [!DNL Python] 2.4 （またはそれ以降）
- [!DNL Python-Levenshtein]

コマンドラインから、次のコマンドを使用して [!DNL FuzzyWuzzy] をインストールします。

```console
pip install fuzzywuzzy
```

または、次のコマンドを使用して、[!DNL Python-Levenshtein] もインストールします。

```console
pip install fuzzywuzzy[speedup]
```

[!DNL Fuzzywuzzy] に関する技術情報については、こちらの [&#x200B; 公式ドキュメント &#x200B;](https://pypi.org/project/fuzzywuzzy/) を参照してください。

### クエリサービスへの接続

接続資格情報を指定して、機械学習モデルをクエリサービスに接続する必要があります。 有効期限のある資格情報と有効期限のない資格情報の両方を指定できます。 必要な資格情報の取得方法について詳しくは、[&#x200B; 資格情報ガイド &#x200B;](../ui/credentials.md) を参照してください。 [!DNL Jupyter Notebook] を使用している場合は、[&#x200B; クエリサービスへの接続方法 &#x200B;](../clients/jupyter-notebook.md) に関する完全なガイドを参照してください。

また、線形代数を有効にするには、必ず [!DNL numpy] パッケージを [!DNL Python] 環境に読み込みます。

```python
import numpy as np
```

[!DNL Jupyter Notebook] からクエリサービスに接続するには、次のコマンドが必要です。

```python
import psycopg2
conn = psycopg2.connect('''
sslmode=require
host=<YOUR_ORGANIZATION_ID>
port=80
dbname=prod:all
user=<YOUR_ADOBE_ID_TO_CONNECT_TO_QUERY_SERVICE>
password=<YOUR_QUERY_SERVICE_PASSWORD>
''')
cur = conn.cursor()
```

これで、[!DNL Jupyter Notebook] インスタンスがクエリサービスに接続されました。 接続に成功した場合、メッセージは表示されません。 接続に失敗した場合は、エラーが表示されます。

### Luma データセットからのデータの描画 {#luma-dataset}

次のコマンドを使用して、分析用のデータを最初のデータセットから取得します。 簡潔にするために、例は列の最初の 10 個の結果に制限されています。

```python
cur.execute('''SELECT * FROM luma;
''')    
luma = np.array([r[0] for r in cur])

luma[:10]
```

「**出力**」を選択すると、返された配列が表示されます。

+++出力

```console
array(['Deluxe King Or Queen Room', 'Kona Tower City / Mountain View',
       'Luxury Double Room', 'Alii Tower Ocean View With King Bed',
       'Club Two Queen', 'Corner Deluxe Studio',
       'Luxury Queen Room With Two Queen Beds', 'Grand Corner King Room',
       'Accessible Club Ocean View Suite With One King Bed',
       'Junior Suite'], dtype='<U66')
```

+++

### Acme データセットからのデータの描画 {#acme-dataset}

次のコマンドを使用して、分析用のデータを 2 番目のデータセットから取得できるようになりました。 ここでも、簡潔にするために、例は列の最初の 10 個の結果に制限されています。

```python
cur.execute('''SELECT * FROM acme;
''')    
acme = np.array([r[0] for r in cur])

acme[:10]
```

「**出力**」を選択すると、返された配列が表示されます。

+++出力

```console
array(['Deluxe King Or Queen Room', 'Kona Tower City / Mountain View',
       'Luxury Double Room', 'Alii Tower Ocean View With King Bed',
       'Club Two Queen', 'Corner Deluxe Studio',
       'Luxury Queen Room With Two Queen Beds', 'Grand Corner King Room',
       'Accessible Club Ocean View Suite With One King Bed',
       'Junior Suite'], dtype='<U66')
```

+++

### ファジースコア関数の作成 {#fuzzy-scoring}

次に、FuzzyWuzzy ライブラリから `fuzz` を読み込み、文字列の部分比比較を実行する必要があります。 部分比率関数を使用すると、部分文字列のマッチングを実行できます。 これは最短の文字列を取り、同じ長さのすべての部分文字列と照合します。 この関数は、最大 100% の類似性の割合を返します。 例えば、部分比率関数は、次の文字列「Deluxe Room」、「1 King Bed」、「Deluxe King Room」を比較し、類似性スコア 69% を返します。

ホテル ルーム マッチのユースケースでは、これは次のコマンドを使用して行われます。

```python
from fuzzywuzzy import fuzz
def compute_match_score(x,y):
    return fuzz.partial_ratio(x,y)
```

次に、[!DNL SciPy] ライブラリから `cdist` を読み込んで、2 つの入力コレクション内の各ペア間の距離を計算します。 これは、各旅行代理店から提供されたホテルの部屋のすべてのペアのスコアを計算します。

```python
from scipy.spatial.distance import cdist
pairwise_distance =  cdist(luma.reshape((-1,1)),acme.reshape((-1,1)),compute_match_score)
```

### あいまい結合スコアを使用して 2 つの列間にマッピングを作成します

距離に基づいて列がスコアリングされたので、ペアのインデックスを作成し、特定のパーセンテージより高いスコアリングの一致のみを保持できます。 この例では、70% 以上のスコアに一致したペアのみを保持します。

```python
matched_pairs = []
for i,c1 in enumerate(luma):
    idx = np.where(pairwise_distance[i,:] > 70)[0]
    for j in idx:
        matched_pairs.append((luma[i].replace("'","''"),acme[j].replace("'","''")))
```

結果は、次のコマンドで表示できます。 簡潔にするために、結果は 10 行に制限されています。

```python
matched_pairs[:10]
```

**出力** を選択して、結果を確認します。

+++出力

```console
[('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Standard Room, Lagoon View', 'Standard Room With Ocean View'),
 ('Standard Room, Lagoon View', 'Standard Room Dolphin Lagoon View'),
 ('Deluxe Room, 2 Queen Beds', 'Deluxe Room - Two Queen Beds'),
 ('Deluxe Room, 2 Queen Beds', 'Deluxe Room - One King Bed'),
 ('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Premier Room, 1 King Bed', 'Royal Club Premier Room - One King Bed'),
 ('Deluxe Room, Corner', 'Deluxe Room (Non Refundable)'),
 ('Deluxe Suite', 'Corner Deluxe Studio')]
```

+++

次に、SQL で次のコマンドを使用して、結果を照合します。

<!-- Q) Why and is this accurate? -->

```python
matching_sql = ' OR '.join(["(e.luma = '{}' AND b.acme = '{}')".format(c1,c2) for c1,c2 in matched_pairs])
```

## マッピングを適用してクエリサービスであいまい結合を行う {#mappings-for-query-service}

次に、高スコアの一致するペアを SQL を使用して結合し、新しいデータセットを作成します。

```python
:
cur.execute('''
SELECT *  FROM luma e
CROSS JOIN acme b
WHERE 
{}
'''.format(matching_sql)) 
[r for r in cur]
```

「**出力**」を選択して、この結合の結果を確認します。

+++出力

```console
[('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Standard Room, Lagoon View', 'Standard Room With Ocean View'),
 ('Standard Room, Lagoon View', 'Standard Room Dolphin Lagoon View'),
 ('Deluxe Room, 2 Queen Beds', 'Deluxe Room - Two Queen Beds'),
 ('Deluxe Room, 2 Queen Beds', 'Deluxe Room - One King Bed'),
 ('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Premier Room, 1 King Bed', 'Royal Club Premier Room - One King Bed'),
 ('Deluxe Room, Corner', 'Deluxe Room (Non Refundable)'),
 ('Deluxe Suite', 'Corner Deluxe Studio'),
 ('Deluxe Suite', 'Deluxe Suite'),
 ('Deluxe Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Club Room, 2 Queen Beds', 'Deluxe Room - Two Queen Beds'),
 ('Business Double Room, 2 Double Beds', 'Double Room with Two Double Beds'),
 ('Business Double Room, 2 Double Beds', 'Double Room with Two Double Beds'),
 ('Business Double Room, 2 Double Beds', 'Double Room with Two Double Beds'),
 ('Business Double Room, 2 Double Beds', 'Business King Room'),
 ('Business Double Room, 2 Double Beds', 'Double Room with Two Double Beds'),
 ('Business Double Room, 2 Double Beds',
  'Business Double Room With Two Double Beds'),
 ('Business Double Room, 2 Double Beds', 'Deluxe Double Room'),
 ('Traditional Double Room, 2 Double Beds',
  'Double Room with Two Double Beds'),
 ('Traditional Double Room, 2 Double Beds',
  'Double Room with Two Double Beds'),
 ('Traditional Double Room, 2 Double Beds',
  'Double Room with Two Double Beds'),
 ('Traditional Double Room, 2 Double Beds',
  'Double Room with Two Double Beds'),
 ('Deluxe Suite, 1 Bedroom', 'Deluxe Suite'),
 ('City Room, City View', 'Room With City View'),
 ('City Room, City View', 'Queen Room With City View'),
 ('City Room, City View', 'Club Level King Or Queen Room with City View'),
 ('Club Room, Premium 2 Queen Beds', 'Club Premium Two Queen'),
 ('Club Room, Premium 2 Queen Beds', 'Premium Two Queen'),
 ('Deluxe Room, Lake View', 'Deluxe King Or Queen Room with Lake View'),
 ('King Room, Suite, 1 King Bed with Sofa bed', 'King Room'),
 ('King Room, Suite, 1 King Bed with Sofa bed', 'King Room'),
 ('King Room, Suite, 1 King Bed with Sofa bed', 'King Room'),
 ('Deluxe Suite, 1 King Bed, Non Smoking, Kitchen', 'Deluxe Suite'),
 ('Junior Suite, 1 King Bed, Accessible (Roll-in Shower)', 'Junior Suite'),
 ('Regency Club, Mountain View', 'Regency Club Ocean View'),
 ('Regency Club, Mountain View', 'Regency Club Mountain View'),
 ('Club Room, 2 Queen Beds', 'Deluxe Room - Two Queen Beds'),
 ('Room, 2 Queen Beds, City View',
  'Queen Room With Two Queen Beds and City View'),
 ('Deluxe Room', 'Queen Room'),
 ('Deluxe Room', 'Deluxe Room (Non Refundable)'),
 ('Deluxe Room', 'Deluxe Room - Two Queen Beds'),
 ('Deluxe Room', 'Deluxe Room - One King Bed'),
 ('Room, Partial Ocean View', 'Room With Ocean View'),
 ('Room, Partial Ocean View', 'Partial Ocean View With Two Double Beds'),
 ('Room, Partial Ocean View', 'Kona Tower Partial Ocean View'),
 ('Room, Partial Ocean View', 'Partial Ocean View Room'),
 ('Room, Partial Ocean View', 'Waikiki Tower Partial Ocean View'),
 ('Premium Room, 1 King Bed', 'Royal Club Premier Room - One King Bed'),
 ('Grand Corner King Room, 1 King Bed', 'Grand Corner King Room'),
 ('Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Room, 1 King Bed', 'Ocean View Room With King Bed'),
 ('Room, 1 King Bed', 'Royal Club Premier Room - One King Bed'),
 ('Deluxe Room, 1 King Bed, Non Smoking', 'Deluxe Room - One King Bed'),
 ('Room, 2 Double Beds, Accessible, Partial Ocean View',
  'Accessible Partial Ocean View With Two Double Beds'),
 ('Room, 2 Double Beds, Accessible, Partial Ocean View',
  'Partial Ocean View Room'),
 ('Room, Ocean View ', 'Room With Ocean View'),
 ('Room, Ocean View ', 'King Or Two Queen Room With Ocean View'),
 ('Room, Ocean View ', 'Standard Room With Ocean View'),
 ('Signature Suite, 1 Bedroom', 'Signature King'),
 ('Room, 2 Queen Beds (Waikiki View)',
  'Queen Room With Two Queen Beds and Waikiki View'),
 ('Deluxe Room', 'Queen Room'),
 ('Deluxe Room', 'Deluxe Room (Non Refundable)'),
 ('Deluxe Room', 'Deluxe Room - Two Queen Beds'),
 ('Deluxe Room', 'Deluxe Room - One King Bed'),
 ('Standard Room, Oceanfront', 'Standard Room With Ocean View'),
 ('Standard Room, Oceanfront', 'Standard Room With Ocean Front View'),
 ('Standard Room, Mountain View (City View - Kona Tower) - No Resort Fee',
  'Standard Room With Mountain View'),
 ('Standard Room, Mountain View (City View - Kona Tower) - No Resort Fee',
  'Standard Room With Ocean View'),
 ('High-Floor Premium Room, 1 King Bed', 'High-Floor Premium King Room'),
 ('Club Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Junior Suite, 1 King Bed with Sofa Bed', 'Junior Suite'),
 ('Junior Suite, 1 King Bed with Sofa Bed', 'Deluxe King Suite With Sofa Bed'),
 ('Deluxe Room, City View', 'Queen Room With City View'),
 ('Deluxe Room, City View', 'Club Level King Or Queen Room with City View'),
 ('Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Room, 1 King Bed', 'Ocean View Room With King Bed'),
 ('Room, 1 King Bed', 'Royal Club Premier Room - One King Bed'),
 ('Room, 2 Double Beds, Partial Ocean View', 'Kona Tower Partial Ocean View'),
 ('Room, 2 Double Beds, Partial Ocean View', 'Partial Ocean View Room'),
 ('Room, 1 Queen Bed, City View',
  'Queen Room With Two Queen Beds and City View'),
 ('Room, Ocean View', 'Room With Ocean View'),
 ('Room, Ocean View', 'King Or Two Queen Room With Ocean View'),
 ('Room, Ocean View', 'Standard Room With Ocean View'),
 ('Standard Room, Partial Ocean View (Kona Tower) - No Resort Fee',
  'Partial Ocean View Room'),
 ('Standard Room, Partial Ocean View (Kona Tower) - No Resort Fee',
  'Standard Room With Ocean View'),
 ('Standard Room, Partial Ocean View (Kona Tower) - No Resort Fee',
  'Standard Room With Ocean Front View'),
 ('Standard Room, Ocean View (Waikiki Tower) - No Resort Fee',
  'Standard Room With Ocean View'),
 ('Standard Room, Partial Ocean View (Waikiki Tower) - No Resort Fee',
  'Standard Room With Ocean View'),
 ('Standard Room, Partial Ocean View (Waikiki Tower) - No Resort Fee',
  'Standard Room With Ocean Front View'),
 ('Regency Club, Ocean View',
  'Accessible Club Ocean View Suite With One King Bed'),
 ('Regency Club, Ocean View', 'Regency Club Ocean View'),
 ('Regency Club, Ocean View', 'Regency Club Mountain View'),
 ('Standard Room, Mountain View (Scenic)', 'Standard Room With Mountain View'),
 ('Standard Room, Mountain View (Scenic)', 'Standard Room With Ocean View'),
 ('Room, 1 Queen Bed', 'Deluxe Room - Two Queen Beds'),
 ('Double Room', 'Luxury Double Room'),
 ('Double Room', 'Double Room with Two Double Beds'),
 ('Double Room', 'Queen Room'),
 ('Double Room', 'Double Room with Two Double Beds'),
 ('Double Room', 'Double Room with Two Double Beds'),
 ('Double Room', 'Double Room with Two Double Beds'),
 ('Double Room', 'Business Double Room With Two Double Beds'),
 ('Double Room', 'Deluxe Double Room'),
 ('Club Room, 1 King Bed', 'Deluxe Room - One King Bed'),
 ('Premier Twin Room', 'High-Floor Premium King Room'),
 ('Premier Twin Room', 'Premier King Room'),
 ('Premier Twin Room', 'Premier Queen Room With Two Queen Beds'),
 ('Premier Twin Room', 'Premium King Room With Free Wi-Fi'),
 ('Premium Room, 1 Queen Bed', 'Premium Two Queen'),
 ('Premium Room, 2 Queen Beds', 'Premium Two Queen'),
 ('Deluxe Room, 1 Queen Bed (High Floor)', 'Deluxe Room - Two Queen Beds'),
 ('Room, 2 Queen Beds, Garden View',
  'Queen Room With Two Queen Beds and Garden View'),
 ('Signature Room, 2 Queen Beds', 'Deluxe Room - Two Queen Beds'),
 ('Signature Room, 2 Queen Beds', 'Signature Two Queen'),
 ('Standard Room, Ocean View', 'Room With Ocean View'),
 ('Standard Room, Ocean View', 'Standard Room With Ocean View'),
 ('Standard Room, Ocean View', 'Standard Room With Ocean Front View')]
```

+++

### あいまい一致結果をExperience Platformに保存 {#save-to-platform}

最後に、あいまい一致の結果をデータセットとして保存し、SQL を使用してAdobe Experience Platformで使用できます。

```python
cur.execute(''' 
Create table luma_acme_join
AS
(SELECT *  FROM luma e
CROSS JOIN acme b
WHERE 
{})
'''.format(matching_sql))
```

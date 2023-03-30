---
title: クエリサービスでのあいまい一致
description: 複数のデータセットの結果を組み合わせ、選択した文字列とほぼ一致させる Platform データの照合を実行する方法を説明します。
source-git-commit: a3a4ca4179610348eba73cf1239861265d2bf887
workflow-type: tm+mt
source-wordcount: '804'
ht-degree: 1%

---

# あいまい一致

Platform データに対して「あいまい」一致を使用すると、一致する文字列を検索せずに、最も可能性の高い概算一致を返すことができます。 これにより、より柔軟にデータを検索でき、時間と労力を節約してデータにアクセスしやすくなります。

あいまい一致は、検索文字列を再書式化して一致させる代わりに、2 つのシーケンス間の類似性の比率を分析し、類似性の割合を返します。 [!DNL FuzzyWuzzy] 関数は、より複雑な状況で文字列を一致させるのに役立つので、このプロセスをお勧めします。 [!DNL regex] または [!DNL difflib].

この使用例では、2 つの異なる旅行代理店データセットをまたいで、ホテルの部屋検索から類似した属性を一致させることに焦点を当てています。 このドキュメントでは、大きな別々のデータソースとの類似性の度合いに応じて文字列を一致させる方法を示しています。 この例では、ファジー一致では、Luma および Acme の旅行代理店からの部屋の特徴に関する検索結果を比較します。

## はじめに {#getting-started}

このプロセスの一環として、機械学習モデルをトレーニングする必要があります。このドキュメントでは、1 つ以上の機械学習環境に関する実務知識を前提としています。

この例では、 [!DNL Python] そして [!DNL Jupyter Notebook] 開発環境 多くのオプションを使用できますが、 [!DNL Jupyter Notebook] の計算要件が小さいオープンソース web アプリケーションなので、をお勧めします。 次のことが可能です。 [Jupyter 公式サイトからダウンロード](https://jupyter.org/).

作業を開始する前に、必要なライブラリを読み込む必要があります。 [!DNL FuzzyWuzzy] はオープンソース [!DNL Python] 上に構築されたライブラリ [!DNL difflib] ライブラリに含まれ、文字列の照合に使用されます。 次を使用します。 [!DNL Levenshtein Distance] シーケンスとパターンの違いを計算する。 [!DNL FuzzyWuzzy] には次の要件があります。

- [!DNL Python] 2.4 以降
- [!DNL Python-Levenshtein]

コマンドラインから、次のコマンドを使用して [!DNL FuzzyWuzzy]:

```console
pip install fuzzywuzzy
```

または、次のコマンドを使用して [!DNL Python-Levenshtein] また、

```console
pip install fuzzywuzzy[speedup]
```

詳細な技術情報： [!DNL Fuzzywuzzy] は、 [公式ドキュメント](https://pypi.org/project/fuzzywuzzy/).

### クエリサービスへの  の接続

接続の資格情報を指定して、機械学習モデルをクエリサービスに接続する必要があります。 有効期限が切れる資格情報と期限が切れない資格情報の両方を指定できます。 詳しくは、 [資格情報ガイド](../ui/credentials.md) 必要な資格情報の取得方法の詳細については、を参照してください。 次を使用する場合： [!DNL Jupyter Notebook]詳しくは、 [クエリサービスへの接続方法](../clients/jupyter-notebook.md).

また、必ず [!DNL numpy] を [!DNL Python] 環境で線形代数を有効にします。

```python
import numpy as np
```

次のコマンドは、からクエリサービスに接続するために必要です。 [!DNL Jupyter Notebook]:

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

お使いの [!DNL Jupyter Notebook] インスタンスがクエリサービスに接続されました。 接続に成功した場合は、メッセージは表示されません。 接続に失敗した場合は、エラーが表示されます。

### Luma データセットからのデータの描画 {#luma-dataset}

分析用のデータは、次のコマンドを使用して最初のデータセットから作成されます。 簡潔にするため、例は列の最初の 10 件の結果に制限されています。

```python
cur.execute('''SELECT * FROM luma;
''')    
luma = np.array([r[0] for r in cur])

luma[:10]
```

選択 **出力** をクリックして、返された配列を表示します。

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

### Acme データセットからのデータの取得 {#acme-dataset}

分析用のデータは、次のコマンドを使用して 2 番目のデータセットから取得されるようになりました。 繰り返しますが、簡潔にするために、例は列の最初の 10 件の結果に制限されています。

```python
cur.execute('''SELECT * FROM acme;
''')    
acme = np.array([r[0] for r in cur])

acme[:10]
```

選択 **出力** をクリックして、返された配列を表示します。

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

### あいまいスコア付け関数の作成 {#fuzzy-scoring}

次に、 `fuzz` FuzzyWuzzy ライブラリから取得し、文字列の部分比較を実行します。 部分比関数を使用すると、サブ文字列の照合を実行できます。 これは最も短い文字列を取り出し、同じ長さのすべてのサブ文字列と照合します。 この関数は、最大 100%の割合の類似率を返します。 例えば、部分比関数では、「Deluxe Room」、「1 King Bed」、「Deluxe King Room」という文字列を比較し、類似性スコア 69%を返します。

ホテルルームマッチの使用例では、次のコマンドを使用してこれを行います。

```python
from fuzzywuzzy import fuzz
def compute_match_score(x,y):
    return fuzz.partial_ratio(x,y)
```

次に、インポート `cdist` から [!DNL SciPy] 2 つの入力コレクション内の各ペア間の距離を計算するライブラリ。 これにより、各旅行代理店が提供するすべてのホテルの部屋の中のスコアを計算します。

```python
from scipy.spatial.distance import cdist
pairwise_distance =  cdist(luma.reshape((-1,1)),acme.reshape((-1,1)),compute_match_score)
```

### ファジー結合スコアを使用して 2 つの列間のマッピングを作成

これで、列のスコアが距離に基づいて設定されたので、ペアのインデックスを作成し、特定の割合を超えたスコアを持つ一致のみを保持できます。 この例では、スコアが 70%以上に一致するペアのみが保持されます。

```python
matched_pairs = []
for i,c1 in enumerate(luma):
    idx = np.where(pairwise_distance[i,:] > 70)[0]
    for j in idx:
        matched_pairs.append((luma[i].replace("'","''"),acme[j].replace("'","''")))
```

結果は、次のコマンドを使用して表示できます。 簡潔にするため、結果は 10 行に制限されます。

```python
matched_pairs[:10]
```

選択 **出力** 結果を確認する。

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

次のコマンドで SQL を使用して、結果が照合されます。

<!-- Q) Why and is this accurate? -->

```python
matching_sql = ' OR '.join(["(e.luma = '{}' AND b.acme = '{}')".format(c1,c2) for c1,c2 in matched_pairs])
```

## クエリサービスであいまい結合を行うには、マッピングを適用します {#mappings-for-query-service}

次に、スコアの高い一致ペアを SQL を使用して結合し、新しいデータセットを作成します。

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

選択 **出力** この結合の結果を確認するには、を参照してください。

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

### あいまい一致結果を Platform に保存 {#save-to-platform}

最後に、あいまい一致の結果を、SQL を使用してAdobe Experience Platformで使用するデータセットとして保存できます。

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



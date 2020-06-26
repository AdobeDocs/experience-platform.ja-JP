---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: CSVファイルのXDMスキーマへのマップ
topic: tutorial
translation-type: tm+mt
source-git-commit: 7876e6d52815968802bd73bb5e340c99ea3387a8
workflow-type: tm+mt
source-wordcount: '1354'
ht-degree: 2%

---


# CSVファイルのXDMスキーマへのマップ

CSVデータをに取り込むに [!DNL Adobe Experience Platform]は、データを [!DNL Experience Data Model] (XDM)スキーマにマップする必要があります。 このチュートリアルでは、ユー [!DNL Platform] ザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする方法について説明します。

さらに、このチュートリアルの付録では、 [マッピング関数の使用に関する詳細について説明します](#mapping-functions)。

## はじめに

このチュートリアルでは、次のコンポーネントについて十分に理解している必要があり [!DNL Platform]ます。

- [!DNL Experience Data Model (XDM System)](../../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。
- [!DNL Batch ingestion](../batch-ingestion/overview.md): ユーザーが指定したデータファイルからデータを [!DNL Platform] 取り込む方法。

また、このチュートリアルでは、CSVデータを取り込むデータセットを既に作成している必要があります。 UIでデータセットを作成する手順については、「 [データ取り込みのチュートリアル](./ingest-batch-data.md)」を参照してください。

## 宛先の選択

にログインし [!DNL Adobe Experience Platform](https://platform.adobe.com) 、左のナビゲーションバーから **[!UICONTROL ワークフローを選択して]** ワークフロー ** ワークスペースにアクセスします。

**[!UICONTROL ワークフロー]** 画面の「 **[!UICONTROL Data ingestion]** 」セクションで「 **[!UICONTROL CSVをXDMスキーマにマップ」を選択し、「]** Launch **** Launch」を選択します。

![](../images/tutorials/map-a-csv-file/workflows.png)

「 *[!UICONTROL CSVをXDMに]* マップ」スキーマ *[!UICONTROL ・ワークフローが表示され、]* 宛先手順に進みます。 取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットの使用**

CSVデータを既存のデータセットに取り込むには、「既存のデータセット **[!UICONTROL を使用]**」を選択します。 検索関数を使用して既存のデータセットを取得するか、パネル内の既存のデータセットのリストをスクロールして取得できます。

![](../images/tutorials/map-a-csv-file/use-existing-dataset.png)

CSVデータを新しいデータセットに取り込むには、「新しいデータセットを **[!UICONTROL 作成]** 」を選択し、表示されるフィールドにデータセットの名前と説明を入力します。 検索機能を使用するか、提供されるスキーマのリストをスクロールして、スキーマを選択します。 「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../images/tutorials/map-a-csv-file/create-new-dataset.png)

## 追加データ

*[!UICONTROL 追加]* data stepが表示されます。 CSVファイルを用意されているスペースにドラッグ&amp;ドロップするか、「ファイルを **[!UICONTROL 選択]** 」を選択してCSVファイルを手動で入力します。

![](../images/tutorials/map-a-csv-file/add-data.png)

ファイルがアップロードされると、「 *[!UICONTROL サンプルデータ]* 」セクションが表示され、最初の10行のデータが表示されます。 データが期待どおりにアップロードされたことを確認したら、「 **[!UICONTROL 次へ]**」を選択します。

![](../images/tutorials/map-a-csv-file/sample-data.png)

## CSVフィールドのXDMスキーマフィールドへのマップ

The *[!UICONTROL Mapping]* step appears. CSVファイルの列は「 *[!UICONTROL Source Field]*」の下に表示され、対応するXDMスキーマフィールドは「 *[!UICONTROL Targetフィールド]*」の下に表示されます。 未選択のターゲットフィールドは赤で枠線表示されます。 フィルターフィールドオプションを使用して、使用可能なソースフィールドのリストを絞り込むことができます。

CSV列をXDMフィールドにマップするには、列の対応するターゲットフィールドの横にあるスキーマアイコンを選択します。

![](../images/tutorials/map-a-csv-file/mapping.png)

[ *[!UICONTROL スキーマフィールドの]* 選択]ウィンドウが表示されます。 XDMスキーマの構造に移動して、CSV列のマッピング先のフィールドを探します。 XDMフィールドをクリックして選択し、「 **[!UICONTROL Select]**」をクリックします。

![](../images/tutorials/map-a-csv-file/select-schema-field.png)

「 *[!UICONTROL マッピング]* 」画面が再表示され、選択したXDMフィールドが「 *[!UICONTROL Targetフィールド]*」の下に表示されます。

![](../images/tutorials/map-a-csv-file/field-mapped.png)

特定のCSV列をマップしない場合は、「ターゲット」フィールドの **横にある削除アイコンをクリックして、マッピングを削除できます** 。 「すべてのマッピングを **[!UICONTROL クリア」ボタンを選択して、すべてのマッピングを削除することもできます]**。

![](../images/tutorials/map-a-csv-file/remove-mapping.png)

新しいマッピングを追加する場合は、「 **[!UICONTROL ソースフィールド]***[!UICONTROL 」リストの上部で「]* 新しいマッピング」を選択します。

![](../images/tutorials/map-a-csv-file/add-mapping.png)

フィールドのマッピング時には、入力ソースフィールドに基づいて値を計算する関数を含めることもできます。 詳細については、付録の「 [マッピング関数](#mapping-functions) 」の節を参照してください。

### 追加計算フィールド

計算済みフィールドでは、入力スキーマーの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して、参照しやすくすることができます。

先に進むには、 **[!UICONTROL 追加]** 計算フィールドボタンを選択します。

![](../images/tutorials/map-a-csv-file/add-calculated-field.png)

計算済みフィールド **[!UICONTROL を作成]** パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。 いずれかのタブを選択して、式エディタに関数、フィールドまたは演算子を追加する開始を行います。

![](../images/tutorials/map-a-csv-file/create-calculated-fields.png)

| タブ | 説明 |
| --------- | ----------- |
| フィールド | 「フィールド」タブのリストフィールドと属性は、ソーススキーマで使用できます。 |
| 関数 | 「関数」タブには、データの変換に使用できる関数がリストされます。 |
| 演算子 | 「operators」タブには、データの変換に使用できる演算子がリストされます。 |

中央の式エディターを使用して、手動でフィールド、関数および演算子を追加できます。 式の作成を開始するエディタを選択します。

![](../images/tutorials/map-a-csv-file/expression-editor.png)

「 **[!UICONTROL 保存]** 」を選択して続行します。

マッピング画面が再び開き、新しく作成したソースフィールドが表示されます。 対応するターゲットフィールドを適用し、「 **[!UICONTROL 完了]** 」を選択してマッピングを完了します。

![](../images/tutorials/map-a-csv-file/new-field.png)

## データフローの監視

CSVファイルをマッピングして作成したら、ファイルを介して取り込まれるデータを監視できます。 データフローの監視の詳細については、「ストリーミングデータフローの [監視に関するチュートリアル](../../ingestion/quality/monitor-data-flows.md)」を参照してください。

## 次の手順

このチュートリアルに従うと、フラットなCSVファイルをXDMスキーマに正常にマッピングし、に取り込むことができ [!DNL Platform]ます。 このデータは、などのダウンストリーム [!DNL Platform] サービスで使用できるようになり [!DNL Real-time Customer Profile]ました。 詳しくは、概要を参照し [!DNL Real-time Customer Profile](../../profile/home.md) てください。

## 付録

次の節では、CSV列をXDMフィールドにマッピングするための追加情報を示します。

### マッピング関数

特定のマッピング関数を使用して、ソースフィールドに入力した値に基づいて値を計算および計算できます。 関数を使用するには、「 *[!UICONTROL ソースフィールド]* 」に適切な構文と入力を入力して、関数を入力します。

例えば、 **city** と **country** CSVフィールドを連結し、city **XDMフィールドに割り当てるには、ソースフィールドを次のように設定し**`concat(city, ", ", county)`ます。

![](../images/tutorials/map-a-csv-file/mapping-function.png)

次の表に、サンプル式とその結果生成される出力を含む、サポートされるすべてのマッピング関数をリストします。

| 関数 | 説明 | サンプル式 | サンプル出力 |
| -------- | ----------- | ----------------- | ------------- |
| concat | 指定した文字列を連結します。 | concat(&quot;Hi, &quot;, &quot;there&quot;, &quot;!&quot;) | `"Hi, there!"` |
| 爆発する | 正規表現に基づいて文字列を分割し、部分の配列を返します。 | explode(&quot;Hi, there!&quot;, &quot; &quot;) | `["Hi,", "there"]` |
| instr | サブ文字列の位置/インデックスを返します。 | instr(&quot;adobe<span>.com&quot;, &quot;com&quot;) | 6 |
| replacestr | 元の文字列に検索文字列が存在する場合、その文字列を置き換えます。 | replacestr(&quot;This is a string re test&quot;, &quot;re&quot;, &quot;replace&quot;) | &quot;This is a string replace test&quot; |
| substr | 渡された長さのサブ文字列を返します。 | substr(&quot;This is a substring test&quot;, 7, 8) | &quot; a subst&quot; |
| lower /<br>lcase | 文字列を小文字に変換します。 | lower(&quot;HeLLo&quot;)<br>lcase(&quot;HeLLo&quot;) | &quot;hello&quot; |
| upper /<br>ucase | 文字列を大文字に変換します。 | upper(&quot;HeLLo&quot;)<br>ucase(&quot;HeLLo&quot;) | &quot;HELLO&quot; |
| split | 区切り文字の入力文字列を分割します。 | split(&quot;Hello world&quot;, &quot; &quot;) | `["Hello", "world"]` |
| join | セパレータを使用してオブジェクトのリストを結合します。 | `join(" ", ["Hello", "world"]`) | &quot;Hello world&quot; |
| 合体 | 渡されたリスト内の最初のnull以外のオブジェクトを返します。 | coalesce(null, null, null, &quot;first&quot;, null, &quot;second&quot;) | &quot;first&quot; |
| decode | キーとキーと値のペアを配列としてフラット化したリストを指定すると、キーが見つかった場合は値を返し、配列に存在する場合はデフォルト値を返します。 | decode(&quot;k2&quot;, &quot;k1&quot;, &quot;v1&quot;, &quot;k2&quot;, &quot;v2&quot;, &quot;default&quot;) | &quot;v2&quot; |
| if | 渡されたブール値式を評価し、結果に基づいて指定された値を返します。 | iif(&quot;s&quot;.equalsIgnoreCase(&quot;S&quot;), &quot;True&quot;, &quot;False&quot;) | &quot;True&quot; |
| min | 渡された引数の最小値を返します。 自然順序を使用します。 | min(3, 1, 4) | 1 |
| max | 渡された引数の最大値を返します。 自然順序を使用します。 | max(3, 1, 4) | 4 |
| first | 最初の引数を取得します。 | first(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;1&quot; |
| last | 最後に渡された引数を取得します。 | last(&quot;1&quot;, &quot;2&quot;, &quot;3&quot;) | &quot;3&quot; |
| uuid /<br>guid | 擬似ランダムIDを生成します。 | uuid()<br>guid() | {UNIQUE_ID} |
| now | 現在の時間を取得します。 | now() | `2019-10-23T10:10:24.556-07:00[America/Los_Angeles]` |
| timestamp | 現在のUnix時間を取得します。 | timestamp() | 1571850624571 |
| format | 指定した形式に従って入力日を形式設定します。 | format({DATE}, &quot;yyyy-MM-dd HH:mm:ss&quot;) | &quot;2019-10-23 11:24:35&quot; |
| dformat | 指定した形式に従ってタイムスタンプを日付文字列に変換します。 | dformat(1571829875, &quot;dd-MMM-yyyy hh:mm&quot;) | &quot;2019年10月23日11:24&quot; |
| date | 日付文字列をZonedDateTimeオブジェクト（ISO 8601形式）に変換します。 | date(&quot;23-Oct-2019 11:24&quot;) | &quot;2019-10-23T11:24:00+00:00&quot; |
| date_part | 日付の一部を取得します。 次のコンポーネント値がサポートされています。 <br><br>&quot;year&quot;yy&quot;<br><br>yy&quot;<br><br>quarter&quot;<br>&quot;qquarte&quot;&quot;<br>&quot;qmonth&quot;mm&quot;mm&quot;&quot;mm&quot;mm&quot;&quot;mm&quot;&quot;dm&quot;ym&quot;dm&quot;dy&quot;yody&quot;dy&quot;&quot;yy&quot;dy&quot;y&quot;&quot;dy&quot;y&quot;<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>day&quot;day&quot;day&quot;day&quot;d&quot;day&quot;day&quot;day&quot;d&quot;dw&quot;w&quot;&quot;4&quot;th&quot;hh&quot;12&quot;hh&quot;&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot;h&quot; | date_part(date(&quot;2019-10-17 11:55:12&quot;), &quot;MM&quot;) | 10 |
| set_date_part | 指定した日付のコンポーネントを置き換えます。 次のコンポーネントを使用できます。 <br><br>&quot;year&quot;<br>yy&quot;yy&quot;month&quot;<br>&quot;mm&quot;&quot;mm&quot;&#39;m&quot;day&quot;<br><br>&quot;dd&quot;&quot;dd&quot;&quot;dd&quot;&quot;d&quot;&quot;hd&quot;&quot;h&quot;&quot;h&quot;&quot;mi&quot;<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>&quot;ni&quot;n&quot;n&quot;n&quot;n&quot;s&quot;n&quot;n&quot;s&quot;s&quot; | set_date_part(&quot;m&quot;, 4, date(&quot;2016-11-09T11:44:44.797&quot;) | &quot;2016-04-09T11:44:44.797&quot; |
| make_date_time /<br>make_timestamp | パーツから日付を作成します。 | make_date_time(2019, 10, 17, 11, 55, 12, 999, &quot;America/Los_Angeles&quot;) | `2019-10-17T11:55:12.0&#x200B;00000999-07:00[America/Los_Angeles]` |
| current_timestamp | 現在のタイムスタンプを返します。 | current_timestamp() | 1571850624571 |
| current_date | 時間コンポーネントを含まない現在の日付を返します。 | current_date() | &quot;2019年11月18日&quot; |
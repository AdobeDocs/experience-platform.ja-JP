---
keywords: Experience Platform；ホーム；人気の高いトピック；ストリーミング接続；ストリーミング接続の作成；ui ガイド；チュートリアル；ストリーミング接続の作成；ストリーミング取得；取り込み；
solution: Experience Platform
title: UI を使用した HTTP API ストリーミング接続の作成
topic-legacy: tutorial
type: Tutorial
description: この UI ガイドは、Adobe Experience Platform を使用してストリーミング接続を作成する際に役立ちます。
exl-id: 7932471c-a9ce-4dd3-8189-8bc760ced5d6
source-git-commit: f5d341daffd7d4d77ee816cc7537b0d0c52ca636
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 25%

---


# の作成 [!DNL HTTP API] UI を使用したストリーミング接続

このチュートリアルでは、 [!UICONTROL ソース] ワークスペース。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

## ストリーミング接続の作成

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

以下 **[!UICONTROL ストリーミング]** カテゴリ、選択 **[!UICONTROL HTTP API]** 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/http/catalog.png)

この **[!UICONTROL HTTP API アカウントに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、新しいデータフローを作成する HTTP API アカウントを選択し、「 」を選択します **[!UICONTROL 次へ]** をクリックして続行します。

![既存のアカウント](../../../../images/tutorials/create/http/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。表示される入力フォームで、アカウント名とオプションの説明を入力します。 また、次の設定プロパティを指定するオプションも取得されます。

- **[!UICONTROL 認証]:** このプロパティは、ストリーミング接続で認証を必要とするかどうかを決定します。 認証を実行すると、データは信頼できるソースから収集されます。個人を特定できる情報 (PII) を扱う場合は、このプロパティをオンにする必要があります。 デフォルトでは、このプロパティはオフになっています。
- **[!UICONTROL XDM 互換]:** このプロパティは、このストリーミング接続が XDM スキーマと互換性のあるイベントを送信するかどうかを示します。 デフォルトでは、このプロパティはオフになっています。

終了したら、「 」を選択します。 **[!UICONTROL ソースに接続]** 次に、 **[!UICONTROL 次へ]** をクリックして続行します。

![新規アカウント](../../../../images/tutorials/create/http/new.png)

## データの選択

HTTP API 接続を作成した後、 **[!UICONTROL データを選択]** の手順が表示され、データをアップロードおよびプレビューするためのインターフェイスが提供されます。

選択 **[!UICONTROL ファイルをアップロード]** をクリックしてデータをアップロードします。 または、 [!UICONTROL ファイルをドラッグ&amp;ドロップ] 」セクションに表示されます。

![add-data](../../../../images/tutorials/create/http/add-data.png)

データのアップロード時に、インターフェイスの右側を使用して、ファイル階層をプレビューできます。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![preview-sample-data](../../../../images/tutorials/create/http/preview-sample-data.png)

## XDM スキーマへのデータフィールドのマッピング

この [!UICONTROL マッピング] の手順が表示され、ソースデータを Platform データセットにマッピングするためのインターフェイスが提供されます。

Parquet ファイルは XDM に準拠している必要があり、手動でマッピングを設定する必要はありません。また、CSV ファイルではマッピングを明示的に設定する必要がありますが、マッピングするソースデータフィールドを選択できます。 JSON ファイルは、XDM に対する苦情としてマークされている場合、手動設定は必要ありません。 ただし、XDM 準拠としてマークされていない場合は、マッピングを明示的に設定する必要があります。

取り込むインバウンドデータのデータセットを選択します。 既存のデータセットを使用するか、新しく作成することができます。

### 新しいデータセットの作成

新しいデータセットを作成するには、 **[!UICONTROL 新しいデータセット]**. 表示されるフォームで、名前、説明（オプション）、およびデータセットのターゲットスキーマを指定します。 次を選択した場合、 [!DNL Profile]が有効なスキーマの場合は、データセットを [!DNL Profile]-enabled。

![new-dataset](../../../../images/tutorials/create/http/new-dataset.png)

### 既存のデータセットを使用する

既存のデータセットを使用するには、「 」を選択します。 **[!UICONTROL 既存のデータセット]**. 表示されるフォームで、使用するデータセットを選択します。 データセットを選択したら、データセットを次のいずれかにするかを選択できます [!DNL Profile]-enabled。

![existing-dataset](../../../../images/tutorials/create/http/existing-dataset.png)

### 標準フィールドをマッピング


必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

新しいソースフィールドを追加するには、「 **[!UICONTROL 新しいマッピングを追加]**.

![add-new-mapping](../../../../images/tutorials/create/http/add-new-mapping.png)

新しいソースフィールドとターゲットフィールドの組み合わせが表示されます。 新しいソースフィールドを追加するには、 [!UICONTROL ソースフィールドを選択] 入力バー。

![select-source-field](../../../../images/tutorials/create/http/select-source-field.png)

この [!UICONTROL 属性を選択] パネルを使用すると、ファイル階層を参照し、特定のソースフィールドを選択してターゲット XDM フィールドにマッピングできます。 マッピングするソースフィールドを選択したら、 **[!UICONTROL 選択]** をクリックして続行します。

![select-attributes](../../../../images/tutorials/create/http/select-attributes.png)

ソースフィールドを選択して、マッピング先の適切なターゲット XDM フィールドを識別できるようになりました。 ターゲットフィールドセクションの下のスキーマアイコンを選択します。

![select-target-field](../../../../images/tutorials/create/http/select-target-field.png)

この [!UICONTROL ソースフィールドをターゲットフィールドにマッピング] ウィンドウが表示され、ターゲットデータセットのスキーマを調べるためのインターフェイスが提供されます。 ソースフィールドに一致するターゲットフィールドを選択し、「 」を選択します。 **[!UICONTROL 選択]** をクリックして続行します。

![map-to-target-field](../../../../images/tutorials/create/http/map-to-target-field.png)

ソースフィールドがすべて適切なターゲット XDM フィールドにマッピングされたら、「 」を選択します。 **[!UICONTROL 次へ]**

![data-prep-complete](../../../../images/tutorials/create/http/data-prep-complete.png)

## データフローの詳細

この **[!UICONTROL データフローの詳細]** 手順が表示されます。 このページでは、名前とオプションの説明を入力して、作成したデータフローの詳細を指定できます。

データフローの詳細を指定したら、「 」を選択します。 **[!UICONTROL 次へ]**.

![dataflow-detail](../../../../images/tutorials/create/http/dataflow-detail.png)

## レビュー

この **[!UICONTROL レビュー]** 手順が表示され、作成前にデータフローの詳細を確認できます。 詳細は、次のカテゴリ内にグループ化されます。

- **[!UICONTROL 接続]**:アカウント名、ソースプラットフォーム、およびソース名が表示されます。
- **[!UICONTROL データセットの割り当てとフィールドのマッピング]**:ターゲットデータセットと、データセットが準拠するスキーマを表示します。

詳細が正しいことを確認したら、「 」を選択します。 **[!UICONTROL 完了]**.

![レビュー](../../../../images/tutorials/create/http/review.png)

## ストリーミングエンドポイント URL の取得

接続が作成され、ソースの詳細ページが表示されます。 このページには、以前に実行したデータフロー、ID、ストリーミングエンドポイント URL を含む、新しく作成した接続の詳細が表示されます。

![endpoint](../../../../images/tutorials/create/http/endpoint.png)

## 次の手順

このチュートリアルに従って、ストリーミング HTTP 接続を作成し、ストリーミングエンドポイントを使用して様々な方法でアクセスできるようにしました [!DNL Data Ingestion] API API でストリーミング接続を作成する手順については、[ストリーミング接続の作成に関するチュートリアル](../../../api/create/streaming/http.md)を参照してください。

データを Platform にストリーミングする方法については、次のチュートリアルをお読みください： [時系列データのストリーミング](../../../../../ingestion/tutorials/streaming-time-series-data.md) または [ストリーミングレコードデータ](../../../../../ingestion/tutorials/streaming-record-data.md).

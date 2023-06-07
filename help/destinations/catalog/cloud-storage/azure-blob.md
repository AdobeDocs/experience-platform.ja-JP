---
title: Azure Blob 接続
description: Azure Blob Storage へのライブアウトバウンド接続を作成して、Adobe Experience Platform から CSV データファイルを定期的に書き出します。
exl-id: 8099849b-e3d2-48a5-902a-ca5a5ec88207
source-git-commit: 8890fd137cfe6d35dcf6177b5516605e7753a75a
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 80%

---

# [!DNL Azure Blob] 接続

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>データセットの書き出し機能のベータ版リリースと、ファイルの書き出し機能の改善により、宛先カタログに 2 つの [!DNL Azure Blob] カードが表示される場合があります。
>* **[!UICONTROL Azure Blob]** 宛先に既にファイルを書き出している場合は、新しい **[!UICONTROL Azure Blob ベータ版]** 宛先への新しいデータフローを作成してください。
>* **[!UICONTROL Azure Blob]** 宛先へのデータフローをまだ作成していない場合は、新しい **[!UICONTROL Azure Blob ベータ版]** カードを使用して **[!UICONTROL Azure Blob]** にファイルを書き出してください。


![2 つの Azure Blob 宛先カードの並列表示の画像](../../assets/catalog/cloud-storage/blob/two-azure-blob-destination-cards.png)

新しい [!DNL Azure Blob] 宛先カードの改善点は次のとおりです。

* [データセット書き出しのサポート](/help/destinations/ui/export-datasets.md)。
* 追加の[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)。
* 書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）
* [書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。

## 概要 {#overview}

[!DNL Azure Blob]（以下「[!DNL Blob]」）は、Microsoft のクラウド用オブジェクトストレージソリューションです。このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Blob] 宛先を作成する手順を説明します。

## に接続 [!UICONTROL Azure Blob] API または UI を介したストレージ {#connect-api-or-ui}

* 次に接続するには： [!UICONTROL Azure Blob] ストレージの場所 Platform ユーザーインターフェイスを使用して、「 」セクションを読みます。 [宛先に接続](#connect) および [この宛先へのセグメントのアクティブ化](#activate) 下
* 次に接続するには： [!UICONTROL Azure Blob] ストレージの場所をプログラムで設定し、読み取り [フローサービス API のチュートリアルを使用して、ファイルベースの宛先に対してセグメントをアクティブ化します](../../api/activate-segments-file-based-destinations.md).

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL Blob] 宛先が既にある場合は、このドキュメントの残りの部分をスキップし、[宛先に対するセグメントのアクティブ化](../../ui/activate-batch-profile-destinations.md)に関するチュートリアルに進んで構いません。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## サポートされているファイル形式 {#file-formats}

[!DNL Experience Platform] では、[!DNL Azure Blob] に書き出す次のファイル形式をサポートしています。

* コンマ区切り値（CSV）：書き出しデータファイルのサポートは現在、コンマ区切り値に制限されています。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_blob_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式のキーの例については、以下のドキュメントリンクを参照してください。"

宛先を認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL 接続文字列]**：Blob ストレージのデータにアクセスするには、接続文字列が必要です。[!DNL Blob] 接続文字列のパターンは `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}` で始まります。
   * [!DNL Blob] 接続文字列の設定について詳しくは、Microsoftドキュメントの [Azure Storage アカウントの接続文字列を構成する](https://learn.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account)を参照してください。
* **[!UICONTROL 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。

   ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細の入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーのパスを入力します。
* **[!UICONTROL コンテナ]**：この宛先で使用する [!DNL Azure Blob Storage] コンテナの名前を入力します。
* **[!UICONTROL ファイルタイプ]**:書き出すファイルに使用する形式Experience Platformを選択します。 このオプションは、 **[!UICONTROL Azure Blob ベータ版]** 宛先。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。 このオプションは、 **[!UICONTROL Azure Blob ベータ版]** 宛先。
* **[!UICONTROL マニフェストファイルを含める]**:書き出しの場所や書き出しサイズなどに関する情報を含むマニフェスト JSON ファイルを書き出しに含める場合は、このオプションをオンに切り替えます。 このオプションは、 **[!UICONTROL Azure Blob ベータ版]** 宛先。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットエクスポートの設定方法について詳しくは、次のチュートリアルを参照してください。

* 方法 [Platform ユーザーインターフェイスを使用したデータセットの書き出し](/help/destinations/ui/export-datasets.md).
* 方法 [フローサービス API を使用したデータセットの書き出し](/help/destinations/api/export-datasets.md).

## 書き出したデータ {#exported-data}

[!DNL Azure Blob Storage] 宛先の場合、[!DNL Platform] は、指定されたストレージの場所に `.csv` ファイルを作成します。ファイルについて詳しくは、セグメントのアクティブ化に関するチュートリアルの[プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)を参照してください。
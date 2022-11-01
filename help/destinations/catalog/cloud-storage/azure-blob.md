---
keywords: Azure Blob;BLOB 宛先；s3;azure BLOB 宛先
title: Azure Blob 接続
description: Azure Blob ストレージへのライブアウトバウンド接続を作成し、CSV データファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 8099849b-e3d2-48a5-902a-ca5a5ec88207
source-git-commit: 56fd7a5ab58186367c729cb4ca8c3b4213c44900
workflow-type: tm+mt
source-wordcount: '890'
ht-degree: 32%

---

# [!DNL Azure Blob] 接続

## 宛先の変更ログ {#changelog}

>[!IMPORTANT]
>
>データセットの書き出し機能のベータ版リリースと、ファイルの書き出し機能の改善により、次の 2 つが表示されるようになりました [!DNL Azure Blob] カードを含める必要があります。
>* 既ににファイルを書き出している場合は、 **[!UICONTROL Azure Blob]** 宛先：新しいデータフローを新しいに作成してください **[!UICONTROL Azure Blob ベータ版]** 宛先。
>* まだ **[!UICONTROL Azure Blob]** 宛先、新しい **[!UICONTROL Azure Blob ベータ版]** 書き出し先のカード **[!UICONTROL Azure Blob]**.


![2 つの Azure Blob 宛先カードの画像を並べて表示します。](/help/destinations/assets/catalog/cloud-storage/blob/two-azure-blob-destination-cards.png)

新しい [!DNL Azure Blob] 宛先カードの内容は次のとおりです。

* [データセットの書き出しのサポート](/help/destinations/ui/export-datasets.md).
* 追加 [ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling).
* 書き出されたファイルに、 [マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping).
* [書き出された CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md).

## 概要 {#overview}

[!DNL Azure Blob] ( 以下「 [!DNL Blob]) は、Microsoftのクラウド用オブジェクトストレージソリューションです。 このチュートリアルでは、 [!DNL Blob] 宛先を [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] の宛先については、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください。 [宛先へのセグメントのアクティブ化](../../ui/activate-batch-profile-destinations.md).

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## サポートされているファイル形式 {#file-formats}

[!DNL Experience Platform] は、次のファイル形式の書き出し先をサポートしています。 [!DNL Azure Blob]:

* コンマ区切り値 (CSV):書き出しデータファイルのサポートは、現在、コンマ区切り値に制限されています。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]**[アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_blob_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64-encoded] 文字列。 以下のドキュメントリンクで、正しく書式設定されたキーの例を確認してください。"

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL 接続文字列]**:接続文字列は、BLOB ストレージのデータにアクセスするために必要です。 この [!DNL Blob] 接続文字列のパターンが次の値で始まる： `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`.
   * 設定に関する詳細 [!DNL Blob] 接続文字列： [Azure ストレージアカウントの接続文字列の構成](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account) (Microsoftドキュメント ) を参照してください。
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64-encoded] 文字列。 base64 でエンコードされた正しい形式のキーの例を以下のドキュメントリンクに表示してください。 中央部分は短くなっていて簡潔です。

![UI での正しい形式と base64 で暗号化された PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。


* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。
* **[!UICONTROL コンテナ]**:名前を入力 [!DNL Azure Blob Storage] この宛先で使用するコンテナ。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しがサポートされます。 データセットのエクスポートを設定する方法について詳しくは、 [データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md).

## 書き出したデータ {#exported-data}

の場合 [!DNL Azure Blob Storage] 宛先、 [!DNL Platform] を作成 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。
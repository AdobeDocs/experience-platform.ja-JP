---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；プロファイルの有効化；プロファイルの有効化
title: リアルタイム顧客プロファイルへのデータの追加
type: Tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順の概要を説明します。
exl-id: c2df224b-bf3d-4994-aa3a-9e9f4a6a726c
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 59%

---


# [!DNL Real-Time Customer Profile] へのデータの追加

このチュートリアルでは、[!DNL Real-Time Customer Profile] にデータを追加するために必要な手順の概要を説明します。

## [!DNL Real-Time Customer Profile] のスキーマを有効にする

[!DNL Real-Time Customer Profile] で使用するために [!DNL Experience Platform] に取り込むデータは、[!DNL Profile] で有効になっている [!DNL Experience Data Model] （XDM）スキーマに準拠している必要があります。 スキーマをプロファイルで有効にするには、[!DNL XDM Individual Profile] クラスまたは [!DNL XDM ExperienceEvent] クラスを実装する必要があります。

[!DNL Schema Registry] API または [!DNL Schema Editor] ユーザーインターフェイスを使用して、スキーマを [!DNL Real-Time Customer Profile] で使用できるようにすることができます。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

バッチ取り込みを使用して [!DNL Experience Platform] にアップロードされたデータはすべて、個々のデータセットにアップロードされます。 [!DNL Real-Time Customer Profile] でこのデータを使用する前に、問題のデータセットを明確に設定する必要があります。 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

[!DNL Profile] 対応の XDM スキーマに準拠している、ストリームで取り込まれたデータは、[!DNL Real-Time Customer Profile] 内の適切なレコードを自動的に追加または上書きします。 レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

[!DNL Real-Time Customer Profile] Access API を使用すると、データセットに読み込まれるバッチデータを取得できます。目的のエンティティを取得できない場合は、[!DNL Profile] でデータセットが有効になっていない可能性があります。データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

[!DNL Real-Time Customer Profile] API を使用してエンティティにアクセスする方法の詳細な手順については、[&#x200B; エンティティエンドポイントガイド &#x200B;](../api/entities.md) （「[!DNL Profile Access] API」とも呼ばれる）を参照してください。

## プロファイルストアデータの更新

場合によっては、組織のプロファイルストアのデータを更新する必要があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。

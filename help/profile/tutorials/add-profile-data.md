---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、プロファイルの有効化、プロファイルの有効化
title: リアルタイム顧客プロファイルへのデータの追加
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、リアルタイム顧客プロファイルにデータを追加するために必要な手順について説明します。
exl-id: c2df224b-bf3d-4994-aa3a-9e9f4a6a726c
source-git-commit: 9f00bff31f9e7d2da1294d3d1f24cba7870a4614
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 58%

---


# [!DNL Real-time Customer Profile] へのデータの追加

このチュートリアルでは、にデータを追加するために必要な手順について説明します。 [!DNL Real-time Customer Profile].

## スキーマの有効化 [!DNL Real-time Customer Profile]

に取り込まれるデータ [!DNL Experience Platform] 使用者 [!DNL Real-time Customer Profile] は、 [!DNL Experience Data Model] (XDM) スキーマが [!DNL Profile]. スキーマをプロファイルで有効にするには、 [!DNL XDM Individual Profile] または [!DNL XDM ExperienceEvent] クラス。

スキーマを有効にして、 [!DNL Real-time Customer Profile] の使用 [!DNL Schema Registry] API または [!DNL Schema Editor] ユーザーインターフェイス。 作業を開始するには、[API を使用したスキーマの作成](../../xdm/tutorials/create-schema-api.md)または[スキーマエディターの UI を使用したスキーマの作成](../../xdm/tutorials/create-schema-ui.md)に関するチュートリアルに従います。

## バッチインジェストを使用してデータを追加する

にアップロードされたすべてのデータ [!DNL Platform] バッチ取得を使用する場合は、個々のデータセットにアップロードされます。 このデータをで使用できるようにする前に [!DNL Real-time Customer Profile]に値を指定する場合は、該当するデータセットを具体的に設定する必要があります。 手順について詳しくは、[プロファイルと ID サービスのデータセットの設定](dataset-configuration.md)に関するチュートリアルを参照してください。

データセットを設定したら、データの取り込みを開始できます。様々な形式のファイルをアップロードする方法について詳しくは、「[バッチ取得開発者ガイド](../../ingestion/batch-ingestion/api-overview.md)」を参照してください。

## ストリーミングインジェストを使用してデータを追加する

ストリームが取り込んだ、 [!DNL Profile]-enabled XDM スキーマは、次の適切なレコードを自動的に追加または上書きします： [!DNL Real-time Customer Profile]. レコードに複数の ID が指定されている場合、または時系列データが使用されている場合は、それらの ID が ID グラフにマッピングされます。追加の設定は必要ありません。詳しくは、[ストリーミングインジェストの開発者ガイドド](../../ingestion/tutorials/streaming-record-data.md)を参照してください。

## アップロードが正常に完了したことを確認する

新しいデータセットに初めてデータをアップロードする場合、または新しい ETL やデータソースに関するプロセスの一部としてデータをアップロードする場合は、データが正しくアップロードされているかどうかを慎重に確認することをお勧めします。

の使用 [!DNL Real-time Customer Profile] API にアクセスすると、データセットに読み込まれるバッチデータを取得できます。 目的のエンティティを取得できない場合は、データセットが有効になっていない可能性があります。 [!DNL Profile]. データセットが有効になっていることを確認した後、ソースデータの形式と識別子が要件を満たしていることを確認します。

を使用してエンティティにアクセスする方法の詳細 [!DNL Real-time Customer Profile] API（を参照） [エンティティエンドポイントガイド](../api/entities.md)( 別名「[!DNL Profile Access] API」を参照してください。

## プロファイルストアデータの更新

場合によっては、組織のプロファイルストアのデータを更新する必要があります。例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。

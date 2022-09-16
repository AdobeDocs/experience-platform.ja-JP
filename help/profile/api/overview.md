---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、統合プロファイル、統合プロファイル、統合プロファイル、プロファイル、rtcp、プロファイルの有効化、プロファイルの有効化
title: リアルタイム顧客プロファイル API ガイド
description: リアルタイム顧客プロファイル API を使用すると、開発者は、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプル、不要になった、またはエラーで追加されたプロファイルデータの削除など、プロファイルデータを調査および操作できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: ce39b95b-cff7-46cf-a14c-8203017c8826
source-git-commit: 9f00bff31f9e7d2da1294d3d1f24cba7870a4614
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 29%

---

# [!DNL Real-time Customer Profile] API ガイド

[!DNL Real-time Customer Profile] を使用すると、Adobe Experience Platform内の各顧客の全体像を確認できます。 [!DNL Profile] を使用すると、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルの異なる顧客データを統合ビューに統合し、顧客とのやり取りごとに実用的なタイムスタンプ付きの説明を提供できます。

この [!DNL Real-time Customer Profile] API には複数のエンドポイントが含まれています（以下で概要を説明します）。 詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[はじめに](getting-started.md)のガイドを参照してください。

使用可能なすべてのエンドポイントと CRUD の操作を表示するには、 [リアルタイム顧客プロファイル API リファレンス Swagger](https://www.adobe.com/go/profile-apis-jp).

の操作に関するガイド [!DNL Real-time Customer Profile] データ [!DNL Experience Platform] UI については、 [プロファイルユーザーガイド](../ui/user-guide.md).

## （アルファ版）計算済み属性 {#computed-attributes}

>[!IMPORTANT]
>
>計算済み属性機能はアルファ版であり、一部のユーザーが使用できます。ドキュメントと機能は変更される場合があります。

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。

各計算済み属性には、受信データを評価し、結果の値をプロファイル属性に保存する式（「ルール」）が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。これらの計算済み属性値は、その後、プロファイルで表示したり、セグメントの作成に使用したり、様々なアクセスパターンを通じてアクセスしたりできます。

計算済み属性の作成、表示、編集および削除は、 `config/computedAttributes` endpoint. 計算済み属性の使用方法については、 [計算済み属性の概要](../computed-attributes/overview.md). API 操作については、 [計算済み属性 API エンドポイントガイド](../computed-attributes/ca-api.md).

## エッジ投影 {#edge-projections}

Adobe Experience Platform を使用すると、「エッジ」と呼ばれる戦略的に配置されたサーバー上のデータに容易にアクセスして、顧客体験をリアルタイムでパーソナライズできます。この [!DNL Real-time Customer Profile] API は、「投影」と呼ばれるコンポーネントを介してエッジを操作するためのエンドポイントを提供します。 これには、各エッジに投影するデータを決定する投影設定や、投影のルーティング先を定義する投影先が含まれます。エッジ投影の操作について詳しくは、 [投影設定および宛先エンドポイントガイド](edge-projections.md).

## エンティティ（[!DNL Profile] アクセス） {#entities}

Adobe Experience Platformから、 [!DNL Real-time Customer Profile] RESTful API またはユーザーインターフェイスを使用するデータ。 API を使用して、「プロファイル」と呼ばれるエンティティにアクセスする方法を学ぶには、 [エンティティエンドポイントガイド](entities.md). を使用してプロファイルにアクセスするには [!DNL Platform] UI( [プロファイルユーザーガイド](../ui/user-guide.md).

## 書き出しジョブ（[!DNL Profile] 書き出し） {#profile-export}

[!DNL Real-time Customer Profile] データは、アクティベーション用のオーディエンスセグメントの書き出しやレポート用のプロファイル属性の書き出しなど、さらに処理するためにデータセットに書き出すことができます。 オーディエンスセグメントの書き出しジョブは、 [!DNL Adobe Experience Platform Segmentation Service] API( [セグメント書き出しジョブエンドポイントガイド](../../profile/api/export-jobs.md) を参照してください。 プロファイル属性の書き出しジョブを作成および管理する手順については、 [書き出しジョブエンドポイントガイド](export-jobs.md).

## 結合ポリシー {#merge-policies}

複数のソースのデータをに取り込む場合 [!DNL Experience Platform]結合ポリシーは、 [!DNL Platform] は、データの優先順位付け方法と、個々の顧客プロファイルを作成するために組み合わされるデータを決定するためにを使用します。 の使用 [!DNL Real-time Customer Profile] API を使用すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなうことができます。  を使用して結合ポリシーを使用するには、[結合ポリシー API エンドポイントガイド](merge-policies.md)を参照してください。

結合ポリシーと、Platform 内での役割について詳しくは、まず [結合ポリシーの概要](../merge-policies/overview.md).

## プレビューサンプルのステータス（[!DNL Profile] プレビュー） {#profile-preview}

データが Platform に取り込まれると、サンプルジョブが実行され、プロファイル数やその他のリアルタイム顧客プロファイルのデータ関連指標が更新されます。 このサンプルジョブの結果は、 `/previewsamplestatus` エンドポイント（リアルタイム顧客プロファイル API の一部） このエンドポイントは、データセットと ID 名前空間の両方によるプロファイル配分のリストを作成したり、組織のプロファイルストアの構成を可視化できる複数のレポートを生成したりする場合にも使用できます。  を使い始めるには、以下を実行します。 `/profilepreviewstatus` エンドポイント ( [プレビューサンプルステータスエンドポイントガイド](preview-sample-status.md).

## プロファイルシステムジョブ {#profile-system-jobs}

に取り込まれるプロファイル対応データ [!DNL Platform] が [!DNL Data Lake] また [!DNL Real-time Customer Profile] データストア。 場合によっては、 [!DNL Profile] ストアを使用して、不要になったデータや誤って追加されたデータを削除します。 そのためには、API を使用して [!DNL Profile System Job](「[!DNL delete request]」（必要に応じて変更、監視または削除可能） を使用して削除リクエストを操作する方法を学ぶには、以下を実行します。 `/system/jobs` エンドポイント [!DNL Real-time Customer Profile] API( [profile system jobs endpoint guide](profile-system-jobs.md).

## プロファイル属性の更新 {#update-profile}

場合によっては、組織のプロファイルストアのデータを更新する必要があります。例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。

## 次の手順 {#next-steps}

を使用して呼び出しを開始するには、以下を実行します。 [!DNL Real-time Customer Profile] API、 [入門ガイド](getting-started.md) 次に、エンドポイントガイドの 1 つを選択して、特定のの使用方法を学びます [!DNL Profile]関連するエンドポイント。 を操作するには [!DNL Profile] データを [!DNL Experience Platform] UI については、 [リアルタイム顧客プロファイルユーザーガイド](../ui/user-guide.md).

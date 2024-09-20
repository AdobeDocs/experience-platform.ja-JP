---
keywords: Experience Platform；プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；API；統合プロファイル；統合プロファイル；統合；プロファイル；rtcp；プロファイルを有効化；プロファイルを有効化
title: リアルタイム顧客プロファイル API ガイド
description: リアルタイム顧客プロファイル API を使用すると、開発者は、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータのエクスポートまたはサンプル、不要になったプロファイルデータまたはエラーで追加されたプロファイルデータの削除など、プロファイルデータを調査および操作できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
role: Developer
exl-id: ce39b95b-cff7-46cf-a14c-8203017c8826
source-git-commit: cb276c55c010aa7ccc936947ad87bf74239d6e99
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 19%

---

# [!DNL Real-Time Customer Profile] API ガイド

[!DNL Real-Time Customer Profile] を使用すると、Adobe Experience Platform内の個々の顧客の全体像を確認できます。 [!DNL Profile] を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルからの様々な顧客データを統合ビューに統合して、すべての顧客インタラクションのタイムスタンプ付きの実用的なアカウントを提供できます。

[!DNL Real-Time Customer Profile] API には、以下に示すように、複数のエンドポイントが含まれます。 詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[はじめに](getting-started.md)のガイドを参照してください。

使用可能なすべてのエンドポイントと CRUD 操作を表示するには、[ リアルタイム顧客プロファイル API リファレンス swagger](https://www.adobe.com/go/profile-apis-en) を参照してください。

[!DNL Experience Platform] UI で [!DNL Real-Time Customer Profile] データを操作する方法については、[ プロファイルユーザーガイド ](../ui/user-guide.md) を参照してください。

## 計算属性 {#computed-attributes}

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティベーションおよびパーソナライゼーションで使用できます。

各計算属性には、受信データを評価し、結果の値をプロファイル属性に保存する式（「ルール」）が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。これらの計算済み属性値は、プロファイルに表示したり、オーディエンスの作成に使用したり、様々なアクセスパターンを使用してアクセスしたりできます。

`ca/attributes/` エンドポイントを使用して、計算済み属性を作成、表示、編集、および削除できます。 計算済み属性の使用方法については、[ 計算済み属性の概要 ](../computed-attributes/overview.md) を参照してください。 API 操作については、[ 計算属性 API エンドポイントガイド ](../computed-attributes/api.md) を参照してください。

## エンティティ（[!DNL Profile] アクセス） {#entities}

Adobe Experience Platformを通じて、RESTful API またはユーザーインターフェイスを使用し [!DNL Real-Time Customer Profile] データにアクセスできます。 API を使用してエンティティ（より一般的には「プロファイル」と呼ばれる）にアクセスする方法については、[ エンティティエンドポイントガイド ](entities.md) に記載されている手順に従ってください。 [!DNL Platform] UI を使用してプロファイルにアクセスするには、[ プロファイルユーザーガイド ](../ui/user-guide.md) を参照してください。

## 書き出しジョブ（[!DNL Profile] 書き出し） {#profile-export}

[!DNL Real-Time Customer Profile] データをデータセットに書き出して、アクティベーション用のオーディエンスやレポート用のプロファイル属性の書き出しなど、さらに処理を行うことができます。 オーディエンスの書き出しジョブは、[!DNL Adobe Experience Platform Segmentation Service] API の一部です。詳しくは、[ セグメント化書き出しジョブエンドポイントガイド ](../../profile/api/export-jobs.md) を参照してください。 プロファイル属性の書き出しジョブを作成および管理する手順については、[ 書き出しジョブエンドポイントガイド ](export-jobs.md) を参照してください。

## 結合ポリシー {#merge-policies}

複数のソースから得られたデータを [!DNL Experience Platform] で統合する場合、結合ポリシーは、データの優先順位付け方法と、[!DNL Platform] れらのデータを組み合わせて個々の顧客プロファイルを作成するかを決定するために使用されるルールです。 [!DNL Real-Time Customer Profile] API を使用すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなうことができます。 API を使用して結合ポリシーを使用するには、[ 結合ポリシーエンドポイントガイド ](merge-policies.md) を参照してください。

結合ポリシーとその Platform 内での役割について詳しくは、まず [ 結合ポリシーの概要 ](../merge-policies/overview.md) をお読みください。

## プレビューサンプルのステータス（[!DNL Profile] プレビュー） {#profile-preview}

データが Platform に取り込まれると、サンプルジョブが実行されて、プロファイル数やその他のリアルタイム顧客プロファイルデータ関連指標が更新されます。 このサンプルジョブの結果は、リアルタイム顧客プロファイル API の一部である `/previewsamplestatus` エンドポイントを使用して確認できます。 また、このエンドポイントを使用して、データセットと ID 名前空間の両方でプロファイル配布をリスト表示したり、複数のレポートを生成して組織のプロファイルストアの構成を可視化したりできます。  `/profilepreviewstatus` エンドポイントの使用を開始するには、[ サンプルステータスプレビューエンドポイントガイド ](preview-sample-status.md) を参照してください。

## プロファイルシステムジョブ {#profile-system-jobs}

[!DNL Platform] に取り込まれたプロファイル対応データは、[!DNL Real-Time Customer Profile] データストアだけでなく [!DNL Data Lake] にも保存されます。 不要になったデータやエラーで追加されたデータを削除するには、プロファイルストアからデータセットに関連付けられたプロファイルデータを削除する必要が生じる場合があります。 それには、API を使用して、「[!DNL delete request]」とも呼ばれる [!DNL Profile System Job] ールを作成する必要があります。このコンポーネントは、必要に応じて変更、監視または削除できます。 [!DNL Real-Time Customer Profile] API の `/system/jobs` エンドポイントを使用して削除リクエストを操作する方法については、[ プロファイルシステムジョブエンドポイントガイド ](profile-system-jobs.md) に記載されている手順に従ってください。

## プロファイル属性の更新 {#update-profile}

場合によっては、組織のプロファイルストアのデータを更新する必要があります。 例えば、レコードを修正したり、属性値を変更したりする必要がある場合があります。これは、バッチの取り込みを通じて行うことができ、upsert タグで構成されたプロファイル対応のデータセットが必要です。属性更新用にデータセットを構成する方法について詳しくは、[プロファイルとアップサートのデータセットの有効化](../../catalog/datasets/enable-upsert.md)に関するチュートリアルを参照してください。

## 次の手順 {#next-steps}

[!DNL Real-Time Customer Profile] API を使用して呼び出しを開始するには、[ はじめる前に ](getting-started.md) ガイドを読み、エンドポイントガイドの 1 つを選択して、特定の [!DNL Profile] 関連エンドポイントの使用方法を学習します。 [!DNL Experience Platform] UI を使用して [!DNL Profile] データを操作するには、[ リアルタイム顧客プロファイルユーザーガイド ](../ui/user-guide.md) を参照してください。

---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API、統合プロファイル、統合プロファイル、統合プロファイル、プロファイル、rtcp、有効プロファイル、有効プロファイル
title: リアルタイム顧客プロファイルAPIガイド
description: リアルタイム顧客プロファイルAPIを使用すると、開発者は、プロファイルの表示、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプル、不要になった、またはエラーで追加されたプロファイルデータの削除など、プロファイルデータを調べて操作できます。 このガイドに従って、API を使用した主な操作の実行方法を学習します。
exl-id: ce39b95b-cff7-46cf-a14c-8203017c8826
source-git-commit: 4c544170636040b8ab58780022a4c357cfa447de
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 23%

---

# [!DNL Real-time Customer Profile] API ガイド

[!DNL Real-time Customer Profile] を使用すると、Adobe Experience Platform内の各顧客の全体像を確認できます。[!DNL Profile] を使用すると、オンライン、オフライン、CRM、サードパーティなど、複数のチャネルの異なる顧客データを、顧客とのやり取りのたびに実用的なタイムスタンプ付きの説明を提供する統合ビューに統合できます。

[!DNL Real-time Customer Profile] APIには、以下に示すように、複数のエンドポイントが含まれます。 詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプル API 呼び出しの読み取りなどに関する重要な情報については、[はじめに](getting-started.md)のガイドを参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、「 [リアルタイム顧客プロファイルAPIリファレンス」のスウォッチ](https://www.adobe.com/go/profile-apis-en)を参照してください。

[!DNL Experience Platform] UIでの[!DNL Real-time Customer Profile]データの操作に関するガイドについては、『[プロファイルユーザーガイド](../ui/user-guide.md)』を参照してください。

## （アルファ版）計算済み属性 {#computed-attributes}

>[!IMPORTANT]
>
>計算済み属性機能はアルファ版であり、一部のユーザーが使用できます。ドキュメントと機能は変更される場合があります。

計算済み属性は、イベントレベルのデータをプロファイルレベルの属性に集計するために使用される関数です。これらの関数は自動的に計算され、セグメント化、アクティブ化およびパーソナライズ機能で使用できます。

各計算済み属性には、受信データを評価し、結果の値をプロファイル属性に保存する式（「ルール」）が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。これらの計算済み属性値は、プロファイルで表示したり、セグメントの作成に使用したり、様々なアクセスパターンを使用してアクセスしたりできます。

計算済み属性の作成、表示、編集および削除は、`config/computedAttributes`エンドポイントを使用しておこなえます。 計算済み属性の使用方法については、[計算済み属性の概要](../computed-attributes/overview.md)を参照してください。 API操作については、[計算済み属性APIエンドポイントガイド](../computed-attributes/ca-api.md)を参照してください。

## エッジ投影 {#edge-projections}

Adobe Experience Platform を使用すると、「エッジ」と呼ばれる戦略的に配置されたサーバー上のデータに容易にアクセスして、顧客体験をリアルタイムでパーソナライズできます。[!DNL Real-time Customer Profile] APIは、「投影」と呼ばれるコンポーネントを介してエッジを操作するためのエンドポイントを提供します。 これには、各エッジに投影するデータを決定する投影設定や、投影のルーティング先を定義する投影先が含まれます。エッジ投影の操作について詳しくは、『[投影設定と宛先エンドポイントのガイド](edge-projections.md)』を参照してください。

## エンティティ（[!DNL Profile] アクセス） {#entities}

Adobe Experience Platformを通じて、RESTful APIまたはユーザーインターフェイスを使用して[!DNL Real-time Customer Profile]データにアクセスできます。 APIを使用して、「プロファイル」と呼ばれるエンティティにアクセスする方法を学ぶには、『[エンティティエンドポイントガイド](entities.md)』で概要を説明する手順に従います。 [!DNL Platform] UIを使用してプロファイルにアクセスするには、『[プロファイルユーザガイド](../ui/user-guide.md)』を参照してください。

## 書き出しジョブ（[!DNL Profile] 書き出し） {#profile-export}

[!DNL Real-time Customer Profile] データは、アクティベーション用のオーディエンスセグメントの書き出しやレポート用のプロファイル属性など、さらに処理するためにデータセットに書き出すことができます。オーディエンスセグメントの書き出しジョブは[!DNL Adobe Experience Platform Segmentation Service] APIに含まれています。詳しくは、『[セグメント化書き出しジョブエンドポイントガイド](../../profile/api/export-jobs.md)』を参照してください。 プロファイル属性の書き出しジョブの作成と管理の手順については、『[書き出しジョブエンドポイントガイド](export-jobs.md)』を参照してください。

## 結合ポリシー {#merge-policies}

[!DNL Experience Platform]で複数のソースのデータを統合する場合、結合ポリシーは、データの優先順位付け方法と、個々の顧客プロファイルを作成するために組み合わされるデータを決定するために[!DNL Platform]で使用されるルールです。 [!DNL Real-time Customer Profile] APIを使用して、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定を行うことができます。  を使用して結合ポリシーを使用するには、[結合ポリシー API エンドポイントガイド](merge-policies.md)を参照してください。

結合ポリシーとPlatform内での役割について詳しくは、まず[結合ポリシーの概要](../merge-policies/overview.md)をお読みください。

## プレビューサンプルのステータス（[!DNL Profile] プレビュー） {#profile-preview}

プロファイルに対して有効なデータがExperience Platformに取り込まれると、そのデータはプロファイルデータストアに保存されます。 プロファイルストアのレコード数が増減すると、データストア内のプロファイルフラグメントと結合プロファイルの数に関する情報を含むサンプルジョブが実行されます。 プロファイルAPIを使用すると、最新の成功したサンプルをプレビューし、データセット別、ID名前空間別にプロファイル配分をリストできます。 `/profilepreviewstatus`エンドポイントの使用を開始するには、『[プレビューのサンプルステータスエンドポイントガイド](preview-sample-status.md)』を参照してください。

## プロファイルシステムジョブ {#profile-system-jobs}

[!DNL Platform]に取り込まれたプロファイル対応データは、[!DNL Data Lake]および[!DNL Real-time Customer Profile]データストアに保存されます。 不要になったデータやエラーで追加されたデータを削除するために、 [!DNL Profile]ストアからデータセットやバッチを削除する必要が生じる場合があります。 そのためには、APIを使用して[!DNL Profile System Job]（「[!DNL delete request]」とも呼ばれます）を作成する必要があります。このは、必要に応じて変更、監視または削除できます。 [!DNL Real-time Customer Profile] APIの`/system/jobs`エンドポイントを使用して削除リクエストを操作する方法を学ぶには、『[プロファイルシステムジョブエンドポイントガイド](profile-system-jobs.md)』で概要を説明している手順に従います。

## 次の手順 {#next-steps}

[!DNL Real-time Customer Profile] APIを使用して呼び出しを開始するには、『[はじめにガイド](getting-started.md)』を読み、特定の[!DNL Profile]関連エンドポイントの使用方法を学ぶために、いずれかのエンドポイントガイドを選択します。 [!DNL Experience Platform] UIを使用して[!DNL Profile]データを操作するには、『[リアルタイム顧客プロファイルユーザーガイド](../ui/user-guide.md)』を参照してください。

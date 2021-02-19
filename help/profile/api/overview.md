---
keywords: Experience Platform;プロファイル；リアルタイムの顧客プロファイル；トラブルシューティング；API；統合プロファイル；統合プロファイル；統合；プロファイル;rtcp;プロファイルの有効化；プロファイルの有効化
title: リアルタイム顧客プロファイルAPIガイド
topic: guide
description: Real-time Customer Commenter APIを使用すると、表示プロファイル、結合ポリシーの作成と更新、プロファイルデータの書き出しとサンプル、不要になった、または誤って追加されたプロファイルデータの削除など、プロファイルデータを参照および操作できます。 このガイドに従って、APIを使用した主な操作の実行方法を学習します。
translation-type: tm+mt
source-git-commit: e649ab3da077cdd8e98562199b8bdece6108a572
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 21%

---


# [!DNL Real-time Customer Profile] APIガイド

[!DNL Real-time Customer Profile] Adobe Experience Platform内の個々の顧客の全体的な表示を確認できます。[!DNL Profile] オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルから個別の顧客データを統合表示に統合し、各顧客インタラクションに関する実用的なタイムスタンプのあるアカウントを提供できます。

[!DNL Real-time Customer Profile] APIには複数のエンドポイントが含まれ、以下で説明します。 詳しくは、個々のエンドポイントのガイドを参照し、必要なヘッダー、サンプルAPI呼び出しの読み取りなどに関する重要な情報については、[はじめに](getting-started.md)を参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、[リアルタイムプロファイルAPIリファレンススウォッチ](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)を参照してください。

[!DNL Experience Platform] UIの[!DNL Real-time Customer Profile]データを操作する手引きについては、[プロファイルユーザーガイド](../ui/user-guide.md)を参照してください。

## （アルファ）計算済み属性{#computed-attributes}

>[!IMPORTANT]
>
>計算済み属性機能はアルファ版であり、一部のユーザーが使用できます。ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントの値を集計できます。各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式（「ルール」）が含まれます。これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。計算済み属性は、`config/computedAttributes`エンドポイントを使用して作成、表示、編集および削除できます。 このエンドポイントの使用方法については、[計算済み属性エンドポイントガイド](computed-attributes.md)を参照してください。

## エッジ投影 {#edge-projections}

Adobe Experience Platform を使用すると、「エッジ」と呼ばれる戦略的に配置されたサーバー上のデータに容易にアクセスして、顧客体験をリアルタイムでパーソナライズできます。[!DNL Real-time Customer Profile] APIは、「投影法」と呼ばれるコンポーネントを通してエッジを操作するための端点を提供します。 これには、各エッジに投影するデータを決定する投影設定や、投影のルーティング先を定義する投影先が含まれます。エッジ投影法の操作について詳しくは、[投影設定と宛先エンドポイントガイド](edge-projections.md)を参照してください。

## エンティティ（[!DNL Profile]アクセス） {#entities}

Adobe Experience Platform経由で[!DNL Real-time Customer Profile]データにアクセスするには、RESTful APIまたはユーザーインターフェイスを使用します。 一般に「プロファイル」と呼ばれるエンティティにアクセスする方法を学習するには、APIを使用して、[entities endpoint guide](entities.md)に記載されている手順に従います。 [!DNL Platform] UIを使用してプロファイルにアクセスするには、[プロファイルユーザーガイド](../ui/user-guide.md)を参照してください。

## 書き出しジョブ（[!DNL Profile]書き出し） {#profile-export}

[!DNL Real-time Customer Profile] アクティベーション用のオーディエンスセグメントのエクスポートや、レポート用のプロファイル属性のエクスポートなど、データをデータセットにエクスポートして追加の処理を行うことができます。オーディエンスセグメント用の書き出しジョブは[!DNL Adobe Experience Platform Segmentation Service] APIの一部です。詳しくは、[セグメント書き出しジョブエンドポイントガイド](../../profile/api/export-jobs.md)を参照してください。 プロファイル属性の書き出しジョブを作成および管理する手順については、[書き出しジョブエンドポイントガイド](export-jobs.md)を参照してください。

## 結合ポリシー {#merge-policies}

[!DNL Experience Platform]で複数のソースのデータをまとめる場合、結合ポリシーは、[!DNL Platform]がデータの優先順位付け方法と、個々の顧客プロファイルを作成するためにどのデータを組み合わせるかを決定するために使用するルールです。 [!DNL Real-time Customer Profile] APIを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 APIを使用した結合ポリシーの操作について詳しくは、[結合ポリシーエンドポイントガイド](merge-policies.md)を参照してください。

[!DNL Platform] UIを使用して結合ポリシーを操作するガイドについては、[merge policies user guide](../ui/merge-policies.md)を参照してください。

## プレビューサンプルの状態([!DNL Profile]プレビュー) {#profile-preview}

プロファイルが可能なデータがExperience Platformに取り込まれると、プロファイルデータストア内に格納される。 プロファイルストアのレコード数が増減すると、データストア内のプロファイルフラグメントと結合プロファイルの数に関する情報を含むサンプルジョブが実行されます。 プロファイルAPIを使用して、最新の成功したサンプルや、データセット別、ID名前空間別にリストプロファイルの配布をプレビューできます。 `/profilepreviewstatus`エンドポイントの使用を開始するには、[プレビューサンプルステータスエンドポイントガイド](preview-sample-status.md)を参照してください。

## プロファイルシステムジョブ {#profile-system-jobs}

[!DNL Platform]に取り込まれたプロファイル対応データは、[!DNL Data Lake]および[!DNL Real-time Customer Profile]データストアに保存されます。 不要になったデータや誤って追加されたデータを削除するために、[!DNL Profile]ストアからデータセットやバッチを削除する必要が生じる場合があります。 これには、APIを使用して[!DNL Profile System Job]（「[!DNL delete request]」とも呼ばれる）を作成する必要があります。これは、必要に応じて変更、監視または削除できます。 [!DNL Real-time Customer Profile] APIの`/system/jobs`エンドポイントを使用して削除要求を操作する方法を学ぶには、『[プロファイルシステムジョブエンドポイントガイド](profile-system-jobs.md)』に記載されている手順に従ってください。

## 次の手順 {#next-steps}

[!DNL Real-time Customer Profile] APIを使用して呼び出しを開始するには、[はじめに](getting-started.md)を読み、いずれかのエンドポイントガイドを選択して、特定の[!DNL Profile]関連エンドポイントの使用方法を学習します。 [!DNL Experience Platform] UIを使用して[!DNL Profile]データを操作するには、[リアルタイム顧客プロファイルユーザーガイド](../ui/user-guide.md)を参照してください。
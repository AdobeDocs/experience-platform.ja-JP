---
keywords: Experience Platform;home;popular topics;Real-time Customer Profile;Identity Service;
solution: Experience Platform
title: リアルタイム顧客プロファイルのチュートリアル
topic: tutorial
type: Tutorial
description: このドキュメントでは、関連する手順について説明し、各ワークフローを完了するためのチュートリアルへのリンクを示しています。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '1001'
ht-degree: 26%

---


# 設定 [!DNL Real-time Customer Profile] および [!DNL Identity Service]

In order to configure [!DNL Real-time Customer Profile] for your organization, you are required to complete multiple separate workflows. このドキュメントでは、関連する手順について説明し、各ワークフローを完了するためのチュートリアルへのリンクを示しています。

To learn more about [!DNL Real-time Customer Profile], begin by reading the [Profile overview](../profile/home.md).

## リアルタイム顧客プロファイルUIの概要

リアルタイム顧客プロファイルは、オンライン、オフライン、CRM、サードパーティデータなど複数のチャネルからのデータを組み合わせて、個々の顧客の全体像を作成します。

**このガイドは次の目的に役立ちます。**
- [!UICONTROL プロファイル] UIと利用可能な機能について理解します。
- プロファイルデータを表示し、管理します。

詳しくは、 [リアルタイムカスタマープロファイルユーザーガイドを参照してください](../profile/ui/user-guide.md)

## リアルタイム顧客プロファイル API

リアルタイム顧客プロファイルAPIには、複数のエンドポイントが含まれます。 プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティのデータなど、複数のチャネルからのさまざまな顧客データを、すべての顧客とのやり取りの実行可能なタイムスタンプ付きのアカウントを提供する統合ビューに統合できます。使用可能な各エンドポイントとその使用例について詳しくは、 [リアルタイム顧客プロファイルAPIの概要](../profile/api/overview.md) （英語）を参照してください。

**以下のAPI開発ガイドを利用できます。**
- [計算済み属性（アルファ） ](../profile/api/computed-attributes.md) — 計算済み属性の使用例、および計算済み属性の設定、アクセス、更新、削除方法について説明します。
- [エッジ投影](../profile/api/edge-projections.md) — 投影先を作成、表示、更新、削除、リストする方法を学びます。 また、このドキュメントには、投影設定のリストと作成に関する情報が含まれ、セレクターの使用例が記載されています。
- [エンティティ(プロファイルアクセス)](../profile/api/entities.md) - IDまたはIDのリストによってプロファイルデータにアクセスする方法を説明します。 さらに、IDを使用して複数のプロファイルの時系列イベントにアクセスする方法、ID別の単一のプロファイルにアクセスする方法、および複数のスキーマエンティティにアクセスする方法を説明します。
- [ジョブの書き出し(プロファイル書き出し)](../profile/api/export-jobs.md) — 書き出しジョブを作成、表示、監視およびキャンセルする方法について説明します。
- [マージポリシー](../profile/api/merge-policies.md) — マージポリシーのコンポーネントに加え、マージポリシーにアクセス、作成、更新および削除する方法について説明します。
- [プレビューサンプルステータス(プロファイルプレビュー)](../profile/api/preview-sample-status.md) — 最後のサンプルステータス、リストプロファイルの表示方法（データセット別）およびリストプロファイルの分布を名前空間別に確認する方法について説明します。
- [プロファイルシステムジョブ（削除リクエスト）](../profile/api/profile-system-jobs.md) -プロファイルストアで、データセットまたはバッチの削除リクエストを表示、作成および削除する方法について説明します。

リアルタイム顧客プロファイルAPIを使用してCRUD操作を実行するために必要な値の詳細と取得については、はじめに（英語のみ）を参照して [ください](../profile/api/getting-started.md)。

## スキーマの対 [!DNL Profile] 応と [!DNL Identity] サービス

Before data can be ingested into Adobe Experience Platform and used in the creation of [!DNL Real-time Customer Profiles], a schema must be created to provide the structure for the data that will be ingested and that schema must be enabled for use in [!DNL Profile] and Adobe Experience Platform [!DNL Identity Service].

**このガイドは次の目的に役立ちます。**
- 既存のスキーマを参照します。
- スキーマの作成と命名.
- XDMミッ追加クスインを定義します。
- スキーマフィールドをIDフィールドとして設定します。
- スキーマのプロファイルを有効にします。

For step-by-step instructions on creating a schema that is enabled for both [!DNL Profile] and [!DNL Identity Service], please refer to the tutorials for [creating a schema using the Schema Registry API](../xdm/tutorials/create-schema-api.md) or [creating a schema using the Schema Builder UI](../xdm/tutorials/create-schema-ui.md).

## およびのデータセットの設定 [!DNL Profile] [!DNL Identity]

To begin ingesting data into [!DNL Profile], you must have a dataset that has been properly configured for use with [!DNL Real-time Customer Profile] and [!DNL Identity Service].

**このガイドは次の目的に役立ちます。**
- プロファイルが有効なデータセットを作成します。
- 既存のデータセットを設定します。
- データセットへのデータの取り込み.
- データセットがプロファイル対応であることを確認し、IDサービスを使用します。

この機能を使い始めるには、APIチュートリアルに従って、プロファイルとIDのデータセットを [設定し](../profile/tutorials/dataset-configuration.md)ます。

## 結合ポリシーの設定

Adobe Experience Platform では、複数のソースのデータを統合して、個々の顧客の全体像を把握できます。When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view.

**このガイドは次の目的に役立ちます。**
- 新しい結合ポリシーを作成します。
- 既存の結合ポリシーを管理します。
- 組織の既定の結合ポリシーを設定します。
- マージポリシー違反を理解します。

To work with merge policies in the [!DNL Platform] UI, visit the [merge policies user guide](../profile/ui/merge-policies.md). リアルタイム顧客プロファイル API で結合ポリシーを使用するには、[結合ポリシーの開発者ガイド](../profile/api/merge-policies.md)を参照してください。

## エッジプロジェクションの設定

複数のチャネルにわたって調整され、一貫性が維持された、またパーソナライズされたエクスペリエンスを顧客にリアルタイムに提供するには、適切なデータを容易に利用できるようにするとともに、変更が発生した際にデータを継続的に更新する必要があります。Adobe [!DNL Experience Platform] enables this real-time access to data through the use of what are known as edges. エッジとは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバーです。データは投影によってエッジにルーティングされ、データの送信先となるエッジを定義する投影先と、エッジ上で利用可能にする特定の情報を定義する投影設定があります。

**このガイドは次の目的に役立ちます。**
- エッジ投影先のリスト、作成、表示、更新および削除。
- リストし、エッジ投影設定を作成します。
- セレクターについて理解します。

For more information and to begin working with edges, refer to the [!DNL Real-time Customer Profile] API [sub-guide on edge projections](../profile/api/edge-projections.md).

## UIでのプロファイルデータの表示方法をカスタマイズする

Experience Platformのユーザーインターフェイス内で、顧客プロファイルの形でリアルタイム顧客プロファイルデータを表示し、操作できます。 UIに表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、個々の顧客の1つの表示を形成しています。 基本属性、リンクされたID、チャネル環境設定などの詳細が含まれます。 プロファイルに表示されるデフォルトのフィールドは、組織レベルで変更して、希望のプロファイル属性を表示することもできます。

**このガイドは次の目的に役立ちます。**
- カードの順序の変更、サイズ変更、編集および削除を行います。
- 追加属性
- 新追加しいカード。
- デフォルトに戻す

プロファイルデータのカスタマイズの詳細については、 [プロファイルカスタマイズドキュメントを参照してください](../profile/ui/profile-customization.md)

## 次の手順

Once you have configured [!DNL Real-time Customer Profile] for your organization, you can begin adding data to individual customer profiles and creating audience segments based on specific customer attributes. 開始する方法については、以下のチュートリアルを参照してください。

- [リアルタイム顧客プロファイルへのデータの追加](../profile/tutorials/add-profile-data.md)
- [セグメントの作成](../segmentation/tutorials/create-a-segment.md)
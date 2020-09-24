---
keywords: Experience Platform;home;popular topics;Real-time Customer Profile;Identity Service;
solution: Experience Platform
title: リアルタイム顧客プロファイルのチュートリアル
topic: tutorial
type: Tutorial
description: このドキュメントでは、関連する手順について説明し、各ワークフローを完了するためのチュートリアルへのリンクを示しています。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 49%

---


# 設定 [!DNL Real-time Customer Profile] および [!DNL Identity Service]

In order to configure [!DNL Real-time Customer Profile] for your organization, you are required to complete multiple separate workflows. このドキュメントでは、関連する手順について説明し、各ワークフローを完了するためのチュートリアルへのリンクを示しています。To learn more about [!DNL Real-time Customer Profile], begin by reading the [Profile overview](../profile/home.md).

## とのスキーマを有効に [!DNL Profile] する [!DNL Identity]

Before data can be ingested into Adobe Experience Platform and used in the creation of [!DNL Real-time Customer Profiles], a schema must be created to provide the structure for the data that will be ingested and that schema must be enabled for use in [!DNL Profile] and Adobe Experience Platform [!DNL Identity Service]. For step-by-step instructions on creating a schema that is enabled for both [!DNL Profile] and [!DNL Identity Service], please refer to the tutorials for [creating a schema using the Schema Registry API](../xdm/tutorials/create-schema-api.md) or [creating a schema using the Schema Builder UI](../xdm/tutorials/create-schema-ui.md).

## およびのデータセットの設定 [!DNL Profile] [!DNL Identity]

To begin ingesting data into [!DNL Profile], you must have a dataset that has been properly configured for use with [!DNL Real-time Customer Profile] and [!DNL Identity Service]. 開始するには、[プロファイルと ID のデータセットの設定に関するチュートリアル](../profile/tutorials/dataset-configuration.md)に従います。

## 結合ポリシーの設定

Adobe Experience Platform では、複数のソースのデータを統合して、個々の顧客の全体像を把握できます。When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view. RESTful API またはユーザーインターフェイスを介すると、新しい結合ポリシーの作成、既存のポリシーの管理、組織のデフォルトの結合ポリシーの設定をおこなえます。To work with merge policies in the [!DNL Platform] UI, visit the [merge policies user guide](../profile/ui/merge-policies.md). リアルタイム顧客プロファイル API で結合ポリシーを使用するには、[結合ポリシーの開発者ガイド](../profile/api/merge-policies.md)を参照してください。

## エッジプロジェクションの設定

複数のチャネルにわたって調整され、一貫性が維持された、またパーソナライズされたエクスペリエンスを顧客にリアルタイムに提供するには、適切なデータを容易に利用できるようにするとともに、変更が発生した際にデータを継続的に更新する必要があります。Adobe [!DNL Experience Platform] enables this real-time access to data through the use of what are known as edges. エッジは地理的に配置されたサーバーです。データを格納し、アプリケーションからデータに容易にアクセスできるようにすることを目的としています。データはプロジェクション、つまりデータの送信先のエッジを定義するプロジェクション先と、エッジ上で利用可能にする情報を定義するプロジェクション設定に従ってエッジにルーティングされます。For more information and to begin working with edges, refer to the [!DNL Real-time Customer Profile] API [sub-guide on edge projections](../profile/api/edge-projections.md).

## 次の手順

Once you have configured [!DNL Real-time Customer Profile] for your organization, you can begin adding data to individual customer profiles and creating audience segments based on specific customer attributes. 開始する方法については、以下のチュートリアルを参照してください。

* [リアルタイム顧客プロファイルへのデータの追加](../profile/tutorials/add-profile-data.md)
* [セグメントの作成](../segmentation/tutorials/create-a-segment.md)
---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイル API 開発者ガイド
topic: guide
translation-type: tm+mt
source-git-commit: d80d49df9c5ac197bdc7f851bbfff18d9b3019d4
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 26%

---


# [!DNL Real-time Customer Profile] API開発者ガイド

[!DNL Real-time Customer Profile] Adobe Experience Platform内の各顧客の全体的な表示を確認できます。 [!DNL Profile] オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルから個別の顧客データを統合表示に統合し、各顧客インタラクションに関する実用的なタイムスタンプのあるアカウントを提供できます。

この [!DNL Real-time Customer Profile] APIには複数のエンドポイントが含まれ、以下に概要を示します。 詳しくは、個々のエンドポイントガイドを参照してください。また、 [入門ガイドを参照し](getting-started.md) 、必要なヘッダーに関する重要な情報、サンプルAPI呼び出しの読み取りなどを確認してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、 [リアルタイムプロファイルAPIリファレンススウォッチガーを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)。

[!DNL Real-time Customer Profile] UIで [!DNL Experience Platform] データを操作する手引きについては、 [プロファイルユーザーガイドを参照してください](../ui/user-guide.md)。

## （アルファ）計算済み属性{#computed-attributes}

>[!IMPORTANT]
>
>計算済み属性機能はアルファ版であり、一部のユーザーが使用できます。ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントの値を集計できます。各計算済み属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式（「ルール」）が含まれます。これらの計算により、ライフタイム購入値、購入間隔、アプリケーションを開いた数などに関する質問に簡単に答えることができます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。You can create, view, edit, and delete computed attributes using the `config/computedAttributes` endpoint. このエンドポイントの使用方法については、『 [計算済み属性エンドポイントガイド](computed-attributes.md)』を参照してください。

## エッジ投影 {#edge-projections}

Adobe Experience Platform を使用すると、「エッジ」と呼ばれる戦略的に配置されたサーバー上のデータに容易にアクセスして、顧客体験をリアルタイムでパーソナライズできます。The [!DNL Real-time Customer Profile] API provides endpoints for working with edges through components called &quot;projections.&quot; これには、各エッジに投影するデータを決定する投影設定や、投影のルーティング先を定義する投影先が含まれます。エッジ投影法の操作について詳しくは、『 [投影の設定と宛先のエンドポイント』ガイドを参照してください](edge-projections.md)。

## エンティティ([!DNL Profile] アクセス) {#entities}

Through Adobe Experience Platform you can access [!DNL Real-time Customer Profile] data using RESTful APIs or the user interface. To learn how to access entities, more commonly known as &quot;profiles&quot;, using the API, follow the steps outlined in the [entities endpoint guide](entities.md). UIを使用してプロファイルにアクセスするには、『 [!DNL Platform] プロファイルユーザガイド [](../ui/user-guide.md)』を参照してください。

## 書き出しジョブ([!DNL Profile] 書き出し) {#profile-export}

[!DNL Real-time Customer Profile] アクティベーション用のオーディエンスセグメントの書き出しや、レポート用のプロファイル属性の書き出しなど、データをデータセットに書き出して追加の処理を行うことができます。 オーディエンスセグメント用の書き出しジョブは [!DNL Adobe Experience Platform Segmentation Service] APIの一部です。詳しくは、 [セグメント書き出しジョブエンドポイントガイド](../../profile/api/export-jobs.md) を参照してください。 プロファイル属性の書き出しジョブを作成および管理する手順については、『 [書き出しジョブエンドポイントガイド](export-jobs.md)』を参照してください。

## 結合ポリシー {#merge-policies}

When bringing data from multiple sources together in [!DNL Experience Platform], merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create individual customer profiles. Using the [!DNL Real-time Customer Profile] API, you can create new merge policies, manage existing policies, and set a default merge policy for your organization. To learn more about working with merge policies using the API, please visit the [merge policies endpoint guide](merge-policies.md).

For a guide to working with merge policies using the [!DNL Platform] UI, please see the [Merge Policies user guide](../ui/merge-policies.md).

## プロファイルシステムジョブ {#profile-system-jobs}

に取り込ま [!DNL Platform] れたデータは、データストアと共 [!DNL Data Lake] に [!DNL Real-time Customer Profile] データストアに保存されます。 Occasionally it may be necessary to delete a dataset or batch from the [!DNL Profile] store in order to remove data that you no longer require or that was added in error. This requires using the API to create a [!DNL Profile System Job], known as a &quot;[!DNL delete request]&quot;, that can also be, modified, monitored, or deleted if required. To learn how to work with delete requests using the `/system/jobs` endpoint in the [!DNL Real-time Customer Profile] API, follow the steps outlined in the [profile system jobs endpoint guide](profile-system-jobs.md).

## 次の手順 {#next-steps}

APIを使用した呼び出しを開始するには、「はじめに [!DNL Real-time Customer Profile] 」ガイドを読み [、特定の](getting-started.md)[!DNL Profile]関連エンドポイントの使い方を学ぶためのエンドポイントガイドの1つを選択します。 UIを使用して [!DNL Profile] データを操作するには、リアルタイムのお客様向けプロファイルユーザガイドを参照してください [!DNL Experience Platform][](../ui/user-guide.md)。
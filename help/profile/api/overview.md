---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: f910351d49de9c4a18a444b99b7f102f4ce3ed5b
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 0%

---


# [!DNL Real-time Customer Profile] API開発者ガイド

[!DNL Real-time Customer Profile] Adobe Experience Platform内の各顧客の全体的な表示を確認できます。 [!DNL Profile] オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルから個別の顧客データを統合表示に統合し、各顧客インタラクションに関する実用的なタイムスタンプのあるアカウントを提供できます。

この [!DNL Real-time Customer Profile] APIには複数のエンドポイントが含まれ、以下に概要を示します。 詳しくは、個々のエンドポイントのガイドを参照してください。必要なヘッダーに関する重要な情報、サンプルAPI呼び出しの読み方などについては、 [はじめに](getting-started.md) 「はじめに」のガイドを参照してください。

使用可能なすべてのエンドポイントとCRUD操作を表示するには、『 [リアルタイムプロファイルAPIリファレンス』スウォッチガーを参照してください](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)。

## （アルファ）計算済み属性

>[!IMPORTANT]
>
>
>計算済み属性機能はアルファベットで表され、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

計算済み属性を使用すると、他の値、計算、式に基づいてフィールドの値を自動的に計算できます。 計算済み属性は、プロファイルレベルで機能します。つまり、すべてのレコードとイベントに対して集計値を実行できます。 計算済みの各属性には、受信データを評価し、結果の値をプロファイル属性またはイベントに格納する式(「rule」)が含まれます。 これらの計算により、ライフタイム購入値、購入間隔、アプリケーション開封回数などに関する質問に簡単に回答できます。情報が必要になるたびに複雑な計算を手動で実行する必要はありません。 エンドポイントを使用して、計算済み属性を作成、表示、編集および削除でき `config/computedAttributes` ます。 このエンドポイントの使用方法については、『 [計算済み属性エンドポイントガイド](computed-attributes.md)』を参照してください。

## エッジ投影法

Adobe Experience Platformは、「エッジ」と呼ばれる戦略的に配置されたサーバー上でデータに容易にアクセスできるようにすることで、顧客体験をリアルタイムにパーソナライズできます。 この [!DNL Real-time Customer Profile] APIは、「投影法」と呼ばれるコンポーネントを通してエッジを操作するための端点を提供します。 これには、各エッジに投影するデータを決定する投影設定や、投影のルーティング先を定義する投影先が含まれます。 エッジ投影法の操作について詳しくは、『 [投影の設定と宛先のエンドポイント』ガイドを参照してください](edge-projections.md)。

## エンティティ(プロファイルアクセス) {#entities}

Adobe Experience Platformを通じて、RESTful APIまたはユーザーインターフェイスを使用して [!DNL Real-time Customer Profile] データにアクセスできます。 APIを使用して、「プロファイル」と呼ばれるエンティティにアクセスする方法を学ぶには、「エン [ティティエンドポイントガイド](entities.md)」に記載されている手順に従います。 UIを使用してプロファイルにアクセスするには、『 [!DNL Platform] プロファイルユーザガイド [](../ui/user-guide.md)』を参照してください。

## 結合ポリシー

複数のソースのデータをにまとめる場合、マージポリシー [!DNL Experience Platform]は、データの優先順位付け方法と、個々の顧客プロファイルを作成するためにどのデータを組み合わせるかを決定するために [!DNL Platform] 使用されるルールです。 Using the [!DNL Real-time Customer Profile] API, you can create new merge policies, manage existing policies, and set a default merge policy for your organization. APIを使用した結合ポリシーの操作について詳しくは、 [結合ポリシーエンドポイントガイドを参照してください](merge-policies.md)。

UIを使用して結合ポリシーを操作する手引きについては、 [!DNL Platform] Merge Policiesユーザーガイドを参照してください [](../ui/merge-policies.md)。

## プロファイルシステムジョブ

に取り込ま [!DNL Platform] れたデータは、データストアと共 [!DNL Data Lake] に [!DNL Real-time Customer Profile] データストアに保存されます。 不要になったデータや誤って追加されたデータを削除するために、データセットやバッチを [!DNL Profile] ストアから削除する必要が生じる場合があります。 これには、APIを使用して「 [!DNL Profile System Job][!DNL delete request]」と呼ばれる、必要に応じて変更、監視または削除が可能なAPIを作成する必要があります。 APIの `/system/jobs` エンドポイントを使用して削除要求を操作する方法を学ぶには、『 [!DNL Real-time Customer Profile] プロファイルシステムジョブエンドポイントガイド [](profile-system-jobs.md)』に説明されている手順に従います。

## 次の手順

APIを使用した呼び出しを開始するには、「はじめに [!DNL Real-time Customer Profile] 」ガイドを読み [、特定の](getting-started.md)[!DNL Profile]関連エンドポイントの使い方を学ぶためのエンドポイントガイドの1つを選択します。 UIを使用した [!DNL Profile] データの操作について詳しくは、リアルタイム顧客プロファイルユーザーガイドを参照して [!DNL Platform] ください [](../ui/user-guide.md)。
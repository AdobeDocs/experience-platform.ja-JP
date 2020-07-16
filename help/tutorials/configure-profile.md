---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リアルタイムのお客様向けプロファイルチュートリアル
topic: tutorial
translation-type: tm+mt
source-git-commit: 5c5f6c4868e195aef76bacc0a1e5df3857647bde
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 0%

---


# 設定 [!DNL Real-time Customer Profile] および [!DNL Identity Service]

組織に対してを設定す [!DNL Real-time Customer Profile] るには、複数の個別のワークフローを完了する必要があります。 このドキュメントでは、関連する手順の概要と、各ワークフローを完了するためのチュートリアルへのリンクを示します。 詳しくは、まず [!DNL Real-time Customer Profile]プロファイルの概要 [を参照してください](../profile/home.md)。

## とのスキーマを有効に [!DNL Profile] する [!DNL Identity]

データをAdobe Experience Platformに取り込んでの作成に使用する前に、スキーマを作成し、取り込むデータの構造を提供し、そのスキーマをAdobe Experience Platformで使用できるようにしなければなりません [!DNL Real-time Customer Profiles][!DNL Profile][!DNL Identity Service]。 との両方に対して有効になるスキーマを作成する手順については、スキーマレジストリAPIを使用したスキーマ [!DNL Profile] 作成のチュートリアル [!DNL Identity Service]、またはスキーマビルダーUIを使用したスキーマ [作成のチュートリアルを参照してください](../xdm/tutorials/create-schema-api.md)[](../xdm/tutorials/create-schema-ui.md)。

## およびのデータセットの設定 [!DNL Profile] [!DNL Identity]

データのへの取り込みを開始するに [!DNL Profile]は、およびでの使用に適切に設定されたデータセットが必要 [!DNL Real-time Customer Profile] で [!DNL Identity Service]す。 使用を開始するには、「プロファイルとIDのデータセットの [設定](../profile/tutorials/dataset-configuration.md)」チュートリアルに従います。

## 結合ポリシーの設定

Adobe Experience Platformを使用すると、複数のソースからデータを統合し、それを組み合わせて、個々の顧客の完全な表示を確認できます。 When bringing this data together, merge policies are the rules that [!DNL Platform] uses to determine how data will be prioritized and what data will be combined to create that unified view. RESTful APIまたはユーザーインターフェイスを使用して、新しい結合ポリシーを作成し、既存のポリシーを管理し、組織のデフォルトの結合ポリシーを設定できます。 UIで結合ポリシーを使用するには、 [!DNL Platform][結合ポリシーユーザーガイドを参照してください](../profile/ui/merge-policies.md)。 リアルタイム顧客プロファイルAPIを使用してマージポリシーを操作するには、 [マージポリシーの開発ガイドを参照してください](../profile/api/merge-policies.md)。

## エッジ投影を設定

複数のチャネルにわたって顧客に対して、調整、一貫性、パーソナライズされたエクスペリエンスをリアルタイムで提供するためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。 アドビで [!DNL Experience Platform] は、エッジと呼ばれるものを使用して、データにリアルタイムでアクセスできます。 エッジは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバです。 データは投影によってエッジにルーティングされ、投影先はデータの送信先となるエッジを定義し、投影設定はエッジで利用可能にする特定の情報を定義します。 エッジの詳細と使用を開始する方法については、エッジ投影に関する [!DNL Real-time Customer Profile] API [サブガイドを参照してください](../profile/api/edge-projections.md)。

## 次の手順

組織に対して設定 [!DNL Real-time Customer Profile] を行ったら、個々の顧客プロファイルへのデータの追加を開始し、特定の顧客属性に基づいてオーディエンスセグメントを作成できます。 使用を開始するには、次のチュートリアルを参照してください。

* [リアルタ追加イム顧客プロファイルに対するデータ](../profile/tutorials/add-profile-data.md)
* [セグメントの作成](../segmentation/tutorials/create-a-segment.md)
---
keywords: Experience Platform;developer guide;Data Science Workspace;popular topics;Real time machine learning;
solution: Experience Platform
title: リアルタイム機械学習の概要
topic: Overview
translation-type: tm+mt
source-git-commit: ab8b000bec0ae30c695582f57c40105b7ca1f22f
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 2%

---


# リアルタイム機械学習の概要

>[!IMPORTANT]
>リアルタイム機械学習は、まだすべてのユーザーが利用できるわけではありません。 この機能はアルファベットで、まだテスト中です。 このドキュメントは変更される可能性があります。

Adobe Experience PlatformのReal-time Machine Learningフレームワークを使用すると、機械学習を利用して、適切なチャネル内で適切なエンドユーザーに2秒未満の適切なエクスペリエンスを適切なタイミングで提供できます。

## メリット

リアルタイム機械学習は、エンドユーザーに対するデジタルエクスペリエンスコンテンツの関連性を大幅に向上させます。 これは、Experience Edgeでリアルタイムの会議と継続的な学習を活用することで可能になります。

ハブとエッジの両方でシームレスな計算を組み合わせることで、従来、関連性が高く応答性の高い、パーソナライズされたエクスペリエンスのパワーに関与してきた待ち時間が大幅に削減されます。 したがって、リアルタイム機械学習では、同期的な意思決定に対する遅延が非常に低いという推測が提供されます。 例としては、パーソナライズされたWebページコンテンツのレンダリングや、オファーの提示、Webストアでの変動の抑制とコンバージョンの増加を目的とした割引などがあります。

## リアルタイム機械学習アーキテクチャ

次の図は、リアルタイム機械学習アーキテクチャの概要を示しています。

![簡単になった概要](../images/rtml/simple-overview.png)

## リアルタイム機械学習ワークフロー（アルファ）

次のワークフローは、リアルタイム機械学習モデルの作成と利用に関する一般的な手順と結果の概要を示しています。

### データの取り込みと準備

データは、Adobe Experience PlatformのExperience Data Model(XDM)で取り込まれ、変換されます。 このデータは、モデルトレーニングに使用されます。 XDMの詳細については、 [XDMの概要を参照してください](../../xdm/home.md)。

### オーサリング

リアルタイム機械学習モデルを作成するには、ゼロから作成するか、Adobe Experience Platform Jupyter Notebooksで事前にトレーニングを受けたONNXモデルとして提供します。

### デプロイ

モデルをExperience Edgeに展開し、予測APIエンドポイントを使用してサービスギャラリーでリアルタイム機械学習サービスを作成します。

### 推論

予測REST APIエンドポイントを使用して、機械学習インサイトをリアルタイムで生成します。

### 配信

その後、リアルタイムの機械学習スコアをアドビのターゲットを使用するエクスペリエンスにマップするセグメントとルールを定義できます。 これにより、ブランドのWebサイトの訪問者は、同じページまたは次のページに、リアルタイム（100ミリ秒未満）で過度にパーソナライズされたエクスペリエンスを表示できます。

## 開発計画

リアルタイム機械学習は現在、アルファ段階です。 次の表に、今後のベータ版のリリースでリリースされる予定の機能とアップデートの一部を示します。

<table>
    <th></th>
    <th>アルファ（5月）</th>
    <th>ベータ</th>
    <tr>
        <td>
            <strong>機能</strong>
        </td>
        <td>
            <li>Data Science Workspaceは、ノートブックランチャーの統合を通じて、独自のモデルと作成者を提供します。</li>
            <li>オーサリング演算子のスターターセット。</li>
            <li>ハブに配置</li>
            <li>Scikit学習ベースのモデルを参照してください。</li>
        </td>
        <td>
            <li>Data Science Workspace Service Gallery UIの統合。</li>
            <li>推論結果により、リアルタイムの顧客プロファイルを自動的に強化します。</li>
            <li>詳細な学習モデルを参照してください。</li>
            <li>カスタム演算子など、一連のオーサリング演算子が拡張されました。</li>
        </td>
    </tr>
    <tr>
        <td>
            <strong>使用可否</strong>
        </td>
        <td>
            北米
        </td>
        <td>
            <li>北米</li>
            <li>ヨーロッパおよび中東（ヨーロッパ、中東、ヨーロッパ、中東、アフリカ）</li>
            <li>アジアパシフィック(APAC)</li>
        </td>
    </tr>
    <tr>
        <td>
            <strong>オーサリング</strong>
        </td>
        <td>
            <li>Pythonサポート</li>
            <li>リアルタイム機械学習SDK</li>
            <li>Pythonオーサリングノード： Pandas、ScikitLearn、ONXNode、Split、ModelUpload、OneHotEncoder。</li>
        </td>
        <td>
            <li>tensorflowのサポート。</li>
            <li>その他のPythonオーサリングノード： リアルタイムの顧客プロファイルリーダ、リアルタイムの顧客プロファイルライタ、数のアレイ、XDM2Frame、Frame2XDM。 </li>
        </td>
    </tr>
    <tr>
        <td>
            <strong>スコアリング実行時間</strong>
        </td>
        <td>
            ONNX
        </td>
        <td>
            ONNX
        </td>
    </tr>
</table>

## 次の手順

はじめに、はじめに [](./getting-started.md) . このガイドでは、リアルタイム機械学習モデルを作成するために必要な前提条件をすべて設定する手順を説明します。


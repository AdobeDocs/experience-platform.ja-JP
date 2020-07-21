---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: マルチエンティティセグメント
topic: overview
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---


# マルチエンティティセグメント

複数エンティティのセグメント化は、プロファイル、店舗またはその他の非製品クラスに基づいて、追加のデータを使用して [!DNL Profile] データを拡張する機能です。 接続すると、追加のクラスのデータは、 [!DNL Profile] スキーマにネイティブであるかのように使用できるようになります。

マルチエンティティのセグメント化の詳細については、以下のビデオを見て、ドキュメントを読み、学習を補完してください。また、 [セグメント化の概要を調べてください](./home.md)。]

>[!VIDEO](https://video.tv.adobe.com/v/28947?quality=12&learn=on)

## はじめに

このチュートリアルでは、セグメント化の使用に関連する様々なAdobe Experience Platformサービスについて、十分な理解が必要です。 このチュートリアルを開始する前に、次のサービスのドキュメントを確認してください。

- [!DNL Real-time Customer Profile](../profile/home.md): 複数のソースからの集計データに基づいて、リアルタイムで統一された消費者プロファイルを提供します。
- [Adobe Experience Platformセグメントサービス](./home.md): リアルタイム顧客プロファイルからセグメントを作成できます。
- [!DNL Experience Data Model (XDM)](../xdm/home.md): 顧客体験データを [!DNL Platform] 整理するための標準化されたフレームワーク。

## XDMの関係を定義する方法

(XDM)スキーマの構造との関係の定義は、セグメント作成の重要で不可欠な部分です。 [!DNL Experience Data Model]

このプロセスは、 [!DNL Schema Registry][!DNL Schema Editor]APIまたは APIを使用して2つのスキーマ間の関係を定義する詳細なガイドについては、APIを使用して2つのスキーマ間 [の関係を定義するチュートリアルをお読みください](../xdm/tutorials/relationship-api.md)。 を使用して2つのスキーマ間の関係を定義する方法の詳細なガイドにつ [!DNL Schema Editor] いては、スキーマエディタ [を使用して2つのスキーマ間の関係を定義する方法のチュートリアルを参照してください](../xdm/tutorials/relationship-ui.md)。

## XDM関係を使用するセグメントの作成方法

XDMの関係を定義したら、 [!DNL Segmentation Service] APIを使用してセグメントを作成できます。

このプロセスは、 [!DNL Segmentation] APIまたは [!DNL Segment Builder] ユーザーインターフェイスを使用して実行できます。 APIを使用してセグメントを作成する方法の詳細なガイドについては、セグメント化API [を使用したセグメントの作成に関するチュートリアルをお読みください](./tutorials/create-a-segment.md)。 セグメントビルダーを使用したセグメントの作成に関する詳細なガイドについては、『セグメントビルダーユーザーガイド [』](./ui/overview.md)を参照してください。

## マルチエンティティセグメントのセグメントの評価およびアクセス方法

セグメントを作成した後、APIを使用してセグメントの結果を評価し、アクセスでき [!DNL Segmentation Service] ます。 マルチエンティティセグメントの評価は、通常のセグメントの評価と非常に似ています。

このプロセスは、 [!DNL Segmentation Service] APIを使用してのみ実行できます。 APIを使用してセグメントを評価し、セグメントにアクセスする詳細なガイドについては、セグメントの [評価とアクセスに関するチュートリアルをお読みください](./tutorials/evaluate-a-segment.md)。
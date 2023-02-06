---
title: 宛先での設定可能で一般的な書き出し設定
description: 宛先レベルで設定可能で、修正済みで、編集できない、宛先の書き出し設定について説明します。
source-git-commit: 04322d10a88b27d5641ab9474c8387945aab9982
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 3%

---


# 宛先での設定可能で一般的な書き出し設定

Experience Platformの宛先への書き出し動作について考える際は、設定が機能する 3 つの異なるレベルを考慮する必要があります。

* 第 1 レベルでは、プロファイルの書き出し動作と設定に関連する一部の設定は、宛先タイプに属するすべての宛先で共通です。 これらの設定は、宛先の書き出しのトリガーと、書き出しに含まれるものを指し、宛先の開発者またはリアルタイム CDP ユーザーは編集できません。
* 第 2 レベルでは、宛先を使用して宛先をオーサリングする際に、宛先開発者が宛先レベルで一部の設定をカスタマイズできます。Destination SDK
* 第 3 レベルでは、リアルタイム CDP ユーザーがアクティベーションワークフローで設定できる設定があります。

![宛先の共通書き出し設定と設定可能な書き出し設定の間のインタープレイを示す図](/help/destinations/assets/how-destinations-work/profile-export-behavior-diagram.png)

このページでは、上記の 3 つのレベルで、宛先のすべての共通および設定可能な書き出し設定について説明またはリンクします。

## 宛先タイプ間の共通の書き出し設定 {#common-settings-across-destination-types}

宛先の書き出し動作は、 *宛先の書き出しのトリガー* および *宛先の書き出しに含まれる内容*. 宛先の書き出しは、宛先サービスが [アップストリームリアルタイム顧客プロファイルサービス](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/platform-applications.html?lang=en#adobe-experience-platform-%26-applications-detailed-architecture-diagram).

宛先の書き出しに含まれる内容は、宛先のタイプによって若干異なります。 詳しくは、 [宛先タイプごとの一般的なエクスポート動作パターン](/help/destinations/how-destinations-work/profile-export-behavior.md). これらの設定は、宛先開発者やリアルタイム CDP ユーザーは編集できません。

## 宛先開発者によるカスタマイズ可能な書き出し設定 {#customizable-settings-by-destination-developers}

宛先開発者は、 [Destination SDK](/help/destinations/destination-sdk/overview.md) カスタムの宛先や、製品化された（非公開または公開）宛先を作成する場合。 Destination SDKは、開発者に、API エンドポイントやファイル受信システムのダウンストリーム機能に基づいて宛先を柔軟に設定できるようにします。 宛先開発者は、ダウンストリーム機能に基づいて、Destination SDKを使用して宛先を設定する際に、次の設定オプションを使用できます。

* 宛先にExperience Platformから書き出すことができる属性と ID を決定します。 また、データの書き出しを正常におこなうために宛先で必要な ID も決定します。
* API 統合に送信される HTTP メッセージを集約する際に、Experience Platformが待機する時間を決定する集約ポリシーを設定します。 宛先の開発者は、様々な集計タイプを設定して、送信 HTTP メッセージに含めるプロファイルの数と、HTTP メッセージのディスパッチまでのExperience Platformの待ち時間を決定できます。 詳細な情報を見つける [集約ポリシーの設定オプション](/help/destinations/destination-sdk/destination-configuration.md#aggregation) 宛先の開発者が使用できるDestination SDKドキュメント。
* HTTP メッセージの書き出しに、セグメントに適合するプロファイル、セグメントから削除されるプロファイル、またはその両方を含めるかどうかを指定します。
* ファイルの書き出し時に、ユーザーが使用できるファイル名およびファイル形式設定を決定します。

## ユーザーがカスタマイズ可能なデータフローレベルの設定 {#settings-on-dataflow-level}

宛先のタイプと宛先の開発者が設定した設定に応じて、編集できない設定の他に、ユーザーがアクティベーションワークフローで設定できる書き出し設定があります。 これらの設定は、特定のデータフローの宛先への書き出しスケジュール、データフローで書き出す必要のある属性と ID フィールド、または書き出したファイルのファイル形式オプションに関連します。

ユーザーが宛先に接続する際に使用できる設定は、宛先開発者が宛先を設定した方法と、ユーザーがどの設定を使用できるようにしたかによって異なります。

例： [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations)の場合、宛先の開発者は宛先が受け入れる ID を設定し、それらの ID のみを [アクティベーションワークフローのマッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)、以下に示すように。

![アクティベーションワークフローのマッピング手順での、ターゲットフィールドの ID 選択の画面記録。 ](/help/destinations/assets/how-destinations-work/identity-mapping-example.gif)

同様に、 [ファイルベースの宛先](/help/destinations/destination-types.md#file-based)を使用する場合、宛先の開発者は、 [ファイル名追加オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names) 顧客が目的地に使用できるようにしたい場合、またはどちらを使用するか [ファイル形式オプション](/help/destinations/destination-sdk/guides/batch/configure-file-formatting-options.md) を使用可能にしたい場合、次に示すように、ユーザーはこれらのオプションからのみ選択できます。

![ファイルベースの宛先に接続する際の、ファイルフォーマットオプションの画面記録。](/help/destinations/assets/how-destinations-work/file-formatting-options.gif)

![アクティベーションワークフローのスケジューリング手順の「ファイル名を追加」オプションの画面記録。 ](/help/destinations/assets/how-destinations-work/filename-append-options.gif)

アクティベーションワークフローで使用できる様々なオプションと手順について詳しくは、以下を参照してください。

* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-batch-profile-destinations.md)
* [エンタープライズの宛先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-streaming-profile-destinations.md)
* [ストリーミングセグメント書き出し宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-segment-streaming-destinations.md)
* [オンデマンドでのファイルのバッチ保存先への書き出し](/help/destinations/ui/export-file-now.md)
* [（ベータ版）クラウドストレージ宛先へのデータセットの書き出し](/help/destinations/ui/export-datasets.md)

## 次の手順 {#next-steps}

このドキュメントを読むと、開発者が個々の宛先レベルで設定できる、アクティベーションワークフローでユーザーが編集できる、宛先タイプ間で共通の宛先の書き出し設定がわかります。

宛先の開発者は、次の操作を実行できます。 [使い始める](/help/destinations/destination-sdk/getting-started.md) Destination SDK データのアクティブ化を検討しているユーザーは、 [カタログ](/help/destinations/catalog/overview.md).
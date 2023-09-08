---
title: 宛先での設定可能で一般的な書き出し設定
description: 宛先レベルで設定可能な宛先の書き出し設定と、固定されていて編集できない書き出し設定について説明します。
exl-id: 3f4706cb-6d51-4567-81f6-5b2bf167b576
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: ht
source-wordcount: '845'
ht-degree: 100%

---

# 宛先での設定可能で一般的な書き出し設定

Experience Platform の宛先への書き出し動作について考える際は、設定が機能する 3 つの異なるレベルを考慮する必要があります。

* 第 1 レベルでは、プロファイルの書き出しの動作と設定の指定に関連する設定の一部は、宛先タイプに属するすべての宛先で共通です。これらの設定では、宛先の書き出しをトリガーする内容と、書き出しに含まれ、宛先開発者または Real-Time CDP ユーザーが編集できない内容を参照します。
* 第 2 レベルでは、Destination SDK を使用して宛先を作成する際に、宛先開発者が宛先レベルで一部の設定をカスタマイズできます。
* 第 3 レベルでは、Real-Time CDP ユーザーがアクティベーションワークフローで設定できる設定の指定があります。

![宛先の一般的な書き出し設定と設定可能な書き出し設定の間の相互作用を示す図](/help/destinations/assets/how-destinations-work/profile-export-behavior-diagram.png)

このページでは、上記の 3 つのレベルで、宛先のすべての一般的かつ設定可能な書き出し設定について説明またはリンクしています。

## 宛先タイプ間の一般的な書き出し設定 {#common-settings-across-destination-types}

宛先の書き出し動作は、*宛先の書き出しをトリガーする内容*&#x200B;と、*宛先の書き出しに含まれる内容*&#x200B;に関して、宛先タイプに属する宛先全体で一貫しています。宛先の書き出しは、宛先サービスが[アップストリームのリアルタイム顧客プロファイルサービス](https://experienceleague.adobe.com/docs/blueprints-learn/architecture/architecture-overview/platform-applications.html?lang=ja#adobe-experience-platform-%26-applications-detailed-architecture-diagram)から受信する通知によってトリガーされます。

宛先の書き出しに含まれる内容は、宛先タイプによって微妙に異なります。詳しくは、[宛先タイプごとの一般的な書き出し動作パターン](/help/destinations/how-destinations-work/profile-export-behavior.md)を参照してください。これらの設定は、宛先開発者または Real-Time CDP ユーザーが編集することはできません。

## 宛先開発者によるカスタマイズ可能な書き出し設定 {#customizable-settings-by-destination-developers}

宛先開発者は、[Destination SDK](/help/destinations/destination-sdk/overview.md) を使用して、カスタムまたは製品化された（プライベートまたはパブリック）宛先を作成できます。Destination SDK は、API エンドポイントとファイル受信システムのダウンストリーム機能に基づいて宛先を設定するための優れた柔軟性を開発者に提供します。ダウンストリーム機能に基づいて、宛先開発者は、Destination SDK を使用して宛先を設定する際に、次の設定オプションを使用できます。

* Experience Platform から宛先に書き出すことができる属性と ID を決定します。また、データの書き出しを正常に行うために、宛先で必要な ID も決定します。
* API 統合に送信される HTTP メッセージを集計する際に、Experience Platform が待機する時間を決定する集計ポリシーを設定します。宛先開発者は、様々な集計タイプを設定して、送信 HTTP メッセージに含めるプロファイルの数と、HTTP メッセージをディスパッチするまで Experience Platform が待機する時間を決定できます。 Destination SDK ドキュメントで、宛先開発者が使用できる[集計ポリシー設定オプション](../destination-sdk/functionality/destination-configuration/aggregation-policy.md)に関する詳細な情報を参照してください。
* HTTP メッセージの書き出しに、セグメントに適合するプロファイル、セグメントから削除されたプロファイル、またはその両方を含める必要があるかどうかを決定します。
* ファイルを書き出す際に、ユーザーが使用できるファイル名とファイル形式の設定を決定します。

## ユーザーがカスタマイズ可能なデータフローレベルの設定 {#settings-on-dataflow-level}

宛先タイプと宛先開発者が設定した指定に依存する編集不可能な設定に加えて、ユーザーがアクティベーションワークフローで設定できる特定の書き出し設定があります。これらの設定は、宛先への特定のデータフローの書き出しスケジュール、データフローで書き出す必要がある属性と ID フィールド、書き出されたファイルのファイル形式オプションに関連しています。

宛先に接続する際にユーザーが使用できる設定は、宛先開発者が宛先を設定した方法と、ユーザーが使用できるようにした設定によって異なります。

例えば、[ストリーミング宛先](/help/destinations/destination-types.md#streaming-destinations)の場合、宛先開発者は、宛先が受け入れる ID を設定できます。以下に示すように、[アクティベーションワークフローのマッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)でこれらの ID のみがユーザーに表示されます。

![アクティベーションワークフローのマッピング手順での、ターゲットフィールドの ID 選択の画面記録。](/help/destinations/assets/how-destinations-work/identity-mapping-example.gif)

同様に、[ファイルベースの宛先](/help/destinations/destination-types.md#file-based)の場合、宛先開発者は、宛先で使用可能にする[ファイル名追加オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)や、使用可能にする[ファイル形式オプション](/help/destinations/destination-sdk/guides/batch/configure-file-formatting-options.md)を決定でき、以下に示すように、ユーザーはこれらのオプションからのみ選択できます。

![ファイルベースの宛先に接続する際の、ファイル形式オプションの画面記録。](/help/destinations/assets/how-destinations-work/file-formatting-options.gif)

![アクティベーションワークフローのスケジューリング手順のファイル名を追加オプションの画面記録。](/help/destinations/assets/how-destinations-work/filename-append-options.gif)

アクティベーションワークフローで使用できる様々なオプションと手順について詳しくは、以下を参照してください。

* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-batch-profile-destinations.md)
* [エンタープライズ宛先に対するオーディエンスデータの有効化 ](/help/destinations/ui/activate-streaming-profile-destinations.md)
* [ストリーミングオーディエンスの書き出し先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-segment-streaming-destinations.md)
* [オンデマンドでのファイルのバッチ宛先への書き出し](/help/destinations/ui/export-file-now.md)
* [（ベータ版）クラウドストレージ宛先へのデータセットの書き出し](/help/destinations/ui/export-datasets.md)

## 次の手順 {#next-steps}

このドキュメントを参照すると、宛先のどの書き出し設定が宛先タイプ間で共通であるか、開発者が個々の宛先レベルで設定できるか、アクティベーションワークフローでユーザーがどの設定を編集できるかがわかります。

次に、[宛先タイプごとの一般的な書き出し動作パターン](/help/destinations/how-destinations-work/profile-export-behavior.md)に関する詳細情報を参照することができます。

宛先開発者は、Destination SDK の[基本を学ぶ](/help/destinations/destination-sdk/getting-started.md)ことができます。データのアクティブ化を検討しているユーザーは、[カタログ](/help/destinations/catalog/overview.md)で使用可能なすべての宛先を確認できます。

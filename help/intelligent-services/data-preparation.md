---
keywords: Experience Platform;home;intelligent services;popular topics
solution: Experience Platform
title: Intelligent Servicesで使用するデータの準備
topic: Intelligent Services
translation-type: tm+mt
source-git-commit: 03135f564bd72fb60e41b02557cb9ca9ec11e6e8

---


# Intelligent Servicesで使用するデータの準備

インテリジェントサービスがマーケティングイベントデータからの洞察を見つけるには、そのデータがセマンティックに強化され、標準構造で維持されている必要があります。 Intelligent Servicesは、これを達成するためにExperience Data Model(XDM)スキーマを活用します。 特に、Intelligent Servicesで使用されるすべてのデータセットは、 **Consumer ExperienceEvent(CEE)** XDMスキーマに準拠している必要があります。

このドキュメントでは、複数のチャネルのマーケティングイベントのデータをこのスキーマにマッピングし、スキーマ内の重要なフィールドの情報をまとめ、データを効率的に構造にマッピングする方法を判断する際の一般的な手引きを示します。

## CEEについてのスキーマ

Consumer ExperienceEventスキーマは、デジタルマーケティングイベント（Webまたはモバイル）、オンラインまたはオフラインのコマースアクティビティに関する個人の行動を説明します。 このスキーマの使用は、意味的に適切に定義されたフィールド（列）があるので、Intelligent Servicesでは必須です。そうしないと、データの明確性が低下する不明な名前は避けられます。

すべてのXDMスキーマと同様、CEEミックスインも拡張可能です。 つまり、CEEミックスインにフィールドを追加し、必要に応じて複数のスキーマに異なるバリエーションを含めることができます。

mixinの完全な例は、 [public XDM repositoryにあります](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-consumer.schema.md)。以下の節で概要を説明するキーフィールドの参照として使用してください。

### キーフィールド

次の表は、CEEミックスイン内の主要なフィールドを示しています。このフィールドを利用して、インテリジェントサービスで有用なインサイトを生成する必要があります。例えば、その他の例に関する説明や参照ドキュメントへのリンクが含まれます。

| XDM フィールド | 説明 | リファレンス |
| --- | --- | --- |
| `xdm:channel` | ExperienceEventに関連するマーケティングチャネル。 このフィールドには、チャネルの種類、メディアの種類、場所の種類に関する情報が含まれます。 **アトリビュー&#x200B;_ション_AIがデータを処理するには、このフィールドを指定する必要があります**。 マッピングの例 [については](#example-channels) 、次の表を参照してください。 | [エクスペリエンスチャネルスキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/channels/channel.schema.md) |
| `xdm:productListItems` | 製品のSKU、名前、価格、数量など、顧客が選択した製品を表す品目の配列。 | [コマースの詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) |
| `xdm:commerce` | 発注書番号や支払い情報など、ExperienceEventに関するコマース固有の情報が含まれます。 | [コマースの詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-commerce.schema.md) |
| `xdm:web` | インタラクション、ページの詳細、転送者など、ExperienceEventに関連するWebの詳細を表します。 | [ExperienceEvent Web詳細スキーマ](https://github.com/adobe/xdm/blob/797cf4930d5a80799a095256302675b1362c9a15/docs/reference/context/experienceevent-web.schema.md) |

### サンプルチャネル {#example-channels}

このフィー `xdm:channel` ルドは、ExperienceEventに関連するマーケティングチャネルを表します。 次の表に、XDMにマッピングされたマーケティングチャネルの例を示します。

| Channel | `channel.mediaType` | `channel._type` | `channel.mediaAction` |
| --- | --- | --- | --- |
| 有料検索 | 有料 | 検索 | クリック |
| Social - Marketing | 獲得 | SOCIAL | クリック |
| 表示 | 有料 | 表示 | クリック |
| 電子メール | 有料 | EMAIL | クリック |
| 内部転送者 | 所有 | 直接 | クリック |
| ビュースルーを表示 | 有料 | 表示 | IMPRESSION |
| QRコードのリダイレクト | 所有 | 直接 | クリック |
| SMSテキストメッセージ | 所有 | SMS | クリック |
| Mobile | 所有 | モバイル | クリック |

## データのマッピングと取り込み

マーケティングイベントのデータをCEEスキーマにマッピングできるかどうかを判断したら、データをインテリジェントサービスに取り込むプロセスを開始できます。 データをアドビのコンサルティングサービスにマッピングし、スキーマに取り込む方法については、アドビのコンサルティングサービスにお問い合わせください。

Adobe Experience Platform購読をお持ちで、データを自分でマッピングして取り込む場合は、次の節で説明する手順に従います。

### Adobe Experience Platformの使用

>[!NOTE] 次の手順では、エクスペリエンスプラットフォームへの購読が必要です。 プラットフォームにアクセスできない場合は、次の手順に進ん [でください](#next-steps) 。

この節では、詳細な手順のチュートリアルへのリンクを含め、Intelligent Servicesで使用するデータをExperience Platformにマッピングして取り込むためのワークフローについて説明します。

#### CEEデータセットとスキーマの作成

取り込むデータを準備する開始の準備が整ったら、まずCEEミックスインを使用する新しいXDMスキーマを作成します。 次のチュートリアルでは、UIまたはAPIで新しいスキーマを作成するプロセスを説明します。

* [UIでのスキーマの作成](../xdm/tutorials/create-schema-ui.md)
* [APIでのスキーマの作成](../xdm/tutorials/create-schema-api.md)

>[!IMPORTANT] 上記のチュートリアルは、ワークフローを作成するための一般的なスキーマです。 スキーマのクラスを選択する場合は、 **XDM ExperienceEventクラスを使用する必要があります**。 このクラスを選択したら、CEEミックスインをスキーマに追加。

CEEミックスインをスキーマに追加した後、データ内の追加のフィールドに必要に応じて他のミックスインを追加できます。

データセットを作成して保存した後、スキーマに基づいて新しいデータセットをスキーマします。 次のチュートリアルでは、UIまたはAPIで新しいデータセットを作成するプロセスについて説明します。

* [UIでのデータセットの作成](../catalog/datasets/user-guide.md#create) （既存のデータセットの使用ワークフローに従います）
* [APIでのデータセットの作成](../catalog/datasets/create.md)

#### データのマッピングと取り込み

CEEスキーマとデータセットを作成したら、データテーブルをスキーマに開始マッピングし、そのデータをプラットフォームに取り込むことができます。 UIでCSVファイルを実行す [る手順については、CSVファイルをXDMスキーマにマッピングする](../ingestion/tutorials/map-a-csv-file.md) （英語のみ）のチュートリアルを参照してください。 データセットを設定したら、同じデータセットを使用して追加のデータファイルを取り込むことができます。

## 次の手順 {#next-steps}

このドキュメントでは、インテリジェントサービスで使用するデータの準備に関する一般的なガイダンスを提供しました。 使用事例に基づいて追加のコンサルティングが必要な場合は、アドビのコンサルティングサポートにお問い合わせください。

データセットに顧客体験データを正しく埋め込むと、インテリジェントサービスを使用してインサイトを生成できます。 使用を開始するには、次のドキュメントを参照してください。

* [アトリビューションAIの概要](./attribution-ai/overview.md)
* [顧客 AI の概要](./customer-ai/overview.md)

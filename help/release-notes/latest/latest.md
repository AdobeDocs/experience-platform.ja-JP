---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: 8f2c9bf8-1487-46e4-993b-bd9b63774cab
source-git-commit: 2c4b0d6dd0884fe81565356c31b18c0555bf973f
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 71%

---

# Adobe Experience Platform リリースノート

**リリース日：2021年11月17日（PT）**

## 新機能

Adobe Experience Platform の新機能：

- [Real-time Customer Data Platform B2B Edition](#B2B)
- [（ベータ版）アドホックアクティベーション API を使用して、バッチ保存先に対するオーディエンスセグメントをアクティブ化します](#ad-hoc-activation)

## 既存の機能に対するアップデート

Adobe Experience Platform の既存の機能に対するアップデート：

- [アトリビューション AI](#attribution-ai)
- [顧客 AI](#customer-ai)

### Real-time Customer Data Platform B2B エディション {#B2B}

**リリース日：2021年11月12日（PT）**

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

Real-time CDP B2B Edition と対応する B2C を区別する様々な Adobe Experience Platform 機能が改善されています。これには、B2B の使用例におけるエクスペリエンスデータモデル（XDM）の改善、ID の解決およびプロファイルのセグメンテーションのアップグレード、Marketo Engage のカスタムコネクタと宛先が含まれます。Marketoコネクタを使用すると、B2B ブランドは業界をリードする B2B エンゲージメントデータと行動情報を結び付け、リードを育成し、アカウントベースのマーケティング操作を強化できます。

-[B2B と B2P の新しいエディション](#editions)
-[新しい Marketo データソースと宛先コネクタ](#marketo)
-[標準 B2B XDM](#XDM)

### B2B と B2P の新しいエディション {#editions}

B2B のデータと機能を Real-time CDP と Platform アクティベーションの両製品に提供する B2B と B2P の新しいエディションを購入いただけます。

Real-time CDP B2B Edition の詳細については、 [概要](../../rtcdp/overview.md).

### 新しい Marketo データソースおよび宛先コネクタ {#marketo}

新しい Marketo データソースおよび宛先コネクタを利用すれば、Marketo データを Platform に送ったり、Platform オーディエンスを Marketo に戻したりすることができます。 すべての Platform ユーザーが利用できます。

| 機能 | 説明 |
|----------|-------------|
| Marketo Engageソースコネクタ | この [Marketo Engageソースコネクタ](../../sources/connectors/adobe-applications/marketo/marketo.md) マーケターは、1 つ以上のMarketoインスタンスからAdobe Experience Platformインスタンスにデータをシームレスに取り込むことができ、リード管理と B2B マーケター向けの完全なソリューションを提供します。 |
| Marketo Engage の宛先 | [Marketo の宛先](../../destinations/catalog/adobe/marketo-engage.md)を利用することで、マーケターは、Adobe Experience Platform で作成したセグメントを Marketo にプッシュできます。それらのセグメントは静的リストとして表示されます。 |

### 標準 B2B XDM {#XDM}

標準 B2B XDM のクラス、フィールドグループおよびデータタイプは、すべての Platform ユーザーが使用できます。

| 機能 | 説明 |
|-----------|--------------|
| 標準 B2B XDM クラス | Real-time Customer Data Platform B2B Edition は、アカウント、機会、キャンペーンなどといった、B2B の基本的なデータエンティティに関する詳細をキャプチャする複数の標準 XDM を提供します。。 |

詳しくは、 [Real-time Customer Data Platform B2B Edition のスキーマ](../../rtcdp/schemas/b2b.md) B2B データエンティティのキャプチャに関する詳細は、ドキュメントを参照してください。

### （ベータ版）アドホックアクティベーション API を使用して、バッチ保存先に対するオーディエンスセグメントをアクティブ化します {#ad-hoc-activation}

アドホックアクティベーション API を使用すると、マーケターは、即時アクティベーションが必要な状況に対し、すばやく効率的に、宛先に対するオーディエンスセグメントをプログラムでアクティブ化できます。 アドホックオーディエンスのアクティベーションは、 [バッチファイルベースの宛先](../../destinations/destination-types.md#file-based) 現在はベータ版です。 詳しくは、 [アドホックアクティベーション API ドキュメント](../../destinations/api/ad-hoc-activation-api.md).

### アトリビューション AI {#attribution-ai}

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
|-----------|---------------|
| 複数のデータセットのサポート | アトリビューション AI では、各データセットをマッピングして結び付けなくても、容易に複数のデータセットを直接 UI に取り込めるようになりました。 この新しい時間節約機能により、複数のデータセットからの豊富なデータにより、より強力で正確なスコアが得られます。 |
| メディアチャネルとキャンペーンフィールドのマッピング | アトリビューション AI は、メディアチャネルとキャンペーンフィールドのマッピングをサポートするようになりました。 データセット間のメディアチャネルマッピングにより、アトリビューション AI から導かれるインサイトが向上し、解釈しやすい明確な結果が得られるようになります。 |

アトリビューション AI について詳しくは、[アトリビューション AI のドキュメント](../../intelligent-services/attribution-ai/overview.md)を参照してください。

### 顧客 AI {#customer-ai}

Real-time Customer Data Platformで使用できる顧客 AI は、個々のプロファイルのカスタム傾向スコア（チャーンやコンバージョンなど）を大規模に生成するために使用されます。 ビジネスニーズから機械学習の問題への変換、アルゴリズムの選択、トレーニング、デプロイは必要ありません。

**更新された機能**

| 機能 | 説明 |
|-----------|-------------|
| 複数のデータセットのサポート | 顧客 AI は、各データセットをマッピングして結び付けなくても、容易に複数のデータセットを直接 UI に取り込めるようになりました。 この新しい時間節約機能により、複数のデータセットからの豊富なデータにより、より強力で正確なスコアが得られます。 |
| カスタムプロファイル属性 | 顧客 AI では、標準のイベントフィールドに加えて、カスタムプロファイルデータセットフィールド（タイムスタンプ付き）をデータに定義できるようになりました。 このオプションを使用すると、モデルの品質向上やより正確な結果の提供につながる可能性のあるプロファイル属性を追加できます。。 |

顧客 AI について詳しくは、[顧客 AI のドキュメント](../../intelligent-services/customer-ai/overview.md)を参照してください。


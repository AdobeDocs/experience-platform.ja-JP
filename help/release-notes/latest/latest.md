---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: 8f2c9bf8-1487-46e4-993b-bd9b63774cab
source-git-commit: 2c4b0d6dd0884fe81565356c31b18c0555bf973f
workflow-type: ht
source-wordcount: '798'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021年11月17日（PT）**

## 新機能

Adobe Experience Platform の新機能：

- [Real-time Customer Data Platform B2B エディション](#B2B)
- [（ベータ版）アドホックアクティベーション API を介して、バッチ配信先に対するオーディエンスセグメントをアクティブ化します](#ad-hoc-activation)

## 既存の機能に対するアップデート

Adobe Experience Platform の既存の機能に対するアップデート：

- [アトリビューション AI](#attribution-ai)
- [顧客 AI](#customer-ai)

### Real-time Customer Data Platform B2B エディション {#B2B}

**リリース日：2021年11月12日（PT）**

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

Real-time CDP B2B Edition と対応する B2C を区別する様々な Adobe Experience Platform 機能が改善されています。これには、B2B の使用例におけるエクスペリエンスデータモデル（XDM）の改善、ID の解決およびプロファイルのセグメンテーションのアップグレード、Marketo Engage のカスタムコネクタと宛先が含まれます。Marketo コネクタを使用すると、B2B ブランドは、業界をリードする B2B エンゲージメントデータを行動情報と結び付けて、リードを育成し、アカウントベースのマーケティングオペレーションを強化できます。

-[B2B と B2P の新しいエディション](#editions)
-[新しい Marketo データソースと宛先コネクタ](#marketo)
-[標準 B2B XDM](#XDM)

### B2B と B2P の新しいエディション {#editions}

Real-time CDP とプラットフォームアクティベーションの両製品に B2B のデータと機能を追加した新しい B2B エディションおよび B2P エディションを購入できます。

Real-time CDP B2B エディションについて詳しくは、[概要](../../rtcdp/overview.md)を参照してください。

### 新しい Marketo データソースおよび宛先コネクタ {#marketo}

新しい Marketo データソースおよび宛先コネクタは、Marketo データを Platform にストリーミングし、Platform オーディエンスを Marketo に戻します。すべての Platform ユーザーが利用できます。

| 機能 | 説明 |
|----------|-------------|
| Marketo Engage ソースコネクタ | [Marketo Engage ソースコネクタ](../../sources/connectors/adobe-applications/marketo/marketo.md)を使用すれば、マーケターは 1 つ以上の Marketo インスタンスから Adobe Experience Platform インスタンスにデータをシームレスに取り込み、リード管理および B2B マーケターに完全なソリューションを提供できます。 |
| Marketo Engage の宛先 | [Marketo の宛先](../../destinations/catalog/adobe/marketo-engage.md)を利用することで、マーケターは、Adobe Experience Platform で作成したセグメントを Marketo にプッシュできます。それらのセグメントは静的リストとして表示されます。 |

### 標準 B2B XDM {#XDM}

標準 B2B XDM のクラス、フィールドグループおよびデータタイプは、すべての Platform ユーザーが使用できます。

| 機能 | 説明 |
|-----------|--------------|
| 標準 B2B XDM クラス | Real-time Customer Data Platform B2B Edition は、アカウント、機会、キャンペーンなどといった、B2B の基本的なデータエンティティに関する詳細をキャプチャする複数の標準 XDM を提供します。。 |

B2B データエンティティのキャプチャについて詳しくは、[Real-time Customer Data Platform B2B エディションのスキーマ](../../rtcdp/schemas/b2b.md)を参照してください。

### （ベータ版）アドホックアクティベーション API を介して、バッチ配信先に対するオーディエンスセグメントをアクティブ化します {#ad-hoc-activation}

アドホックアクティベーション API を使用すると、マーケターは、即時にアクティベーションが必要な状況で、宛先へのオーディエンスセグメントをプログラムによってすばやく効率的にアクティブ化できます。アドホックオーディエンスのアクティベーションは、[バッチファイルベースの宛先](../../destinations/destination-types.md#file-based)でのみサポートされています（現在はベータ版）。詳しくは、[アドホックアクティベーション API のドキュメント](../../destinations/api/ad-hoc-activation-api.md)を参照してください。

### アトリビューション AI {#attribution-ai}

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
|-----------|---------------|
| 複数のデータセットのサポート | アトリビューション AI では、各データセットをマッピングして結び付けなくても、容易に複数のデータセットを直接 UI に取り込めるようになりました。 この時間を節約できる新機能により、複数のデータセットから豊富なデータを取得して、より強力で正確なスコアを得ることができます。 |
| メディアチャネルとキャンペーンフィールドのマッピング | アトリビューション AI は、メディアチャネルとキャンペーンフィールドのマッピングをサポートするようになりました。 データセット間のメディアチャネルマッピングにより、アトリビューション AI から導かれるインサイトが向上し、解釈しやすい明確な結果が得られるようになります。 |

アトリビューション AI について詳しくは、[アトリビューション AI のドキュメント](../../intelligent-services/attribution-ai/overview.md)を参照してください。

### 顧客 AI {#customer-ai}

Real-time Customer Data Platform で使用できる顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。これを実現するために、ビジネスニーズを機械学習の問題に変換したり、アルゴリズムを選択したり、トレーニング、またはデプロイする必要はありません。

**更新された機能**

| 機能 | 説明 |
|-----------|-------------|
| 複数のデータセットのサポート | 顧客 AI は、各データセットをマッピングして結び付けなくても、容易に複数のデータセットを直接 UI に取り込めるようになりました。 この時間を節約できる新機能により、複数のデータセットから豊富なデータを取得して、より強力で正確なスコアを得ることができます。 |
| カスタムプロファイル属性 | 顧客 AI では、標準のイベントフィールドに加えて、カスタムプロファイルデータセットフィールド（タイムスタンプ付き）をデータに定義できるようになりました。 このオプションを使用すると、モデルの品質向上やより正確な結果の提供につながる可能性のあるプロファイル属性を追加できます。。 |

顧客 AI について詳しくは、[顧客 AI のドキュメント](../../intelligent-services/customer-ai/overview.md)を参照してください。


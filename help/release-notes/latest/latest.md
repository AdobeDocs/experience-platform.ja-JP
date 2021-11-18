---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: aa8cafc9a40748eda3098b2af732a828d39204b2
workflow-type: tm+mt
source-wordcount: '724'
ht-degree: 28%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 11 月 17 日**

## 新機能

Adobe Experience Platform の新機能：

- [Real-time Customer Data Platform B2B Edition](#B2B)

## 既存の機能の更新

Adobe Experience Platform の既存の機能のアップデート：

- [Attribution AI](#attribution-ai)
- [顧客 AI](#customer-ai)

### Real-time Customer Data Platform B2B エディション {#B2B}

**リリース日：2021 年 11 月 12 日**

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

Real-time CDP B2B Edition と対応する B2C を区別する様々な Adobe Experience Platform 機能が改善されています。B2B の使用例に対するエクスペリエンスデータモデル (XDM) の改善、ID 解決とプロファイルセグメント化のアップグレード、Marketo Engage用のカスタムビルドのコネクタと宛先が含まれます。 Marketoコネクタを使用すると、B2B ブランドは業界をリードする B2B エンゲージメントデータと行動情報を結び付け、リードを育成し、アカウントベースのマーケティング操作を強化できます。

-[新しい B2B エディションと B2P エディション](#editions)
-[新しいMarketoデータソースおよび宛先コネクタ](#marketo)
-[標準 B2B XDM](#XDM)

### 新しい B2B エディションと B2P エディション {#editions}

B2B のデータと機能をリアルタイム CDP とプラットフォームアクティベーション製品の両方に提供する新しい B2B および B2P エディションを購入できます。

Real-time CDP B2B Edition の詳細については、 [概要](../../rtcdp/overview.md).

### 新しいMarketoデータソースおよび宛先コネクタ {#marketo}

新しいMarketoデータソースおよび宛先コネクタは、Marketoデータを Platform および Platform オーディエンスにストリーミングしてMarketoに戻します。 すべての Platform ユーザーが利用できます。

| 機能 | 説明 |
|----------|-------------|
| Marketo Engageソースコネクタ | この [Marketo Engageソースコネクタ](../../sources/connectors/adobe-applications/marketo/marketo.md) マーケターは、1 つ以上のMarketoインスタンスからAdobe Experience Platformインスタンスにデータをシームレスに取り込むことができ、リード管理と B2B マーケター向けの完全なソリューションを提供します。 |
| Marketo Engage先 | この [Marketoの宛先](../../destinations/catalog/adobe/marketo-engage.md) Adobe Experience Platformで作成したセグメントをMarketoにプッシュし、静的リストとして表示できます。 |

### 標準 B2B XDM {#XDM}

標準の B2B XDM クラス、フィールドグループ、およびデータタイプは、すべての Platform ユーザーが使用できます。

| 機能 | 説明 |
|-----------|--------------|
| 標準 B2B XDM クラス | Real-time Customer Data Platform B2B Edition は、アカウント、商談、キャンペーンなど、B2B の重要なデータエンティティに関する詳細をキャプチャする、いくつかの標準 XDM を提供します。 |

詳しくは、 [Real-time Customer Data Platform B2B Edition のスキーマ](../../rtcdp/schemas/b2b.md) B2B データエンティティのキャプチャに関する詳細は、ドキュメントを参照してください。

### Attribution AI {#attribution-ai}

Attribution AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

| 機能 | 説明 |
|-----------|---------------|
| 複数のデータセットのサポート | Attribution AIは、各データセットをマッピングして結び付けなくても、複数のデータセットを UI で直接簡単に取り込めるようになりました。 この新しい時間節約機能により、複数のデータセットからの豊富なデータにより、より強力で正確なスコアが得られます。 |
| メディアチャネルとキャンペーンフィールドのマッピング | Attribution AIは、メディアチャネルとキャンペーンフィールドのマッピングをサポートするようになりました。 Attribution AIセット間のメディアチャネルマッピングにより、データから得られるインサイトが向上し、解釈しやすいより明確な結果が得られます。 |

Attribution AIの詳細については、 [Attribution AIドキュメント](../../intelligent-services/attribution-ai/overview.md).

### 顧客 AI {#customer-ai}

Real-time Customer Data Platformで使用できる顧客 AI は、個々のプロファイルのカスタム傾向スコア（チャーンやコンバージョンなど）を大規模に生成するために使用されます。 ビジネスニーズから機械学習の問題への変換、アルゴリズムの選択、トレーニング、デプロイは必要ありません。

**更新された機能**

| 機能 | 説明 |
|-----------|-------------|
| 複数のデータセットのサポート | 顧客 AI で、各データセットをマッピングして結び付けなくても、複数のデータセットを UI から直接簡単に取り込めるようになりました。 この新しい時間節約機能により、複数のデータセットからの豊富なデータにより、より強力で正確なスコアが得られます。 |
| カスタムプロファイル属性 | 顧客 AI で、標準のイベントフィールドに加えて、データでカスタムプロファイルデータセットフィールド（タイムスタンプ付き）を定義できるようになりました。 このオプションを使用すると、影響力があると見なすプロファイル属性を追加して、モデルの品質を向上させ、より正確な結果を提供できます。 |

顧客 AI について詳しくは、 [顧客 AI ドキュメント](../../intelligent-services/customer-ai/overview.md).

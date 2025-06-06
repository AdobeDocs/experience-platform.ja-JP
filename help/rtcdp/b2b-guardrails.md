---
keywords: プロファイル；リアルタイム顧客プロファイル；トラブルシューティング；ガードレール；ガイドライン；制限；エンティティ；プライマリエンティティ；ディメンションエンティティ；RTCDP;CDP;B2B edition;Real-Time Customer Data Platform;real time customer data platform;real time cdp;b2b;cdp;
title: Real-Time Customer Data Platform B2B editionのデフォルトガードレール
type: Documentation
description: Adobe Experience Platform は、従来のリレーショナルデータモデルとは異なる、高度に非正規化されたハイブリッドデータモデルを使用します。 このドキュメントでは、Adobe Real-Time Customer Data Platform B2B editionを使用して最適なシステムパフォーマンスを得るためにデータをモデル化する際に役立つ、デフォルトの使用方法とレートの上限について説明します。
badgeB2B: label="B2B edition" type="Informative" url="https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
feature: Guardrails, B2B
exl-id: 8eff8c3f-a250-4aec-92a1-719ce4281272
source-git-commit: bc399f3af0524232671af780ea1380f1a71a5b7e
workflow-type: tm+mt
source-wordcount: '1817'
ht-degree: 46%

---

# Real-Time Customer Data Platform B2B editionのデフォルトガードレール

>[!NOTE]
>
>このドキュメントで概要を説明する上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

Real-Time Customer Data Platform B2B editionを使用すると、リアルタイム顧客プロファイルおよびアカウントプロファイルの形式で、行動インサイトと顧客属性に基づいてパーソナライズされたクロスチャネルエクスペリエンスを提供できます。 プロファイルに対するこの新しいアプローチをサポートするために、Experience Platform では、従来のリレーショナルデータモデルとは異なる、高度に非正規化されたハイブリッドデータモデルを使用します。

>[!IMPORTANT]
>
>このガードレール ページに加えて、販売注文と対応する [ 製品説明 ](https://helpx.adobe.com/jp/legal/product-descriptions.html) でライセンスの使用権限を確認してください。

このドキュメントでは、最適なシステムパフォーマンスを得るためにデータをモデルリングする際に役立つ、デフォルトの使用方法とレートの上限について説明します。次のガードレールを確認する際は、データが正しくモデル化されていることが前提になっています。データのモデル化方法に関するご質問は、カスタマーサービス担当者にお問い合わせください。

>[!INFO]
>
>ほとんどのお客様は、これらのデフォルトの上限を超えることはありません。カスタムの上限について詳しくは、カスタマーケア担当者にお問い合わせください。

## 上限のタイプ

このドキュメントでは、次の 2 種類のデフォルトの上限について説明します。

| ガードレール タイプ | 説明 |
| -------------- | ----------- |
| **パフォーマンスガードレール（ソフトリミット）** | パフォーマンスガードレールは、ユースケースのスコーピングに関連する使用制限です。 パフォーマンスガードレールを超えると、パフォーマンスの低下や待ち時間が発生する場合があります。 Adobeは、このようなパフォーマンス低下に対する責任を負いません。 パフォーマンスのガードレールを常に超えるお客様は、パフォーマンスの低下を避けるために、追加容量のライセンスを選択できます。 |
| **システムで適用されるガードレール（ハードリミット）** | システムに適用されたガードレールは、Real-Time CDP UI または API によって適用されます。 これらは、UI と API によってこれをブロックされるか、エラーを返すので、超えることはできない制限です。 |

>[!INFO]
>
>このドキュメントで概要を説明する上限は、常に改善されています。 定期的にアップデートを確認してください。カスタムの上限について知りたい場合は、カスタマーケア担当者にお問い合わせください。

## データモデルの上限

次のガードレールは、リアルタイム顧客プロファイルデータをモデル化する際の推奨の上限を提供します。 プライマリエンティティとディメンションエンティティについて詳しくは、付録の[エンティティタイプ](#entity-types) に関する節を参照してください。

### プライマリエンティティのガードレール

>[!NOTE]
>
>この節で概要を説明するデータモデルの上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| Real-Time CDP B2B edition標準 XDM クラスデータセット | 60 | パフォーマンスガードレール | Real-Time CDP B2B editionが提供する標準 Experience Data Model （XDM）クラスを利用するデータセットは、最大 60 個を推奨します。 B2B ユースケースの標準 XDM クラスの完全なリストについては、[Real-Time CDP B2B editionのスキーマ ドキュメント ](schemas/b2b.md) を参照してください。 <br/><br/>*メモ：Experience Platform の非正規化ハイブリッドデータモデルの性質上、ほとんどのお客様がこの上限を超えることはありません。データのモデル化方法やカスタムの上限について詳しくは、カスタマーケア担当者にお問い合わせください。* |
| ID グラフ内の個々のアカウントの ID 数 | 50 | パフォーマンスガードレール | 個人アカウント用 ID グラフの最大 ID 数は 50 です。 ID が 50 を超えるプロファイルは、セグメント化、書き出し、検索から除外されます。 |
| 従来のマルチエンティティ関係 | 20 | パフォーマンスガードレール | プライマリエンティティとディメンションエンティティの間に定義されるマルチエンティティの関係は、最大 20 個を推奨します。既存の関係が削除されるか無効になるまで、追加の関係マッピングは行わないでください。 |
| XDM クラスあたりの多対 1 の関係 | 2 | パフォーマンスガードレール | XDM クラスあたりる最大 2 つの多対 1 の関係を定義することをお勧めします。 既存の関係が削除されるか無効になるまで、追加の関係マッピングは行わないでください。2 つのスキーマ間の関係を作成する手順については、[B2B スキーマの関係の定義](../xdm/tutorials/relationship-b2b.md)に関するチュートリアルを参照してください。 |

### ディメンションエンティティのガードレール

>[!NOTE]
>
>この節で概要を説明するデータモデルの上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| ネストされたレガシー関係がありません | 0 | パフォーマンスガードレール | 2 つの非 [!DNL XDM Individual Profile] スキーマ間に関係を作成しないでください。[!DNL Profile] 結合スキーマの一部ではないスキーマには、関係の作成は **推奨されません**。 |
| 多対 1 の関係に追加できるのはB2B オブジェクトのみ | 0 | システムに適用されたガードレール | システムは、B2B オブジェクト間の多対 1 の関係のみをサポートします。 多対 1 の関係について詳しくは、[B2B スキーマの関係の定義](../xdm/tutorials/relationship-b2b.md)に関するチュートリアルを参照してください。 |
| B2B オブジェクト間のネストされた関係の最大深度 | 3 | システムに適用されたガードレール | B2B オブジェクト間でネストされた関係の最大深度は 3 です。つまり、高度にネストされたスキーマでは、B2B オブジェクト間の関係を 3 レベルをより深くネストしないでください。 |
| 各ディメンションエンティティに対して 1 つのスキーマ | 1 | システムに適用されたガードレール | 各ディメンションエンティティには、1 つのスキーマが必要です。 複数のスキーマから作成されたディメンションエンティティを使用しようとすると、セグメント化の結果に影響を与える可能性があります。 異なるディメンションエンティティには、別々のスキーマが想定されます。 |

## データサイズの上限

次のガードレールは、データサイズを参照し、意図したとおりに取り込み、保存し、クエリできるデータの推奨される上限を提供します。プライマリエンティティとディメンションエンティティについて詳しくは、付録の[エンティティタイプ](#entity-types)に関する節を参照してください。

>[!INFO]
>
>データサイズは、取得時に JSON で非圧縮データとして測定されます。

### プライマリエンティティのガードレール

>[!NOTE]
>
>この節で概要を説明するデータサイズの上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| 1 日に取り込まれるバッチ（XDM クラスあたり） | 45 | パフォーマンスガードレール | 1 日に取り込まれるバッチの合計数は、XDM クラスあたり 45 を超えないようにしてください。追加のバッチを取り込むと、最適なパフォーマンスが妨げられる場合があります。 |

### ディメンションエンティティのガードレール

>[!NOTE]
>
>この節で概要を説明するデータサイズの上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| すべてのディメンションエンティティの合計サイズ | 5 GB | パフォーマンスガードレール | すべてのディメンションエンティティの推奨合計サイズは 5 GB です。大きなディメンションエンティティの取り込みは、システムのパフォーマンスに影響を与える可能性があります。例えば、10 GB の製品カタログをディメンションエンティティとして読み込むことはお勧めしません。 |
| ディメンショナルエンティティスキーマごとのデータセット | 5 | パフォーマンスガードレール | 各ディメンショナルエンティティスキーマに関連付けるデータセットの数は、最大で 5 個にすることをお勧めします。 例えば、「products」のスキーマを作成し、関連する 5 個のデータセットを追加する場合、products スキーマに関連付けられる 6 個目のデータセットを作成しないでください。 |
| 1 日に取り込まれるディメンションエンティティバッチ | エンティティごとに 4 個 | パフォーマンスガードレール | 1 日に取り込まれるディメンションエンティティバッチの推奨最大数は、エンティティあたり 4 個です。 例えば、製品カタログのアップデートを 1 日に最大 4 回取り込むことができます。同じエンティティに対して追加のディメンションエンティティバッチを取り込むと、システムのパフォーマンスに影響を与える可能性があります。 |

## セグメンテーションガードレール

このセクションで概説するガードレールとは、Experience Platform内に作成できるオーディエンスの数と特性のほか、オーディエンスを宛先にマッピングしてアクティブ化する方法のことです。

>[!NOTE]
>
>このセクションで概説するセグメント化の上限とは、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

| ガードレール | 上限 | 上限のタイプ | 説明 |
| --------- | ----- | ---------- | ----------- |
| B2B サンドボックスごとのセグメント定義 | 400 | パフォーマンスガードレール | 組織は合計 400 個を超えるセグメント定義を持つことができます（各 B2B サンドボックスのセグメント定義の数が 400 個未満である必要があります）。 追加のセグメント定義を作成しようとすると、システムのパフォーマンスに影響を与える可能性があります。 |

## 次の手順

このドキュメントで概要を説明する上限は、Real-Time Customer Data Platform B2B editionで有効になった変更内容を表しています。 Real-Time CDP B2B editionのデフォルトの上限の詳細なリストについては、これらの上限と、[ リアルタイム顧客プロファイルデータドキュメントのガードレール ](../profile/guardrails.md) に概説されているAdobe Experience Platformの一般的な上限を組み合わせてください。

## 付録

このセクションでは、このドキュメントに記載の上限に関する追加の詳細を示します。

### エンティティタイプ

[!DNL Profile] ストアデータモデルは、2 つのコアエンティティタイプ（[ プライマリエンティティ ](#primary-entity) と [ ディメンションエンティティ ](#dimension-entity) で構成されます。

#### プライマリエンティティ

プライマリエンティティ、別称プロファイルエンティティは、データを結合して個人の「単一の信用できるソース」としたものです。 この統合データは、「結合ビュー」と呼ばれるものを使用して表されます。統合ビューは、同じクラスを実装するすべてのスキーマのフィールドを、1 つの結合スキーマに集約します。[!DNL Real-Time Customer Profile] の結合スキーマは、すべてのプロファイル属性と行動イベントのコンテナとして機能する、非正規化されたハイブリッドデータモデルです。

時間に依存しない属性（「レコードデータ」とも呼ばれる）は、[!DNL XDM Individual Profile]、時系列データ（「イベントデータ」とも呼ばれる）は [!DNL XDM ExperienceEvent] を使用してモデル化されます。レコードと時系列データが Adobe Experience Platform に取り込まれると、[!DNL Real-Time Customer Profile] がトリガーされ、使用可能なデータの取り込みが開始されます。 取り込まれるインタラクションや詳細が多いほど、個人プロファイルは正確になります。

![ レコードデータと時系列データの違いの概要を示したインフォグラフィック。](../profile/images/guardrails/profile-entity.png)

#### Dimension エンティティ

プロファイルデータを保持しているプロファイルデータストアはリレーショナルストアではありませんが、シンプルで直感的にオーディエンスを作成できるようにするために、プロファイルでは小さなディメンションエンティティとの統合が可能になっています。 この統合は、[ マルチエンティティセグメント化 ](../segmentation/tutorials/multi-entity-segmentation.md) とも呼ばれます。

組織では、店舗、製品、資産など、個人以外のものを記述する XDM クラスを定義することもできます。 これらの [!DNL XDM Individual Profile] 以外のスキーマは「ディメンションエンティティ」（「ルックアップエンティティ」とも呼ばれます）と呼ばれ、時系列データを含みません。 ディメンションエンティティを表すスキーマは、[ スキーマ関係 ](../xdm/tutorials/relationship-ui.md) を使用してプロファイルエンティティにリンクされます。

ディメンションエンティティは、複数エンティティのセグメント定義を支援および簡素化するルックアップデータを提供します。また、セグメントエンジンが、処理の最適化（高速ポイントルックアップ）のためにデータセット全体をメモリに読み込めるようディメンションエンティティのサイズは小さくする必要があります。

![ プロファイルエンティティがディメンションエンティティで構成されていることを示すインフォグラフィック。](../profile/images/guardrails/profile-and-dimension-entities.png)

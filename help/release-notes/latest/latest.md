---
title: Adobe Experience Platform リリースノート
description: Experience Platformに関する最新のリリースノート
doc-type: release notes
last-update: July 15, 2020
author: crhoades, ens25212
translation-type: tm+mt
source-git-commit: f881c1365684b1ca9e6bf9a8ce866d234dc54128
workflow-type: tm+mt
source-wordcount: '679'
ht-degree: 5%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 7 月 15 日**

Adobe Experience Platform内の既存の機能の更新：

- [データガバナンス](#governance)
- [リアルタイム顧客プロファイル](#profile)
- [Segmentation Service](#segmentation)
- [ソース](#sources)

## [!DNL Data Governance] {#governance}

Adobe Experience Platformデータ・ガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーに対するコンプライアンスを確保するために使用される一連の戦略とテクノロジーです。 カタログ化、データ系列、データ使用状況のラベル付け、データアクセスポリシー、マーケティング活動 [!DNL Experience Platform] のためのデータに関するアクセス制御など、様々なレベルで重要な役割を果たします。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| ポリシーの自動適用 [!DNL Real-time Customer Data Platform] | データ使用ポリシーは、違反アクションが発生した場合に、宛先へのセグメントのアクティブ化など、自動的に適用されるようになりました。 [!DNL Real-time CDP] ポリシー違反がトリガーされると、アクティベーションワークフロー内の使用制限をリアルタイムで表示し、使用できないデータとその理由を示すことができます。<br><br>詳細については、の概要で、「データの使用 [に関するコンプライアンスの](../../rtcdp/privacy/data-governance-overview.md#enforce-data-usage-compliance) 適用 [!DNL Data Governance] 」 [!DNL Real-time CDP] の節を参照してください。 |
| Adobe Audience Manager統合 | 共有されるセグメントは、適用され [!DNL Audience Manager] たデータ使用ラベルを [!DNL Platform] 継承します。また、適用されたデータ使用ラベルも継承 [!DNL Data Export Controls]します。 使用ラベルとデータエクスポートコントロールの間の特定の [!DNL Audience Manager] マッピングについては、ドキュメントを参照してください [](https://docs.adobe.com/content/help/en/audience-manager/user-guide/implementation-integration-guides/integration-experience-platform/aam-aep-audience-sharing.html#aam-data-export-control-in-aep)。 |
| カスタムデータ使用ラベル | Policy Service APIまたはUIを使用して、カスタムデータ使用ラベルを作成できるようになりました。 See the [labels overview](../../data-governance/labels/overview.md) for more information. |

サービスの詳細については、 [データ管理の概要](../../data-governance/home.md) （英語）を参照してください。

## [!DNL Real-time Customer Profile] {#profile}

Adobe Experience Platformを使用すると、顧客がブランドとどこで、いつやり取りしたかに関係なく、顧客に対して、調整され、一貫性のある関連性のあるエクスペリエンスを提供できます。 を使用 [!DNL Real-time Customer Profile]すると、オンライン、オフライン、CRM、サードパーティなどの複数のチャネルからのデータを組み合わせた個々の顧客の全体的な表示を確認できます。 [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データ使用ポリシーの適用 | で [!DNL Real-time Customer Data Platform]は、  プロファイルワークスペース内の違反操作が試行されると、データ使用ポリシー違反が自動的に表示されます。 自動ポリシー適用の詳細については、「Data Governance [のリリースノート](#governance) 」を参照してください。 |

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platformセグメントサービスは、セグメントを作成して [!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよびRESTful APIを提供します。 これらのセグメントは一元的に設定および管理され [!DNL Platform]、アドビの任意のアプリケーションから容易にアクセスできます。

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、記録データ（人口統計情報など）や、ブランドに対する顧客のインタラクションを表す時系列イベントに基づくことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングセグメント | ストリーミングセグメントは、ユーザーをデータの到着地としてセグメントに分類できるようになり、セグメントの認定時間を大幅に短縮で [!DNL Platform]きます。 また、セグメントのストリーミングは、セグメント化ジョブを手動で実行する必要がなくなります。 |
| データ使用ポリシーの適用 | で [!DNL Real-time Customer Data Platform]は、 [!UICONTROL Segments] Workspaceで違反操作が実行されると、データ使用ポリシー違反が自動的に表示されます。 自動ポリシー適用の詳細については、「Data Governance [のリリースノート](#governance) 」を参照してください。 |

詳しくは、 [!DNL Segmentation Service][セグメント化の概要を参照してください。](../../segmentation/home.md)

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込みながら、サー [!DNL Platform] ビスを使用してデータの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRMシステムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] には、様々なデータプロバイダーのソース接続を簡単に設定できる、RESTful APIとインタラクティブUIが用意されています。 これらのソース接続を使用すると、外部のストレージシステムやCRMサービスの認証と接続、取り込みの実行時間の設定、データ取り込みスループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データフローの削除に対するUIのサポート | エラーが発生した、または不要になったデータフローは、UIを使用して削除できるようになりました。 |
| 1回の取り込みでのAPIとUIのサポート | 開始日のみが提供され、将来のインジェストがスケジュールされないデータフローの1回限りのインジェストは、APIまたはUIを使用して実行できるようになりました。 |

ソースについて詳しくは、 [ソースの概要を参照してください](../../sources/home.md)。

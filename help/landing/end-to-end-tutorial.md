---
keywords: Experience Platform;ホーム;人気のトピック;CJA;ジャーニー分析;Customer Journey Analytics;キャンペーンオーケストレーション;オーケストレーション;カスタマージャーニー;ジャーニー;ジャーニーオーケストレーション;機能;地域
title: Adobe Experience Platformのエンドツーエンドのサンプルワークフロー
description: Adobe Experience Platformの基本的なエンドツーエンドワークフローの概要を説明します。
exl-id: 0a4d3b68-05a5-43ef-bf0d-5738a148aa77
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1859'
ht-degree: 11%

---

# Adobe Experience Platformのエンドツーエンドのサンプルワークフロー

Adobe Experience Platform は、顧客体験を促進する完全なソリューションを構築し、管理するための、市場で最も強力で柔軟性の高いオープンシステムです。Experience Platform を使用すると、顧客データとコンテンツを任意のシステムから一元管理し、データサイエンスと機械学習を適用して、パーソナライズされた豊富なエクスペリエンスのデザインと配信を大幅に改善できます。

RESTful API に基づいて構築されたExperience Platformは、システムの全機能を開発者に公開し、使い慣れたツールを使用してエンタープライズソリューションを簡単に統合できるようにサポートします。 Experience Platformを使用すると、顧客データを取り込み、ターゲットとするオーディエンスへとデータをセグメント化し、それらのオーディエンスを外部の宛先に対してアクティブ化することで、顧客の全体像を把握できます。 次のチュートリアルでは、ソースを介した取り込みから宛先を介したオーディエンスアクティベーションまで、すべての手順を示すエンドツーエンドのワークフローを示します。

![Experience Platformのエンドツーエンドワークフロー &#x200B;](./images/end-to-end-tutorial/platform-end-2-end-workflow.png)

## はじめに

このエンドツーエンドワークフローでは、複数のAdobe Experience Platform サービスを使用します。 以下は、このワークフローで使用されるサービスのリストであり、その概要へのリンクを示しています。

- [[!DNL Experience Data Model (XDM)]](../xdm/home.md)：[!DNL Experience Platform] が、カスタマーエクスペリエンスデータを整理する際に使用する、標準化されたフレームワーク。セグメント化を最大限に活用するには、[データモデリングのベストプラクティス](../xdm/schema/best-practices.md)に従って、データがプロファイルとイベントとして取り込まれていることを確認してください。
- [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握できます。
- [ソース](../sources/home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [[!DNL Segmentation Service]](../segmentation/home.md)：[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータを細かいグループに分類できます。
- [[!DNL Real-Time Customer Profile]](../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。
- [&#x200B; データセット &#x200B;](../catalog/datasets/overview.md):[!DNL Experience Platform] でのデータ永続性を確保するためのストレージと管理の構成体。
- [&#x200B; 宛先 &#x200B;](../destinations/home.md)：宛先は、一般に使用されるアプリケーションとの事前定義済みの統合で、これを使用すると、Experience Platformのデータをシームレスにアクティブ化してクロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告およびその他の多くのユースケースを実現できます。

## XDM スキーマの作成

データをExperience Platformに取り込む前に、まず XDM スキーマを作成して、そのデータの構造を記述する必要があります。 次の手順でデータを取り込む場合、受信データをこのスキーマにマッピングします。 XDM スキーマの例の作成方法について詳しくは、[&#x200B; スキーマエディターを使用したスキーマの作成 &#x200B;](../xdm/tutorials/create-schema-ui.md) に関するチュートリアルを参照してください。

上記のチュートリアルでは、スキーマの ID フィールドを設定する方法を示します。 ID フィールドは、レコードまたは時系列イベントに関連する個々のユーザーを識別するために使用できるフィールドを表します。 ID フィールドは、Experience Platformでの顧客 ID グラフの作成方法において重要なコンポーネントです。これは、最終的に、リアルタイム顧客プロファイルが様々なデータフラグメントを結合して顧客の全体像を把握する方法に影響します。 Experience Platformの ID グラフの表示方法について詳しくは、[ID グラフビューアの使用方法 &#x200B;](../identity-service/features/identity-graph-viewer.md) に関するチュートリアルを参照してください。

スキーマをリアルタイム顧客プロファイルで使用できるようにするには、スキーマに基づくデータから顧客プロファイルを作成できるようにする必要があります。 詳しくは、スキーマ UI ガイドの [&#x200B; プロファイルのスキーマの有効化 &#x200B;](../xdm/ui/resources/schemas.md#profile) の節を参照してください。

## Experience Platform へのデータの取り込み

XDM スキーマを作成したら、データをシステムに取り込むことができます。

Experience Platformに取り込まれたすべてのデータは、取り込み時に個々のデータセットに保存されます。 データセットは、特定の XDM スキーマにマッピングされるデータレコードのコレクションです。 [!DNL Real-Time Customer Profile] でデータを使用する前に、問題のデータセットを明確に設定する必要があります。 プロファイルのデータセットを有効にする方法について詳しくは、[&#x200B; データセット UI ガイド &#x200B;](../catalog/datasets/user-guide.md#enable-profile) および [&#x200B; データセット設定 API チュートリアル &#x200B;](../profile/tutorials/dataset-configuration.md) を参照してください。 データセットを設定したら、データのデータセットへの取り込みを開始できます。

Experience Platformを使用すると、データを外部ソースから取得しながら、Experience Platform サービスを使用して、受信データの構造化、ラベル付け、拡張を行うことができます。 アドビのアプリケーション、クラウドベースのストレージ、データベースなど、様々なソースからデータを取り込むことができます。例えば、[Amazon S3](../sources/tutorials/api/create/cloud-storage/s3.md) を使用してデータを取り込むことができます。 使用可能なソースの完全なリストは、[&#x200B; ソースコネクタの概要 &#x200B;](../sources/home.md) で確認できます。

Amazon S3 をソースコネクタとして使用している場合は、[Amazon S3 コネクタの作成 &#x200B;](../sources/tutorials/api/create/cloud-storage/s3.md) に関する API チュートリアルまたは [Amazon S3 コネクタの作成 &#x200B;](../sources/tutorials/ui/create/cloud-storage/s3.md) に関する UI チュートリアルの手順に従って、コネクタ内でデータの作成、接続、取り込みを行う方法を学習できます。

ソースコネクタに関する詳細な手順については、[&#x200B; ソースコネクタの概要 &#x200B;](../sources/home.md) を参照してください。 ソースがベースとなっている API であるフローサービスについて詳しくは、[&#x200B; フローサービス API リファレンス &#x200B;](https://www.adobe.io/experience-platform-apis/references/flow-service/) を参照してください。

ソースコネクタを通じてデータがExperience Platformに取り込まれ、プロファイル対応データセットに保存されると、XDM スキーマに設定した ID データに基づいて顧客プロファイルが自動的に作成されます。

初めて新しいデータセットにデータをアップロードする際、または新しい ETL プロセスやデータソースを設定する際に、データが正しくアップロードされ、生成されたプロファイルに期待するデータが含まれていることを確認するように、データを慎重にチェックすることをお勧めします。 Experience Platform UI で顧客プロファイルにアクセスする方法について詳しくは、『 [&#x200B; リアルタイム顧客プロファイル UI ガイド &#x200B;](../profile/ui/user-guide.md) 』を参照してください。 リアルタイム顧客プロファイル API を使用してプロファイルにアクセスする方法について詳しくは、[&#x200B; エンティティエンドポイントの使用 &#x200B;](../profile/api/entities.md) に関するガイドを参照してください。

## データの評価

取り込んだデータからプロファイルを正常に生成したら、セグメント化を使用してデータを評価できます。 セグメント化は、マーケティング可能なユーザーグループを顧客ベースと区別するために、プロファイルストアから個人のサブセットで共有される特定の属性や行動を定義するプロセスです。 セグメント化について詳しくは、[&#x200B; セグメント化サービスの概要 &#x200B;](../segmentation/home.md) を参照してください。

### セグメント定義の作成

開始するには、セグメント定義を作成して、顧客をクラスター化し、ターゲットオーディエンスを作成する必要があります。 セグメント定義は、ターゲットとするオーディエンスの定義に使用できるルールのコレクションです。 セグメント定義を作成するには、[&#x200B; セグメントビルダーの使用に関する UI ガイドまたは &#x200B;](../segmentation/ui/segment-builder.md) セグメントの作成 [&#x200B; に関する API チュートリアルの手順に従 &#x200B;](../segmentation/tutorials/create-a-segment.md) ます。

セグメント定義を作成したら、セグメント定義 ID をメモしておいてください。

### セグメント定義の評価

セグメント定義を作成したら、セグメントジョブを作成してセグメントを 1 回限りのインスタンスとして評価することも、スケジュールを作成してセグメントを継続的に評価することもできます。

セグメント定義をオンデマンドで評価するには、セグメントジョブを作成します。 セグメントジョブは、参照されているセグメント定義と結合ポリシーに基づいて新しいオーディエンスセグメントを作成する非同期プロセスです。 結合ポリシーとは、顧客プロファイルの作成に使用するデータと、ソース間に不一致がある場合に優先順位を付けるデータを決定するためにExperience Platformで使用される一連のルールです。 結合ポリシーの使用方法については、[&#x200B; 結合ポリシー UI ガイド &#x200B;](../profile/merge-policies/ui-guide.md) を参照してください。

セグメントジョブを作成して評価すると、オーディエンスのサイズや処理中に発生したエラーなど、セグメントに関する情報を取得できます。 指定する必要のあるすべての詳細を含め、セグメントジョブの作成方法については、『 [&#x200B; セグメントジョブ開発者ガイド &#x200B;](../segmentation/api/segment-jobs.md) 』を参照してください。

セグメント定義を継続的に評価するには、スケジュールを作成し、有効にします。 スケジュールは、指定した時間に 1 日に 1 回、セグメントジョブを自動的に実行するためのツールです。 スケジュールを作成して有効にする方法については、[&#x200B; スケジュールエンドポイント &#x200B;](../segmentation/api/schedules.md) の API ガイドの手順に従ってください。

## 評価済みデータの書き出し

1 回限りのセグメントジョブまたは継続的なスケジュールを作成した後、セグメント書き出しジョブを作成して、結果をデータセットに書き出したり、結果を宛先に書き出したりできます。 次の節では、これらの両方のオプションに関するガイダンスを示します。

### 評価済みのデータをデータセットに書き出す

1 回限りのセグメントジョブまたは継続的なスケジュールを作成した後は、セグメント書き出しジョブを作成することで結果を書き出すことができます。 セグメント書き出しジョブは、評価されたオーディエンスに関する情報をデータセットに送信する非同期タスクです。

書き出しジョブを作成する前に、まずデータの書き出し先となるデータセットを作成する必要があります。 データセットの作成方法を学ぶには、セグメントの評価に関するチュートリアルの [&#x200B; ターゲットデータセットの作成 &#x200B;](../segmentation/tutorials/evaluate-a-segment.md#create-dataset) の節を読み、作成後にデータセット ID をメモしてください。 データセットを作成したら、書き出しジョブを作成できます。 エクスポートジョブの作成方法については、[&#x200B; エクスポートジョブエンドポイント &#x200B;](../segmentation/api/export-jobs.md) に関する API ガイドの手順に従ってください。

### 評価済みのデータを宛先に書き出す

または、1 回限りのセグメントジョブまたは継続的なスケジュールを作成した後で、結果を宛先に書き出すことができます。 宛先は、オーディエンスをアクティブ化して配信できる、外部サービスのAdobe アプリケーションなどのエンドポイントです。 使用可能な宛先の完全なリストは、[&#x200B; 宛先カタログ &#x200B;](../destinations/catalog/overview.md) で確認できます。

バッチまたはメールマーケティングの宛先に対してデータをアクティブ化する方法については、[Experience Platform UI を使用してプロファイル書き出しのバッチ宛先に対してオーディエンスデータをアクティブ化する方法 &#x200B;](../destinations/ui/activate-batch-profile-destinations.md) および [Flow Service API を使用してバッチ宛先に接続してデータをアクティブ化する方法 &#x200B;](../destinations/api/connect-activate-batch-destinations.md) に関するチュートリアルを参照してください。

## Experience Platform データアクティビティの監視

Experience Platformでは、データフロー（Experience Platformの様々なコンポーネント間でデータを移動するジョブを表します）を使用して、データがどのように処理されているかを追跡できます。 これらのデータフローは様々なサービスで構成され、ソースコネクタからターゲットデータセットにデータを移動できます。その後、[!DNL Identity Service] と [!DNL Real-Time Customer Profile] がデータを利用してから、最終的に宛先に対してアクティブ化します。 監視ダッシュボードは、データフローのジャーニーを視覚的に表現します。 Experience Platform UI 内のデータフローを監視する方法について詳しくは、[&#x200B; ソースのデータフロー監視 &#x200B;](../dataflows/ui/monitor-sources.md) および [&#x200B; 宛先のデータフロー監視 &#x200B;](../dataflows/ui/monitor-destinations.md) のチュートリアルを参照してください。

また、統計指標とイベント通知を使用して、[!DNL Observability Insights] を使用して、Experience Platform アクティビティを監視することもできます。 アラート通知は、Experience Platform UI を通じて登録したり、設定済みの Webhook に送信したりできます。 Experience Platform UI からの使用可能なアラートを表示、有効化、無効化および登録する方法について詳しくは、『 [[!UICONTROL &#x200B; アラート &#x200B;] UI ガイド &#x200B;](../observability/alerts/ui.md) 』を参照してください。 Webhook を通じてアラートを受け取る方法について詳しくは、[Adobe I/O イベント通知の登録 &#x200B;](../observability/alerts/subscribe.md) に関するガイドを参照してください。

## 次の手順

このチュートリアルを読むことで、Experience Platformのシンプルなエンドツーエンドフローの基本的な概要を学びました。 Experience Platformについて詳しくは、[Adobe Experience Platformの概要 &#x200B;](./home.md) を参照してください。 Experience Platform UI とExperience Platform API の使用について詳しくは、[Experience Platform UI ガイドと &#x200B;](./ui-guide.md)2&rbrace;Experience Platform API ガイドをお読みください [&#128279;](./api-guide.md)

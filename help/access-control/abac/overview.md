---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;属性ベースのアクセス制御;
title: 属性ベースのアクセス制御の概要
description: このドキュメントでは、Adobe Experience Platform の属性ベースのアクセス制御に関する情報を提供します
exl-id: 5495c55f-b808-40c1-8896-e03eace0ca4d
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '1874'
ht-degree: 71%

---

# 属性ベースのアクセス制御の概要 {#attribute-based-access-control-overview}

属性ベースのアクセス制御は、管理者が属性に基づいて特定のオブジェクトや機能へのアクセスを制御できるようにする Adobe Experience Platform の機能です。属性は、スキーマフィールドやセグメントに追加されるラベルなど、オブジェクトに追加されるメタデータであることがあります。 管理者は、ユーザーアクセス権限を管理する属性を含めた、アクセスポリシーを定義します。

この機能を使用すると、エクスペリエンスデータモデル（XDM）スキーマフィールドに、組織またはデータの使用範囲を定義するラベルを付けることができます。 同時に、管理者は、ユーザーと役割の管理インターフェイスを使用して、XDM スキーマフィールドに関するアクセスポリシーを定義し、ユーザーまたはユーザーのグループ（内部、外部、またはサードパーティのユーザー）に与えるアクセスをうまく管理できます。また、属性ベースのアクセス制御により、管理者は特定のセグメントへのアクセスを管理できます。

>[!IMPORTANT]
>
>属性ベースのアクセス制御をExperience Platformのデータガバナンス機能と混同しないでください。つまり、組織内のどのユーザーがアクセス権を持っているかではなく、ラベルとポリシーを使用してExperience Platformでのデータの使用方法を制御できます。 詳しくは、[ データガバナンスの概要 ](../../data-governance/home.md) を参照してください。

属性ベースのアクセス制御により、組織の管理者は、すべてのExperience Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、カスタマイズされた種類のデータへのユーザーのアクセスを制御できます。 管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

次のビデオは、属性ベースのアクセス制御についての理解を深めるために、また、役割、リソース、ポリシーを設定する方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/3451828?learn=on&captions=jpn)

## 属性ベースのアクセス制御用語

属性ベースのアクセス制御には、次のコンポーネントが含まれます。

| 用語 | 定義 |
| --- | --- |
| 属性 | 属性は、ユーザーとユーザーがアクセスできるExperience Platform リソースとの相関関係を示す識別子です。 属性は、スキーマフィールドやセグメントに追加されるラベルなど、オブジェクトに追加されるメタデータであることがあります。 管理者は、ユーザーアクセス権限を管理する属性を含めた、アクセスポリシーを定義します。 |
| ラベル | ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。ラベルはいつでも適用でき、データの管理方法を柔軟に選択できます。ベストプラクティスでは、データがExperience Platformに取得されるとすぐに、またはデータがExperience Platformで使用できるようになるとすぐに、データのラベル付けが推奨されます。 |
| 権限 | 権限には、サンドボックスの作成、スキーマの定義、データセットの管理など、Experience Platform の機能の表示や使用の機能が含まれます。 |
| 権限セット | 権限セットは、管理者が役割に適用できる権限のグループを表します。 管理者は、個々の権限を割り当てる代わりに、役割に権限セットを割り当てることができます。これにより、権限のグループを含む事前定義済みの役割からカスタムの役割を作成できます。 |
| ポリシー | ポリシーとは、属性を統合して、許容されるアクションと許容されないアクションを確立するステートメントです。ポリシーはローカルまたはグローバルのいずれかであり、他のポリシーを上書きできます。 |
| リソース | リソースとは、サブジェクトがアクセスできるまたはアクセスできないアセットやオブジェクトです。 リソースには、セグメントまたはスキーマフィールドを使用できます。 |
| ロール | 役割は、Experience Platform インスタンスとやり取りするユーザーのタイプを分類する方法で、アクセス制御ポリシーの構成要素になります。 役割ベースのアクセス制御環境では、ユーザーアクセスプロビジョニングは、共通の責任とニーズによってグループ化されます。役割には特定の権限セットがあり、必要な表示または書き込みアクセスの範囲に応じて、組織のメンバーを 1 つ以上の役割に割り当てることができます。 |
| 件名 | サブジェクトとは、アクションを実行するためにリソースへのアクセスを要求するユーザーのことです。 |
| ユーザーグループ | ユーザーグループとは、同じ機能を実行するためのアクセス権を持つ、グループ化された複数のユーザーのことです。 |

## 権限

>[!IMPORTANT]
>
>組織で属性ベースのアクセス制御を有効にすると、Adobe Admin Consoleのロールの代わりにAdobe Experience Cloudの権限を使用して、組織内のユーザー、機能、ラベル、その他のリソースに対する権限の管理を開始できます。

権限は、管理者がユーザーの役割およびアクセスポリシーを定義し、製品アプリケーション内の機能およびオブジェクトのアクセス権限を管理できる、Experience Cloud の領域です。

権限を通じて、役割を作成および管理し、それらの役割に対して必要なリソース権限を割り当てることができます。また、権限では、特定の役割に関連付けられたラベル、サンドボックス、ユーザーを管理することもできます。詳しくは、[権限 ガイド](ui/browse.md)を参照してください。

## 属性ベースのアクセス制御 API

属性ベースのアクセス制御 API により、API を使用してExperience Platform内のロール、ポリシーおよびプロダクトをプログラムで管理できます。 詳しくは、[API を使用した属性ベースのアクセス制御設定の管理](api/overview.md)に関するガイドを参照してください

## Adobe Experience Platform の属性ベースのアクセス制御

次の節では、属性ベースのアクセス制御をExperience Platformの他のコンポーネントに統合する方法について説明します。

### アクセス制御

Experience Platformは、[Adobe Admin Console](https://adminconsole.adobe.com) の役割を活用して、ユーザーを権限とサンドボックスにリンクします。 権限は、データモデリング、プロファイル管理、サンドボックス管理など、様々なExperience Platform機能へのアクセスを制御します。 組織で属性ベースのアクセス制御を有効にすると、Adobe Admin Consoleのロールの代わりにAdobe Experience Cloudの権限を使用して、組織内のユーザー、機能、ラベル、その他のリソースに対する権限の管理を開始できます。

ヘルスケアやプライバシーシールドを購入したお客様に対しては、属性ベースのアクセス制御に対する利用が制限されます。この機能の特長は次のとおりです。

* 権限インターフェイス：属性ベースのアクセス制御のユーザーの役割、権限、ポリシーを定義するためのインターフェイスを提供します。

* ラベル付け：アクセス制御ポリシーを活用するために、ユーザーの役割、スキーマフィールド、セグメント、その他のサポートされているオブジェクトにラベルを追加、編集、削除します。 **メモ：** 同じアクセス制限を適用する場合、ラベル付き属性を利用するセグメントにも同様にラベルを付ける必要があります。

Admin Console から新しい権限インターフェイスへのすべての Experience Platform を利用したアプリケーションの管理ワークフローが切り替えられます。

>[!IMPORTANT]
>
>役割は、組織が有効になると自動的に権限インターフェイスに移行されます。 Admin Consoleの役割は当分の間、そのまま残ります。 組織が有効になった後で役割を変更 **しない** ください。

アクセス制御について詳しくは、[アクセス制御の概要](../home.md)を参照してください。

### 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

管理者は、属性ベースのアクセス制御機能を使用して、次の操作を実行できます。

* 役割、権限、ラベルに基づいて、アクティベーションプロセスの特定のセグメントを表示するためのユーザーアクセスを設定します。
   * アクティベーションプロセスでは、ユーザーは、宛先に対してアクティブ化するセグメントを選択する必要がある場合があります。 管理者は、組織内のユーザーをプロビジョニングして、ユーザーがアクセスできるラベルでラベル付けされたセグメントと、ラベルを含まないセグメントのみを表示できます。
* ロール、権限、ラベルに基づいて、アクティベーションプロセスで特定のフィールドを表示するようにユーザーアクセスを設定します。
   * アクティベーションプロセスでは、ユーザーは、宛先に対してアクティブ化するフィールドを選択する必要が生じる場合があります。管理者は、組織内のユーザーをプロビジョニングして、ユーザーがアクセスできるラベルでラベル付けされたフィールドと、ラベルを含まないフィールドのみを表示できます。

>[!IMPORTANT]
>
>要約すると、宛先と属性ベースのアクセス制御を使用する際には、次の点に注意してください。
>
>* アクティブ化できるのは、アクティベーションワークフローの [Audience Portal](/help/segmentation/ui/audience-portal.md#browse) および [ セグメント手順を選択 ](/help/destinations/ui/activate-batch-profile-destinations.md#select-segments) でアクセスおよび表示する権限を持つオーディエンスのみです。
>* [アクティベーションワークフローのマッピング手順](/help/destinations/ui/activate-segment-streaming-destinations.md#mapping)では、アクセス権限のあるフィールドのみを表示して選択してアクティブ化できます。
>* 書き出し用にマッピングされるすべてのフィールドへのアクセス権を持たない既存の宛先に対して、追加のセグメントをアクティブ化しようとすると、アクティベーションワークフローがブロックされます。

[!DNL Destinations] について詳しくは、[[!DNL Destinations] 概要](../../destinations/home.md)を参照してください。

### ID サービス

Adobe Experience Platform [!DNL Identity Service] を利用すると、デバイスやシステム間の ID を橋渡しすることで顧客と顧客の行動をよりよく把握でき、インパクトのある、パーソナライズされたデジタル体験をリアルタイムで提供できます。

属性ベースのアクセス制御の一環として、`view-identity-graph` 権限を使用すると、ユーザーインターフェイスまたは API で ID グラフにアクセスできる組織内のユーザーを決定できます。 詳しくは、[ID グラフビューアへのアクセス](../../identity-service/features/identity-graph-viewer.md)に関するガイドを参照してください.

[!DNL Identity Service] について詳しくは、[[!DNL Identity Service] 概要](../../identity-service/home.md)を参照してください。

### リアルタイム顧客プロファイル

Experience Platformを使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。 リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、個別の顧客データを統合ビューに統合し、顧客のやり取りごとに実用的なタイムスタンプ付きの説明を提供できます。

管理者は、属性ベースのアクセス制御機能を使用して、次の操作を実行できます。

* 役割、権限、ラベルに基づいた、特定のプロファイル属性に対するユーザーアクセスの設定。
   * 管理者は、組織内のユーザーをプロビジョニングして、ユーザーがアクセスできるラベルでラベル付けされたプロファイル属性と、ラベルを含まないプロファイル属性のみを表示できます。
   * 管理者は、セグメントの作成時に、組織内のユーザーをプロビジョニングして、ユーザーがアクセスできるラベルでラベル付けされたプロファイル属性のみを表示できます。
* データモデルの XDM スキーマで使用される特定のデータフィールドにラベルを付けることによる、データプレビューへのユーザーアクセスの設定。

プロファイルについて詳しくは、[プロファイルの概要](../../profile/home.md)を参照してください。

### セグメント化サービス

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

管理者は、属性ベースのアクセス制御機能を使用して、次の操作を実行できます。

* 役割、権限、ラベルに基づいた、特定のセグメントを表示および管理するためのユーザーアクセスの設定。
   * 管理者は、セグメント化 UI の使用時に、組織内のユーザーをプロビジョニングして、ユーザーがアクセスできるラベルでラベル付けされたセグメントと、ラベルを含まないセグメントのみを表示できます。

[!DNL Segmentation Service] について詳しくは、[[!DNL Segmentation Service] 概要](../../segmentation/home.md)を参照してください。

### XDM

エクスペリエンスデータモデル（XDM）は、デジタルエクスペリエンスを向上するために設計されたオープンソースの仕様です。Experience Platform上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

属性ベースのアクセス制御を使用すると、次を行うことができます。

* [フィールドグループとクラスにデータ使用ラベルを適用できます](../../xdm/tutorials/labels.md)。これにより、同じフィールドグループまたはクラスを持つ複数のスキーマで、フィールドグループまたはクラスレベルの設定に応じて、同じ属性でタグ付けされたフィールドを使用できます。
* ユーザーに割り当てられた役割に適用される権限セットに応じて、特定の XDM スキーマフィールドへのユーザーアクセスを設定できます。

XDM について詳しくは、[XDM の概要](../../xdm/home.md)を参照してください。
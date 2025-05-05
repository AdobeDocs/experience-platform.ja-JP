---
title: Real-time Customer Data Platform B2B Edition のセグメント化のユースケース
description: Adobe Real-time Customer Data Platform B2B Edition で使用可能な様々なユースケースの概要です。
feature: Get Started, Audiences, Segments, B2B
badgeB2B: label="B2B エディション" type="Informative" url="https://helpx.adobe.com/legal/product-descriptions/real-time-customer-data-platform-b2b-edition-prime-and-ultimate-packages.html newtab=true"
exl-id: 2a99b85e-71b3-4781-baf7-a4d5436339d3
source-git-commit: 8a487d948d2eb7db167298b61045ef8dd2099da6
workflow-type: tm+mt
source-wordcount: '1422'
ht-degree: 1%

---

# Real-time Customer Data Platform B2B Edition のセグメント化のユースケース

このドキュメントでは、Adobe Real-time Customer Data Platform B2B Edition のセグメント定義の例と、一般的な B2B のユースケースに対して様々なタイプの属性を組み合わせる方法について説明します。 宛先がお客様の B2B ワークフローにどのように適合するかを理解するには、[ エンドツーエンドのチュートリアル ](../b2b-tutorial.md#create-a-segment-to-evaluate-your-data) を参照してください。

>[!NOTE]
>
>これらのセグメント化のユースケースに必要な属性は、Real-time Customer Data Platform B2B Edition のお客様のみが使用できます。 Real-time Customer Data Platform B2B Edition を使用していない場合は、代わりに [ セグメント化の概要 ](./segmentation-overview.md) を参照してください。

## 前提条件 {#prerequisites}

B2B クラスにセグメント属性を使用する前に、次の手順を完了する必要があります。

1. B2B クラスを使用するスキーマを作成します。 B2B Edition のクラスには、Account、Campaign、Opportunity、Marketing List などが含まれます。 [B2B クラスで使用するスキーマの設定方法 ](../schemas/b2b.md) について詳しくは、スキーマドキュメントを参照してください。
2. エクスペリエンスデータモデル（XDM） B2B スキーマ間の関係を作成します。 B2B エディションの属性に基づくオーディエンスでは、拡張された B2B セグメント化機能を完全に使用するためにクラス間の関係が必要です。 詳しくは、[2 つの B2B スキーマ間の関係を定義する方法 ](../../xdm/tutorials/relationship-b2b.md) に関するドキュメントを参照してください。
3. B2B スキーマに基づくデータセットを使用して、データを取り込みます。 [ データの取り込み方法について ](../../sources/connectors/adobe-applications/marketo/marketo.md) については、ソースドキュメントを参照してください。
4. オーディエンスの作成方法に関する詳細なガイダンスについては、[ セグメントビルダーユーザーガイド ](../../segmentation/ui/segment-builder.md) を参照してください。

これらの要件が満たされたら、これらの属性を組み合わせて、一般的な B2B ユースケースを実現することができます。

## はじめに {#getting-started}

B2B クラスの結合スキーマの関係が確立され、データの取り込みに使用されると、その属性はセグメントビルダーの左側のパネルで使用できるようになります。

B2B クラスとその属性には、Real-time Customer Data Platform内で標準として使用可能なものと区別するために、セグメント化ワークスペース内の `B2B` ラベルが付きます。

B2B ユースケースのオーディエンスを効果的に作成するには、スキーマを熟知し、データモデルがどのように見えるかを理解することが重要です。 また、データがデータオブジェクト間で受け取るパスを把握しておくと便利です。

次の図は、Real-Time CDP B2B Edition 内で使用可能な B2B クラス間の関係を示しています。

![B2B クラス ERD](../assets/segmentation/b2b-classes.png)

データモデルは複雑になる可能性があるので、Platform UI を使用して、データモデルのより詳細な視覚表現を表示し、ユースケースに関連する属性を見つけやすくできます。 開始するには、Platform UI に移動し、左側のナビゲーションで「スキーマ」を選択します。

使用可能なリストから適切なスキーマを選択し、[!UICONTROL &#x200B; 構成 &#x200B;] サイドパネルから適切な関係を選択します。 次の例では、「人物」関係を選択すると、現在のスキーマのどの属性が関連する「人物」スキーマを参照するか（関係のソーススキーマの場合）、「人物」スキーマによって参照されるか（関係の参照スキーマの場合）が表示されます。

![ スキーマワークスペースで人物の関係を使用したソースキーの例 ](../assets/segmentation/source-key-schema-relationship-example.png)

この関係は、次の画像に示すように、`Key` フォルダーを使用してセグメントビルダー内に反映されます。

![ セグメント化ワークスペースでセグメントビルダーを使用したソースキーの例 ](../assets/segmentation/source-key-segmentation-example.png)

使用可能な B2B クラスについて詳しくは、[Real-time Customer Data Platform B2B Edition のスキーマ ](../schemas/b2b.md) ドキュメントを参照してください。

以下のユースケースでは、これらの結果を得るために、異なるスキーマ間の関係を確立するために使用するクラスに関する情報を提供します。 これらの例は、独自のオーディエンスの作成に役立ちます。

## 様々なセグメント化の使用例 {#use-cases}

B2B Edition でのセグメント化には、次のユースケースが使用できます。 各例では、オーディエンスの実行内容の説明と、それらの作成に使用されるクラスの説明を提供しています。 提供される画像は、スキーマの構造を反映した [!UICONTROL &#x200B; 属性 &#x200B;] サイドレールのファイルパスをハイライト表示します。 ディスプレイの右側にある [!UICONTROL &#x200B; セグメントのプロパティ &#x200B;] セクションには、オーディエンスの属性の分類が書き込まれています。

### 例 1:B2B オポチュニティの「意思決定者」を検索 {#find-decision-maker}

あらゆるオポチュニティの「意思決定者」であるすべての人物を検索します。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラスと [!UICONTROL XDM Business Opportunity Person Relation] クラスの間をリンクさせる必要があります。

![ サンプル 1 の設定を表示する UI](../assets/segmentation/example-1.png)

### 例 2：特定のドル金額を超える商談に割り当てられた B2B プロファイルの検索 {#find-opportunities-amount}

商談額が指定額（100 万ドル）を超える商談に直接割り当てられている人物をすべて検索します。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Opportunity Person Relation] クラス、および [!UICONTROL XDM Business Opportunity] クラスの間をリンクしている必要があります。

![ サンプル 2 の設定を表示する UI](../assets/segmentation/example-2.png)

### 例 3：場所ごとに商談に割り当てられた B2B プロファイルの検索 {#find-opportunities-location}

アカウントが特定の場所（カナダ）にある機会に直接割り当てられている人物をすべて検索します。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Opportunity Person Relation] クラス、[!UICONTROL XDM Business Opportunity] クラス、および [!UICONTROL XDM Business Account] クラスの間をリンクしている必要があります。

![ サンプル 3 の設定を示す UI](../assets/segmentation/example-3.png)

### 例 4：業界別およびブラウジング行動別の機会の「意思決定者」を検索する {#find-industry-browsing-behavior}

アカウントが「金融」業界に属する機会の「意思決定者」であるすべての人物を見つけ、過去 3 日間に価格ページにアクセスしました。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Opportunity Person Relation] クラス、[!UICONTROL XDM Business Opportunity] クラス、[!UICONTROL XDM Business Account] クラス、[!UICONTROL XDM ExperienceEvent] クラスの間をリンクしている必要があります。

![ サンプル 4 の設定を示す UI](../assets/segmentation/example-4.png)

### 例 5：部門名と商談額による商談の B2B プロファイルの検索 {#find-department-opportunity-amount}

人事部門で働き、指定された金額（100 万ドル）以上に相当するオープン商談が 1 つ以上あるアカウントを持つすべての人物を検索します。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Account] クラス、および [!UICONTROL XDM Business Opportunity] クラスをリンクしている必要があります。

![ サンプル 5 の設定を示す UI](../assets/segmentation/example-5.png)

### 例 6：役職および年収で B2B プロファイルを検索する {#find-by-job-title-and-revenue}

副社長という役職を持ち、年間売上高が指定された額（1 億ドル）以上のアカウントを持つすべての人を検索します。また、先月に少なくとも 3 回は価格ページにアクセスしました。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Account] クラス、および [!UICONTROL XDM ExperienceEvent] クラスをリンクしている必要があります。

![ サンプル 6 設定を表示する UI](../assets/segmentation/example-6.png)

### 例 7：商談のステータスとブラウジング行動で「意思決定者」を検索 {#find-by-opportunity-status-and-browsing-behavior}

クローズ済みの失注したオポチュニティの「意思決定者」であるすべての人物を検索し、先週の価格ページにアクセスしました。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Opportunity Person Relation] クラス、[!UICONTROL XDM Business Opportunity] クラス、[!UICONTROL XDM ExperienceEvent] クラスの間をリンクしている必要があります。

![ サンプル 7 設定を表示する UI](../assets/segmentation/example-7.png)

### 例 8：関連するアカウントを使用してセグメント化のリーチを拡大 {#related-accounts}

人事部門で勤務し、指定された金額（100 万ドル）以上のオープン商談が 1 つ以上ある任意のアカウント *またはアカウントに関連する任意のアカウント* またはアカウントの関連するアカウント）に関連するすべての従業員を検索します。 このオーディエンスは、[!UICONTROL XDM Individual Profile] クラス、[!UICONTROL XDM Business Account] クラス、および [!UICONTROL XDM Business Opportunity] クラスをリンクしている必要があります。

![ 関連するアカウントのセグメント化を表示する UI](../assets/segmentation/example-8.png)

### 例 9：リードスコアやアカウントスコアを使用してプロファイルを選定 {#account-scoring}

リードスコアが 80 以上のすべてのプロファイルを検索します。

![ リードおよびアカウントの予測スコアリングに関するセグメント化を表示する UI](../assets/segmentation/example-9.png)

### 例 10：親組織が特定のドル金額を超える売上高を持つアカウントに関連付けられた B2B プロファイルの検索 {#find-parent-org-amount}

親組織に、指定された金額（$100,000,000）を超える収益があるアカウントに関連付けられているすべての人物を検索します。

![ セグメント化の親組織を表示する UI](../assets/segmentation/example-10.png)

### 例 11：アクティブな関係を持つ役職とアカウント名で B2B プロファイルを検索する {#find-by-job-title-and-account-name}

アカウントの関係が「アクティブ」であるアカウント「Acme」の「マネージャー」であるすべての人々を見つけます。

![ セグメント化の親組織を表示する UI](../assets/segmentation/example-11.png)

### 例 12:actualCost が budgetedCost を超えるキャンペーンをターゲットとした B2B プロファイルの検索 {#find-actualcost-exceed-budgetcost}

actualCost が budgetedCost を超えたキャンペーンのターゲットであるすべての人物を検索します。

![ セグメント化の親組織を表示する UI](../assets/segmentation/example-12.png)

### 例 13:Marketo静的リストに属し、isDeleted=false である B2B プロファイルを検索する {#find-marketo-static-list}

isDeleted=false で、Marketo静的リスト「Anniversary users」に属するすべてのユーザーを検索します。

![ セグメント化の親組織を表示する UI](../assets/segmentation/example-13.png)

## 次の手順 {#next-steps}

この概要を読むことで、Real-Time CDP B2B Edition を使用して利用できるセグメント化の可能性について理解できました。 セグメント化サービスの詳細については、[セグメント化に関するドキュメント](../../segmentation/home.md)を参照してください。

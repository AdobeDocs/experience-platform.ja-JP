---
keywords: セグメント化；セグメント化rtcdp；リアルタイム顧客データプラットフォームセグメント化
title: セグメント化サービスの概要
seo-title: リアルタイム顧客データプラットフォームにおけるセグメント化サービス
description: リアルタイム CDP は Adobe Experience Platform をベースに構築され、多くの Experience Platform サービスと機能を利用します。セグメント化サービスを使用すると、顧客を類似の特徴を持つ小さなグループに分けて、独自のマーケティングを提供できます。
translation-type: tm+mt
source-git-commit: 9f20f8bfeefb5bc427a72a11ac1f01816cdbc4b2
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 33%

---


# [!DNL Segmentation Service] in [!DNL Real-time Customer Data Platform]

[!DNL Real-time Customer Data Platform] （リアルタイムCDP）では、複数のソースからのデータを取得し、お客様に対して調整と一貫性のあるエクスペリエンスを提供できます。関連するパーソナライズされたマーケティングキャンペーンを提供するには、Adobe Experience Platformの[!DNL Segmentation Service]を使用します。

リアルタイムCDPはAdobe Experience Platformの上に構築されており、多くの[!DNL Experience Platform]サービスと機能を利用しています。 [!DNL Segmentation Service]を使用すると、類似の特性を持つ小さなグループに顧客を分けることで、カスタマイズしたマーケティングを提供できます。

## セグメント化

セグメント化とは、プロファイルストアにあるプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから選別するプロセスです。例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。

## [!DNL Segment Builder]

[!DNL Platform] セグメントを簡単に作成してアクセスでき、異なる構築ブロックを使用してセグメントの特徴をさらに明確にできます。セグメントビルダーの使用方法の詳細については、『[セグメントビルダーガイド](./segment-builder-guide.md)』を参照してください。

## 顧客 AI

リアルタイム顧客データプラットフォームに含まれる顧客AIは、説明を加えながら個々のレベルで顧客予測を生み出す力を提供します。

顧客 AI は、影響力のある要因の助けを借りて、顧客が何をする可能性があるかとその理由を知ることができます。さらに、最も適切なオファーとメッセージを提供することで、顧客AIの予測とインサイトを活用して顧客体験をパーソナライズできます。 お客様向けAIは、次の機能をサポートします。

* より強力なセグメント化およびターゲティングを行うための、高精度な顧客傾向モデルの提供。
* 特定の顧客行動の背後にある影響要因と可能性を把握する。
* 会社固有の使用例とデータに対してカスタマイズ可能なオプションを提供する。
* チャーンやコンバージョンなどの顧客傾向スコアによるリアルタイム顧客プロファイルの強化。
* 傾向スコアの影響力のある要因による顧客プロファイルの向上。
* 影響力のある要因と傾向スコアに基づく顧客のセグメントの作成。

顧客AIは、**[!UICONTROL Adobeサービス]**&#x200B;の下の&#x200B;**[!UICONTROL サービス]**&#x200B;タブにあります。

![顧客AIの場所](../assets/overview/rtcdp-customer-ai.png)

### 顧客 AI を使い始める

顧客AIを使い始めるには、[data preperation tutorial](../../intelligent-services/data-preparation.md)に従って、使用事例に基づいて入力スキーマを設定する必要があります。 次に、[顧客AIインスタンス](../../intelligent-services/customer-ai/user-guide/configure.md)を設定する必要があります。 インスタンスを設定した後、モデルが生成され、インサイトとスコアを[表示できます](../../intelligent-services/customer-ai/user-guide/discover-insights.md)。 モデルから生成されたデータを使用して、データ駆動型アクティベーションのセグメントを作成できます。

お客様のAIの詳細については、[お客様のAIの概要](../../intelligent-services/customer-ai/overview.md)を参照して、開始をご確認ください。 さらに、次のビデオでは、顧客AIが顧客プロファイルにAIベースの傾向を強化し、顧客のセグメント化とターゲティングの取り組みを強化する方法を示しています。

>[!VIDEO](https://video.tv.adobe.com/v/40374/?quality=12&learn=on)


## 次の手順

この概要を読んだ後、リアルタイムCDPが[!DNL Segmentation Service]をどのように利用してマーケティングキャンペーンのカスタマイズとパーソナライズを強化しているかを理解する必要があります。 [!DNL Segmentation Service]の詳細については、[セグメント化に関するドキュメント](../../segmentation/home.md)を参照してください。

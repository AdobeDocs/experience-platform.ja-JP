---
keywords: セグメント化；セグメント化 rtcdp；リアルタイム顧客データプラットフォームセグメント化
title: リアルタイム顧客データプラットフォームでのセグメント化サービス
seo-title: Segmentation Service in Real-time Customer Data Platform
description: リアルタイム CDP は Adobe Experience Platform をベースに構築され、多くの Experience Platform サービスと機能を利用します。セグメント化サービスを使用すると、顧客を類似の特徴を持つ小さなグループに分けて、独自のマーケティングを提供できます。
exl-id: 140667c0-e288-40c4-8c45-c275e348b84a
source-git-commit: 0085306a2f5172eb19590cc12bc9645278bd2b42
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 31%

---

# [!DNL Segmentation Service] in [!DNL Real-time Customer Data Platform]

[!DNL Real-time Customer Data Platform] （リアルタイム CDP）を使用すると、複数のソースからデータを取り込み、顧客に対して調整され、一貫性のあるエクスペリエンスを提供できます。関連するパーソナライズされたマーケティングキャンペーンの配信は、Adobe Experience Platformの [!DNL Segmentation Service] を使用して実現できます。

リアルタイム CDP はAdobe Experience Platformをベースに構築され、多くの [!DNL Experience Platform] サービスと機能を利用します。 [!DNL Segmentation Service] を使用すれば、顧客を類似の特性を持つ小さなグループに分けて、独自のマーケティングを提供できます。

## セグメント化

セグメント化とは、プロファイルストアにあるプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから選別するプロセスです。例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。

## [!DNL Segment Builder]

[!DNL Platform] では、セグメントを簡単に作成してアクセスでき、様々な構成要素を使用してセグメントをさらに特徴付けることができます。セグメントビルダーの使用方法の詳細については、『[セグメントビルダーガイド](./segment-builder-guide.md)』を参照してください。

## 顧客 AI

リアルタイム顧客データプラットフォームに含まれる顧客 AI は、顧客予測と説明を個人レベルで生み出す力を提供します。

顧客 AI は、影響力のある要因の助けを借りて、顧客が何をする可能性があるかとその理由を知ることができます。さらに、顧客 AI の予測とインサイトから得たメリットを活用して、最も適切なオファーとメッセージを提供することで、顧客体験をパーソナライズできます。 顧客 AI は、次の機能をサポートします。

* 顧客の傾向モデルを高精度で提供し、セグメント化とターゲティングを強化する。
* 特定の顧客行動に影響を及ぼす要因と可能性を把握する。
* 自社独自の使用例やデータに合わせてカスタマイズ可能なオプションを提供する。
* チャーンやコンバージョンなどの顧客の傾向スコアによるリアルタイム顧客プロファイルの強化。
* 傾向スコアに影響を与える要因による顧客プロファイルの強化。
* 影響を及ぼす要因と傾向スコアに基づく顧客のセグメントの作成

顧客 AI は、**[!UICONTROL Adobe サービス]** の下の「**[!UICONTROL サービス]**」タブにあります。

![顧客 AI の場所](../assets/overview/rtcdp-customer-ai.png)

### 顧客 AI を使い始める

顧客 AI の使用を開始するには、[ データの操作に関するチュートリアル ](../../intelligent-services/data-preparation.md) に従い、使用事例に基づいて入力スキーマを設定する必要があります。 次に、顧客 AI インスタンス ](../../intelligent-services/customer-ai/user-guide/configure.md) を設定する必要があります。 [インスタンスを設定すると、モデルが生成され、[ インサイトとスコアを表示 ](../../intelligent-services/customer-ai/user-guide/discover-insights.md) できます。 モデルから生成されたデータを使用して、データ駆動型のアクティベーション用のセグメントを作成できます。

顧客 AI の詳細については、まず [ 顧客 AI の概要 ](../../intelligent-services/customer-ai/overview.md) を参照してください。 さらに、次のビデオでは、顧客 AI がどのように AI ベースの傾向で顧客プロファイルを充実させ、顧客セグメント化やターゲティングの取り組みを強化するかを示します。

>[!VIDEO](https://video.tv.adobe.com/v/40374/?quality=12&learn=on)


## 次の手順

この概要を読むと、リアルタイム CDP が [!DNL Segmentation Service] を利用してマーケティングキャンペーンのカスタマイズとパーソナライゼーションを強化する方法を理解できます。 [!DNL Segmentation Service] の詳細については、[ セグメント化に関するドキュメント ](../../segmentation/home.md) を参照してください。

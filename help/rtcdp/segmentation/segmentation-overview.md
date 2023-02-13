---
keywords: セグメント化;セグメント化 rtcdp;Real-Time Customer Data Platform セグメント化
title: Real-Time Customer Data Platform におけるセグメント化サービス
description: Adobe Real-Time Customer Data Platform は Adobe Experience Platform をベースに構築され、多くの Experience Platform サービスと機能を利用します。セグメント化サービスを使用すると、顧客を類似の特性を持つ小さなグループに分けて、独自のマーケティングを提供できます。
exl-id: 140667c0-e288-40c4-8c45-c275e348b84a
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: ht
source-wordcount: '540'
ht-degree: 100%

---

# [!DNL Real-Time Customer Data Platform] における [!DNL Segmentation Service]

[!DNL Adobe Real-Time Customer Data Platform]（Real-Time CDP）を使用すると、複数のソースからデータを取り込み、顧客に合わせた一貫性のあるエクスペリエンスを提供できます。関連するパーソナライズされたマーケティングキャンペーンの配信は、Adobe Experience Platform の一部である [!DNL Segmentation Service] を使用して実現できます。

Real-Time CDP は Adobe Experience Platform をベースに構築され、多くの [!DNL Experience Platform] サービスと機能を利用します。[!DNL Segmentation Service] を使用すると、顧客を類似の特性を持つ小さなグループに分けて、独自のマーケティングを提供できます。

## セグメント化

セグメント化とは、プロファイルストアにあるプロファイルのサブセットによって共有される特定の属性や行動を定義し、マーケティング可能な人々のグループを顧客ベースから選別するプロセスです。例えば、「スニーカーを購入し忘れましたか？」という電子メールキャンペーンでは、過去 30 日間にランニングシューズを検索したが購入を完了しなかったすべてのユーザーのオーディエンスが必要な場合があります。様々なセグメントを使用すると、様々なオーディエンスに焦点を当て、よりカスタマイズされたマーケティングエクスペリエンスを提供できます。

## [!DNL Segment Builder]

[!DNL Platform] では、セグメントを簡単に作成してアクセスでき、様々な構成要素を使用してセグメントをさらに特徴付けることができます。セグメントビルダーの使用方法の詳細については、『[セグメントビルダーガイド](./segment-builder-guide.md)』を参照してください。

## 顧客 AI

Real-Time Customer Data Platform に含まれる顧客 AI は、説明を使用して個人レベルで顧客予測を生成する機能を提供します。

顧客 AI は、影響力のある要因の助けを借りて、顧客が何をする可能性があるかとその理由を知ることができます。さらに、顧客 AI の予測とインサイトを活用して、最も適切なオファーとメッセージを提供することで顧客体験をパーソナライズできます。顧客 AI は、次の点で役立ちます。

* 顧客の傾向モデルを高精度で提供してセグメント化とターゲティングを強化する
* 特定の顧客行動の背後にある影響要因と可能性を把握する
* 企業固有のユースケースやデータに合わせてカスタマイズ可能なオプションを提供する
* チャーンやコンバージョンなど、顧客の傾向スコアを使用してリアルタイム顧客プロファイルを強化する
* 傾向スコアに影響を及ぼす要因を使用して顧客プロファイルを強化する
* 影響を及ぼす要因と傾向スコアに基づいて顧客のセグメントを作成する

顧客 AI は、**[!UICONTROL アドビサービス]**&#x200B;の下の「**[!UICONTROL サービス]**」タブにあります。

![顧客 AI の場所](../assets/overview/rtcdp-customer-ai.png)

### 顧客 AI の基本を学ぶ

顧客 AI を使い始めるには、[データ準備チュートリアル](../../intelligent-services/data-preparation.md)に従い、ユースケースに基づいて入力スキーマを設定する必要があります。次に、[顧客 AI インスタンスを設定](../../intelligent-services/customer-ai/user-guide/configure.md)する必要があります。インスタンスを設定すると、[インサイトとスコアを表示](../../intelligent-services/customer-ai/user-guide/discover-insights.md)できるモデルが生成されます。モデルから生成されたデータを使用して、データ駆動型アクティベーション用のセグメントを作成できます。

顧客 AI について詳しくは、[顧客 AI の概要](../../intelligent-services/customer-ai/overview.md)を参照してください。さらに、次のビデオでは、顧客 AI が AI ベースの傾向で顧客プロファイルを充実させ、顧客のセグメント化とターゲティングの取り組みを強化する方法を示しています。

>[!VIDEO](https://video.tv.adobe.com/v/40374/?quality=12&learn=on)


## 次の手順

この概要を読むと、Real-Time CDP が [!DNL Segmentation Service] を利用してマーケティングキャンペーンのカスタマイズとパーソナライゼーションを強化する方法を理解できるはずです。[!DNL Segmentation Service] について詳しくは、[セグメント化に関するドキュメント](../../segmentation/home.md)を参照してください。

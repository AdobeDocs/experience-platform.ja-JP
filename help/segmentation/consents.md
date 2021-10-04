---
keywords: Experience Platform；ホーム；人気のあるトピック；オプトアウト；セグメント化；セグメント化サービス；セグメント化サービス；オプトアウト；オプトアウト；オプトアウト；オプトアウト；同意；共有；収集；
solution: Experience Platform
title: セグメントでの同意の遵守
topic-legacy: overview
description: 個人データの収集とセグメント操作での共有に関する顧客の同意設定を遵守する方法を説明します。
exl-id: fe851ce3-60db-4984-a73c-f9c5964bfbad
source-git-commit: bd312024a1a3fb6da840a38d6e9d19fcbd6eab5a
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 0%

---

# セグメントでの同意の遵守

[!DNL California Consumer Privacy Act](CCPA) などの法的プライバシー規制により、消費者は、自分の個人データを収集したり、サードパーティと共有したりすることをオプトアウトする権利が得られます。 Adobe Experience Platformには、これらの顧客の同意に関する設定をリアルタイム顧客プロファイルデータに取り込むための、標準的な Experience Data Model(XDM) コンポーネントが用意されています。

顧客が個人データの共有に関する同意を取り下げた場合、または拒否した場合は、マーケティング活動のオーディエンスを生成する際に、組織がその優先順位に従うことが重要です。 このドキュメントでは、顧客ユーザーインターフェイスを使用して、セグメント定義にExperience Platformの同意値を統合する方法について説明します。

## はじめに

顧客の同意値を遵守するには、関連する様々な [!DNL Adobe Experience Platform] サービスに関する理解が必要です。 このチュートリアルを開始する前に、次のサービスに関する十分な知識があることを確認してください。

* [[!DNL Experience Data Model (XDM)]](../xdm/home.md):Platform が顧客体験データを整理する際に使用する標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [[!DNL Adobe Experience Platform Segmentation Service]](./home.md):データからオーディエンスセグメントを作成 [!DNL Real-time Customer Profile] できます。

## 同意スキーマのフィールド

顧客の同意と環境設定を尊重するために、[!UICONTROL XDM 個人プロファイル ] の和集合スキーマの 1 つに、標準のフィールドグループ **[!UICONTROL 同意と環境設定]** が含まれている必要があります。

フィールドグループが提供する各属性の構造と使用例について詳しくは、[ 同意と環境設定のリファレンスガイド ](../xdm/field-groups/profile/consents.md) を参照してください。 フィールドグループをスキーマに追加する手順については、[XDM UI ガイド ](../xdm/ui/resources/schemas.md#add-field-groups) を参照してください。

フィールドグループを [ プロファイル対応スキーマ ](../xdm/ui/resources/schemas.md#profile) に追加し、そのフィールドを使用してエクスペリエンスアプリケーションから同意データを取り込んだら、収集した同意属性をセグメントルールで使用できます。

## セグメント化での同意の処理

オプトアウトされたプロファイルをセグメントに含めないようにするには、既存のセグメントに特別なフィールドを追加し、新しいセグメントを作成する際に含める必要があります。

以下の手順は、2 種類のオプトアウトフラグに適したフィールドを追加する方法を示しています。

1. [!UICONTROL データ収集]
1. [!UICONTROL データの共有]

>[!NOTE]
>
>このガイドでは、上記 2 つのオプトアウトフラグに焦点を当てていますが、追加の同意シグナルを組み込むようにセグメントを設定することもできます。 [ 同意と環境設定のリファレンスガイド ](../xdm/field-groups/profile/consents.md) では、これらの各オプションとその用途に関する詳細を説明しています。

UI でセグメントを作成する際、「**[!UICONTROL 属性]**」で「**[!UICONTROL XDM 個人プロファイル]**」に移動し、「**[!UICONTROL 同意と環境設定]**」を選択します。 ここから、**[!UICONTROL データ収集]** と **[!UICONTROL データ共有]** のオプションを確認できます。

![](./images/opt-outs/consents.png)

まず、「**[!UICONTROL データ収集]**」カテゴリを選択し、「**[!UICONTROL 選択値]**」をセグメントビルダーにドラッグします。 属性をセグメントに追加する際に、含めるまたは除外する必要がある [ 同意値 ](../xdm/field-groups/profile/consents.md#choice-values) を指定できます。

![](./images/opt-outs/consent-values.png)

1 つ目のアプローチは、データの収集をオプトアウトした顧客を除外することです。 これをおこなうには、演算子を **[!UICONTROL 等しくない]** に設定し、次の値を選択します。

* **[!UICONTROL いいえ（オプトアウト）]**
* **[!UICONTROL デフォルトの「いいえ」（オプトアウト）]**
* **[!UICONTROL 不明]** （不明な場合に同意が留保されると見なされる場合）

![](./images/opt-outs/collect.png)

左側のレールの **[!UICONTROL 属性]** で、**[!UICONTROL 同意と環境設定]** セクションに戻り、**[!UICONTROL データを共有]** を選択します。 対応する **[!UICONTROL 選択値]** をキャンバスにドラッグし、[!UICONTROL  データ収集 ] 選択値と同じ値を選択します。 2 つの属性の間に **[!UICONTROL Or]** の関係が確立されていることを確認します。

![](./images/opt-outs/share.png)

**[!UICONTROL データ収集]** と **[!UICONTROL データ共有]** の両方の同意値がセグメントに追加されると、データの使用をオプトアウトした顧客は、結果として得られるオーディエンスから除外されます。 ここから、「**[!UICONTROL 保存]**」を選択してプロセスを終了する前に、セグメント定義のカスタマイズを続行できます。

## 次の手順

このチュートリアルに従うと、Experience Platformでセグメントを作成する際に、顧客の同意と環境設定を尊重する方法をより深く理解できます。

Platform での同意の管理について詳しくは、次のドキュメントを参照してください。

* [Adobe標準を使用した同意処理](../landing/governance-privacy-security/consent/adobe/overview.md)
* [IAB TCF 2.0 標準を使用した同意処理](../landing/governance-privacy-security/consent/iab/overview.md)
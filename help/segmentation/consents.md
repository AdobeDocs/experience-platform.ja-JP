---
keywords: Experience Platform；ホーム；人気のあるトピック；オプトアウト；セグメント化；セグメント化サービス；セグメント化サービス；オプトアウトの遵守；オプトアウト；オプトアウト；オプトアウト；同意；共有；収集；
solution: Experience Platform
title: セグメントでの同意の遵守
topic-legacy: overview
description: セグメント操作での個人データの収集と共有に関する顧客の同意設定を遵守する方法を説明します。
exl-id: fe851ce3-60db-4984-a73c-f9c5964bfbad
source-git-commit: 6d11a94d45b4a089ca6960aaf1ce78ae654ebc3f
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# セグメントでの同意の遵守

[!DNL California Consumer Privacy Act](CCPA)などの法的プライバシー規制により、消費者は、個人データの収集やサードパーティとの共有をオプトアウトする権利を得ることができます。 Adobe Experience Platformは、これらの顧客の同意に関する設定をリアルタイム顧客プロファイルデータで取り込むための、標準のエクスペリエンスデータモデル(XDM)コンポーネントを提供します。

顧客が個人データの共有に関する同意を撤回または拒否した場合、マーケティング活動のオーディエンスを生成する際に、組織がその優先度に従うことが重要です。 このドキュメントでは、顧客ユーザーインターフェイスを使用して、セグメント定義に顧客の同意値を統合するExperience Platformを説明します。

## 概要

顧客の同意値を守るには、関連する様々な[!DNL Adobe Experience Platform]サービスに関する理解が必要です。 このチュートリアルを開始する前に、次のサービスに関する詳細を必ず理解しておく必要があります。

* [[!DNL Experience Data Model (XDM)]](../xdm/home.md):Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
* [[!DNL Real-time Customer Profile]](../profile/home.md):複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [[!DNL Adobe Experience Platform Segmentation Service]](./home.md):データからオーディエンスセグメントを作成 [!DNL Real-time Customer Profile] できます。

## 同意スキーマのフィールド

顧客の同意と環境設定を守るために、[!UICONTROL XDM個人プロファイル]の和集合スキーマに含まれるスキーマの1つに、標準フィールドグループ&#x200B;**[!UICONTROL Privacy/Personalization/Marketing Preferences (Consents)]**&#x200B;が含まれている必要があります。

フィールドグループが提供する各属性の構造と使用例について詳しくは、[同意と環境設定のリファレンスガイド](../xdm/field-groups/profile/consents.md)を参照してください。 フィールドグループをスキーマに追加する手順については、『[XDM UIガイド](../xdm/ui/resources/schemas.md#add-field-groups)』を参照してください。

フィールドグループを[プロファイル対応スキーマ](../xdm/ui/resources/schemas.md#profile)に追加し、そのフィールドを使用してエクスペリエンスアプリケーションから同意データを取り込んだら、収集した同意属性をセグメントルールで使用できます。

## セグメントでの同意の処理

オプトアウトされたプロファイルをセグメントに含めないようにするには、新しいセグメントを作成する際に、既存のセグメントに特別なフィールドを追加し、含める必要があります。

以下の手順は、2種類のオプトアウトフラグに適したフィールドを追加する方法を示しています。

1. [!UICONTROL データ収集]
1. [!UICONTROL データを共有]

>[!NOTE]
>
>このガイドでは、上記2つのオプトアウトフラグに焦点を当てていますが、追加の同意シグナルを組み込むようにセグメントを設定することもできます。 これらの各オプションとその用途に関する詳細については、[同意と環境設定リファレンスガイド](../xdm/field-groups/profile/consents.md)を参照してください。

UIでセグメントを作成する際に、「**[!UICONTROL 属性]**」の下で「**[!UICONTROL XDM個人プロファイル]**」に移動し、「**[!UICONTROL 同意と環境設定]**」を選択します。 ここから、**[!UICONTROL データ収集]**&#x200B;と&#x200B;**[!UICONTROL データ共有]**&#x200B;のオプションを確認できます。

![](./images/opt-outs/consents.png)

まず、「**[!UICONTROL データ収集]**」カテゴリを選択し、**[!UICONTROL 選択値]**&#x200B;をセグメントビルダーにドラッグします。 属性をセグメントに追加する際に、含めるまたは除外する必要がある[同意の値](../xdm/field-groups/profile/consents.md#choice-values)を指定できます。

![](./images/opt-outs/consent-values.png)

1つのアプローチは、データの収集をオプトアウトした顧客を除外することです。 これをおこなうには、演算子「**[!UICONTROL 等しくない]**」をに設定し、次の値を選択します。

* **[!UICONTROL いいえ（オプトアウト）]**
* **[!UICONTROL デフォルトの「いいえ」（オプトアウト）]**
* **[!UICONTROL 不明]** （不明な場合に同意が留保されると見なされる場合）

![](./images/opt-outs/collect.png)

左側のレールの&#x200B;**[!UICONTROL 属性]**&#x200B;で、**[!UICONTROL 同意と環境設定]**&#x200B;セクションに戻り、「**[!UICONTROL データを共有]**」を選択します。 対応する&#x200B;**[!UICONTROL 選択値]**&#x200B;をキャンバスにドラッグし、[!UICONTROL データ収集]選択値と同じ値を選択します。 2つの属性の間に&#x200B;**[!UICONTROL Or]**&#x200B;の関係が確立されていることを確認します。

![](./images/opt-outs/share.png)

**[!UICONTROL データ収集]**&#x200B;と&#x200B;**[!UICONTROL 共有データ]**&#x200B;の両方の同意値がセグメントに追加されると、データの使用をオプトアウトした顧客は、結果として得られるオーディエンスから除外されます。 ここから、「**[!UICONTROL 保存]**」を選択する前に、セグメント定義のカスタマイズを続行できます。

## 次の手順

このチュートリアルに従うと、Experience Platformでセグメントを作成する際に、顧客の同意と環境設定を遵守する方法に関する理解が深まります。

Platformでの同意の管理について詳しくは、次のドキュメントを参照してください。

* [Adobe標準を使用した同意処理](../landing/governance-privacy-security/consent/adobe/overview.md)
* [IAB TCF 2.0標準を使用した同意処理](../landing/governance-privacy-security/consent/iab/overview.md)
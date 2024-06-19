---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
description: Adobe 2.0 標準を使用して、Adobe Experience Platformで顧客の同意シグナルを処理する方法を説明します。
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
source-git-commit: b08c6cf12a38f79e019544dea91913a77bd6490a
workflow-type: tm+mt
source-wordcount: '1557'
ht-degree: 1%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformを使用すると、顧客から収集した同意データを処理し、保存されている顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客についてデータ収集が行われるのか、特定の目的のために顧客のプロファイルが使用されるのかを判断できます。 例えば、特定のプロファイルの同意データによって、書き出されたオーディエンスセグメントにデータを含めることができるかどうか、または特定のマーケティングチャネル（メール、テキストメッセージ、プッシュ通知など）にデータを参加できるかどうかを決定できます。

このドキュメントでは、Platform データ操作を設定して、同意管理プラットフォーム（CMP）によって生成された顧客同意データを取り込み、そのデータをダウンストリームのユースケース用の顧客プロファイルに統合する方法の概要を説明します。

>[!NOTE]
>
>このドキュメントでは、Adobe標準を使用した同意データの処理に焦点を当てています。 IAB Transparency and Consent Framework （TCF） 2.0 に従って同意データを処理する場合は、に関するガイドを参照してください。 [Adobe Real-time Customer Data Platformでの TCF 2.0 のサポート](../iab/overview.md).

## 前提条件

このガイドでは、同意データの処理に関わる様々なExperience Platformサービスについて、実際に理解している必要があります。

* [Experience Data Model（XDM）](/help/xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform ID サービス](/help/identity-service/home.md)：デバイスやシステムをまたいで ID を結び付けることで、顧客体験のフラグメント化によって発生する基本的な課題を解決します。
* [リアルタイム顧客プロファイル](/help/profile/home.md)：使用 [!DNL Identity Service] データセットから詳細な顧客プロファイルをリアルタイムで作成する機能。 リアルタイム顧客プロファイルは、データレイクからデータを取り込み、顧客プロファイルを独立したデータストアに保持します。
* [Adobe Experience Platform Web SDK](/help/web-sdk/home.md)：様々な Platform サービスを顧客向け web サイトに統合できるクライアントサイド JavaScript ライブラリ。
   * [SDK 同意コマンド](../../../../web-sdk/commands/setconsent.md)：このガイドに示される同意関連の SDK コマンドのユースケースの概要。
* [Adobe Experience Platform セグメント化サービス](/help/segmentation/home.md)：を使用すると、リアルタイム顧客プロファイルデータを、類似の特性を共有し、マーケティング戦略と同様に応答する個人のグループに分割できます。

## 同意処理フローの概要 {#summary}

次に、システムが適切に設定された後に同意データが処理される方法について説明します。

1. 顧客は、web サイト上のダイアログを通じて、データ収集に対する同意環境設定を指定します。
1. ページの読み込み（または CMP が同意環境設定の変更を検出した場合）ごとに、サイト上のカスタムスクリプトが現在の環境設定を標準 XDM オブジェクトにマッピングします。 その後、このオブジェクトは Platform Web SDK に渡されます `setConsent` コマンド。
1. 条件 `setConsent` を呼び出すと、Platform Web SDK は同意値が最後に受信した値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合は、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/環境設定データはに取り込まれます [!DNL Profile]スキーマに同意/環境設定フィールドが含まれている対応データセット。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、に直接アップロードされる、お客様が生成した XDM データを介してExperience Platformへと送ることもできます [!DNL Profile]が有効なデータセット。

### 同意の適用

Platform での同意処理のサポートの現在のリリースでは、データ収集権限のみ（`collect.val`）は、Platform Web SDK によって自動的に適用されます。 顧客プロファイルで同意および環境設定をよりきめ細かく収集して保持できますが、これらの追加のシグナルは、独自のダウンストリームプロセスで手動で適用する必要があります。

>[!NOTE]
>
>上記の XDM 同意フィールドの構造について詳しくは、のガイドを参照してください [[!UICONTROL 同意および環境設定] データタイプ](/help/xdm/data-types/consents.md).

システムが設定されると、Platform Web SDK は現在のユーザーのデータ収集の同意値を解釈し、データがAdobe Experience Platform Edge Networkに送信されるか、クライアントからドロップされるか、データ収集権限が yes または no に設定されるまで保持される必要があるかどうかを判断します。

## CMP 内で顧客同意データを生成する方法の決定 {#consent-data}

各 CMP システムは一意なので、顧客がサービスとやり取りする際に同意を提供できる最適な方法を決定する必要があります。 これを行う一般的な方法は、次の例のような cookie 同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客がデータの特定のマーケティングおよびパーソナライゼーションのユースケースをオプトインまたはオプトアウトできるようにする必要があります。 これらの同意と環境設定は、用に定義したデータモデルに準拠している必要があります [!DNL Profile]次の手順では、が有効になっているデータセットです。

## に標準化された同意フィールドを追加 [!DNL Profile]が有効なデータセット {#dataset}

顧客の同意データはに送信する必要があります [!DNL Profile]スキーマに同意フィールドが含まれている対応データセット。 これらのフィールドは、個々の顧客に関する属性情報の取得に使用するのと同じスキーマおよびデータセットに含める必要があります。

のチュートリアルを参照してください。 [同意データを取得するためのデータセットの設定](./dataset.md) これらの必須フィールドをに追加する方法に関する詳細な手順については、を参照してください。 [!DNL Profile]このガイドを続行する前に、で有効化されたデータセットを使用してください。

## 更新 [!DNL Profile] 同意データを含める結合ポリシー {#merge-policies}

を作成したら [!DNL Profile]同意データを処理するための – 対応データセットの場合は、各顧客プロファイルに常に同意フィールドを含めるように結合ポリシーが設定されていることを確認する必要があります。 これには、競合する可能性がある他のデータセットよりも同意データセットが優先されるように、データセットの優先順位を設定することが含まれます。

>[!NOTE]
>
>競合するデータセットがない場合は、代わりに結合ポリシーのタイムスタンプの優先順位を設定する必要があります。 これにより、顧客から指定された最新の同意が使用される同意設定になります。

結合ポリシーの使用方法について詳しくは、まず次を参照してください [結合ポリシーの概要](../../../../profile/merge-policies/overview.md). 結合ポリシーを設定する場合は、プロファイルに、が提供する必要なすべての同意属性が含まれていることを確認する必要があります [!UICONTROL 同意および環境設定] スキーマフィールドグループ（『 』ガイドで説明されているもの） [データセットの準備](./dataset.md).

## 同意データを Platform に取り込む

データセットと結合ポリシーを用意して、顧客プロファイルで必要な同意フィールドを表したら、次の手順で、同意データ自体を Platform に取り込みます。

第一に、CMP が同意変更イベントを検出した場合は常に、Adobe Experience Platform Web SDK を使用して同意データを Platform に送信する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience Platform Mobile SDK を使用する必要があります。 また、収集した同意データを、同意データセットの XDM スキーマにマッピングし、バッチ取り込みを通じて Platform に送信することで、直接取り込むことを選択することもできます。

これらの各方法の詳細については、以下のサブセクションで説明します。

### 同意データを処理するためのExperience PlatformWeb SDK の設定 {#web-sdk}

Web サイトで同意変更イベントをリッスンするように CMP を設定したら、更新された同意設定を受け取り、ページの読み込みのたびに、および同意変更イベントが発生するたびに Platform に送信するようにExperience Platform Web SDK を統合できます。 のガイドを参照してください [顧客同意データを処理するための Web SDK の設定](../sdk.md) を参照してください。

### 同意データを処理するためのExperience PlatformMobile SDK の設定 {#mobile-sdk}

お使いのモバイルアプリケーションで顧客の同意環境設定が必要な場合は、Experience PlatformMobile SDK を統合して、同意設定を取得および更新し、同意 API が呼び出されるたびに同意を Platform に送信できます。

以下の Mobile SDK ドキュメントを参照してください [同意モバイル拡張機能の設定](https://developer.adobe.com/client-sdks/documentation/consent-for-edge-network/) および [同意 API の使用](https://developer.adobe.com/client-sdks/documentation/consent-for-edge-network/api-reference/). Mobile SDK を使用してプライバシーに関する懸念を処理する方法の詳細については、 [プライバシーと GDPR](https://developer.adobe.com/client-sdks/resources/privacy-and-gdpr/).

### XDM 準拠の同意データの直接取り込み {#batch}

バッチ取り込みを使用して、XDM 準拠の同意データを CSV ファイルから取り込むことができます。 これは、まだ顧客プロファイルに統合されていない、以前に収集した同意データのバックログがある場合に役立ちます。

のチュートリアルに従います [csv ファイルから XDM へのマッピング](../../../../ingestion/tutorials/map-csv/overview.md) データフィールドを XDM に変換し、Platform に取り込む方法を説明します。 を選択した場合 [!UICONTROL 宛先] マッピングの場合、必ずを選択します **[!UICONTROL 既存のデータセットを使用]** 「」オプションを選択し、 [!DNL Profile]先ほど作成した同意データセットを有効にしました。

## 実装のテスト {#test-implementation}

顧客同意データをに取り込んだ後 [!DNL Profile]対応データセットを使用すると、更新されたプロファイルに同意属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UI で既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられた少なくとも 1 つの ID 値（および対応する名前空間）を把握している必要があります。
>
>この情報にアクセスできない場合は、独自のテスト同意データを取り込み、代わりに、既知の ID 値/名前空間に関連付けることができます。

の節を参照してください。 [id 別プロファイルの参照](../../../../profile/ui/user-guide.md#browse) が含まれる [!DNL Profile] プロファイルの詳細を検索する方法に関する特定の手順の UI ガイド。

新しい同意属性は、デフォルトではプロファイルのダッシュボードには表示されません。 したがって、に移動する必要があります **[!UICONTROL 属性]** プロファイルの詳細ページでタブをクリックし、期待どおりに取り込まれていることを確認します。 のガイドを参照してください [プロファイルダッシュボード](../../../../profile/ui/profile-dashboard.md) ニーズに合わせてダッシュボードをカスタマイズする方法を説明します。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理し、それらの属性を顧客プロファイルで表すように Platform を設定する方法について説明しました。 セグメントの選定やその他のダウンストリームのユースケースの決定要因として、顧客同意環境設定を統合できるようになりました。

Experience Platformのプライバシー関連の機能について詳しくは、の概要を参照してください [platform におけるガバナンス、プライバシー、セキュリティ](../../overview.md).

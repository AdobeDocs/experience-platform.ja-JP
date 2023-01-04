---
keywords: Experience Platform;ホーム;人気のトピック
solution: Experience Platform
title: Adobe Experience Platformでの同意処理
topic-legacy: getting started
description: Adobe2.0 標準を使用して、Adobe Experience Platformで顧客の同意シグナルを処理する方法を説明します。
exl-id: cd76a3f6-ae55-4d75-9b30-900fadb4664f
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '1567'
ht-degree: 1%

---

# Adobe Experience Platformでの同意処理

Adobe Experience Platformを使用すると、顧客から収集した同意データを処理し、保存した顧客プロファイルに統合できます。 その後、このデータをダウンストリームプロセスで使用して、特定の顧客に対してデータ収集をおこなうかどうか、またはそのプロファイルを特定の目的で使用するかどうかを判断できます。 例えば、特定のプロファイルの同意データは、書き出されたオーディエンスセグメントに含めることができるかどうかや、電子メール、テキストメッセージ、プッシュ通知などの特定のマーケティングチャネルに参加できるかどうかを決定できます。

このドキュメントでは、同意管理プラットフォーム (CMP) によって生成された顧客の同意データを取り込み、そのデータをダウンストリームの使用例のために顧客プロファイルに統合するための Platform データ操作の設定方法の概要を説明します。

>[!NOTE]
>
>このドキュメントでは、Adobe標準を使用した同意データの処理に焦点を当てています。 IAB Transparency and Consent Framework(TCF)2.0 に準拠して同意データを処理する場合は、 [Adobe Real-time Customer Data Platformでの TCF 2.0 のサポート](../iab/overview.md).

## 前提条件

このガイドでは、同意データの処理に関わる様々なExperience Platformサービスに関する十分な知識が必要です。

* [Experience Data Model（XDM）](../../../../xdm/home.md)：Adobe Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
* [Adobe Experience Platform Identity Service](../../../../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、顧客体験のフラグメント化によって生じる基本的な課題を解決します。
* [リアルタイム顧客プロファイル](../../../../profile/home.md):使用 [!DNL Identity Service] データセットから詳細な顧客プロファイルをリアルタイムで作成する機能。 リアルタイム顧客プロファイルは、データレイクからデータを取り込み、顧客プロファイルを独自の別々のデータストアに保持します。
* [Adobe Experience Platform Web SDK](../../../../edge/home.md):様々な Platform サービスを顧客向け Web サイトに統合できる、クライアント側の JavaScript ライブラリ。
   * [SDK の同意コマンド](../../../../edge/consent/supporting-consent.md):このガイドに示す同意関連の SDK コマンドの使用例の概要です。
* [Adobe Experience Platform Segmentation Service](../../../../segmentation/home.md):リアルタイム顧客プロファイルデータを、類似した特性を共有し、マーケティング戦略と同様に対応する個人のグループに分割できます。

## 同意処理フローの概要 {#summary}

以下では、システムが正しく設定された後に同意データが処理される方法を説明します。

1. 顧客が Web サイト上のダイアログを通じて、データ収集に関する同意設定を提供します。
1. ページが読み込まれるたび（または CMP が同意設定の変更を検出した場合）、サイト上のカスタムスクリプトは現在の環境設定を標準の XDM オブジェクトにマッピングします。 このオブジェクトは、その後、Platform Web SDK に渡されます `setConsent` コマンドを使用します。
1. 条件 `setConsent` が呼び出されると、Platform Web SDK は、同意の値が最後に受け取った値と異なるかどうかを確認します。 値が異なる（または以前の値がない）場合、構造化された同意/環境設定データがAdobe Experience Platformに送信されます。
1. 同意/環境設定データは、 [!DNL Profile]スキーマに同意/環境設定フィールドが含まれる、有効なデータセット。

CMP の同意変更フックによってトリガーされる SDK コマンドに加えて、同意データは、に直接アップロードされる、お客様が生成した XDM データを通じてExperience Platformに送ることもできます。 [!DNL Profile]-enabled データセット。

### 同意の適用

Platform での同意処理のサポートの現在のリリースでは、データ収集権限 (`collect.val`) は、Platform Web SDK によって自動的に適用されます。 より詳細な同意や環境設定を収集して顧客プロファイルで保持できますが、これらの追加のシグナルは、独自のダウンストリームプロセスで手動で適用する必要があります。

>[!NOTE]
>
>上記の XDM 同意フィールドの構造について詳しくは、 [[!UICONTROL 同意および環境設定] データタイプ](../../../../xdm/data-types/consents.md).

システムが設定されると、Platform Web SDK は、現在のユーザーのデータ収集の同意値を解釈し、データをAdobe Experience Platform Edge Network に送信するか、クライアントから削除するか、データ収集権限が yes または no に設定されるまで保持するかを決定します。

## CMP 内で顧客の同意データを生成する方法を決定する {#consent-data}

各 CMP システムは固有なので、お客様がサービスとやり取りする際に同意を得る最適な方法を決定する必要があります。 これを実現する一般的な方法は、次の例のような Cookie の同意ダイアログを使用することです。

![](../../../images/governance-privacy-security/consent/adobe/overview/consent-dialog.png)

このダイアログでは、顧客がデータに対して特定のマーケティングおよびパーソナライゼーションの使用例をオプトインまたはオプトアウトできるようにする必要があります。 これらの同意および環境設定は、 [!DNL Profile]-enabled データセットを次の手順で使用します。

## への標準化された同意フィールドの追加 [!DNL Profile]有効なデータセット {#dataset}

顧客の同意データを [!DNL Profile]スキーマに同意フィールドが含まれる有効なデータセット。 これらのフィールドは、個々の顧客に関する属性情報を取り込むために使用するのと同じスキーマおよびデータセットに含める必要があります。

次のチュートリアルを参照してください。 [同意データを取得するためのデータセットの設定](./dataset.md) を参照してください。 [!DNL Profile]有効なデータセットを有効にしてから、このガイドに進んでください。

## 更新 [!DNL Profile] 同意データを含めるためのポリシーの結合 {#merge-policies}

以下を作成したら、 [!DNL Profile] — 有効な同意データセットを処理する場合は、各顧客プロファイルに同意フィールドを常に含めるように結合ポリシーが設定されていることを確認する必要があります。 これには、競合する可能性のある他のデータセットよりも同意データセットの方が優先されるように、データセットの優先順位を設定する必要があります。

>[!NOTE]
>
>競合するデータセットがない場合は、結合ポリシーのタイムスタンプの優先順位を代わりに設定する必要があります。 これにより、お客様が指定した最新の同意が、使用される同意設定になることを保証できます。

結合ポリシーの操作方法の詳細については、まず [結合ポリシーの概要](../../../../profile/merge-policies/overview.md). 結合ポリシーを設定する場合、プロファイルに、 [!UICONTROL 同意および環境設定] スキーマフィールドグループ ( [データセットの準備](./dataset.md).

## 同意データを Platform に取り込む

顧客プロファイルで必要な同意フィールドを表すデータセットと結合ポリシーを取得したら、次の手順では、同意データ自体を Platform に取り込みます。

主に、Adobe Experience Platform Web SDK を使用して、CMP で同意変更イベントが検出された場合は常に、同意データを Platform に送信する必要があります。 モバイルプラットフォームで同意データを収集する場合は、Adobe Experience Platform Mobile SDK を使用する必要があります。 また、収集した同意データを同意データセットの XDM スキーマにマッピングし、バッチ取得を通じて Platform に送信することで、収集した同意データを直接取得することもできます。

これらの各メソッドの詳細については、以下のサブセクションで説明します。

### 同意データを処理するExperience PlatformWeb SDK の設定 {#web-sdk}

Web サイト上の同意変更イベントをリッスンするように CMP を設定したら、Experience PlatformWeb SDK を統合して、更新された同意設定を受け取り、ページの読み込みのたびに、同意変更イベントが発生した場合に Platform に送信できます。 詳しくは、 [顧客の同意データを処理するための Web SDK の設定](../sdk.md) を参照してください。

### 同意データを処理するExperience PlatformMobile SDK を設定する {#mobile-sdk}

モバイルアプリケーションでExperience Platformの同意設定が必要な場合は、Consent API が呼び出されるたびに同意設定を取得および更新し、Platform に送信するように、顧客の Mobile SDK を統合できます。

詳しくは、 Mobile SDK のドキュメントを参照してください。 [同意モバイル拡張機能の設定](https://aep-sdks.gitbook.io/docs/foundation-extensions/consent-for-edge-network) および [同意 API の使用](https://aep-sdks.gitbook.io/docs/foundation-extensions/consent-for-edge-network/api-reference). Mobile SDK を使用してプライバシーに関する問題を処理する方法について詳しくは、の節を参照してください。 [プライバシーと GDPR](https://aep-sdks.gitbook.io/docs/resources/privacy-and-gdpr).

### XDM 準拠の同意データを直接取り込む {#batch}

バッチ取得を使用して、XDM 準拠の同意データを CSV ファイルから取り込むことができます。 これは、以前に収集された同意データのバックログがまだ顧客プロファイルに統合されていない場合に役立ちます。

次のチュートリアルに従います。 [CSV ファイルの XDM へのマッピング](../../../../ingestion/tutorials/map-csv/overview.md) データフィールドを XDM に変換し、Platform に取り込む方法を説明します。 選択時に、 [!UICONTROL 宛先] マッピングの場合は、必ず **[!UICONTROL 既存のデータセットを使用]** オプションを選択し、 [!DNL Profile]先ほど作成した —enabled 同意データセット。

## 実装をテストする {#test-implementation}

顧客の同意データを [!DNL Profile]有効なデータセットの場合は、更新したプロファイルに同意属性が含まれているかどうかを確認できます。

>[!IMPORTANT]
>
>UI で既存のプロファイルの属性を表示するには、そのプロファイルに関連付けられた 1 つ以上の ID 値（および対応する名前空間）を把握しておく必要があります。
>
>この情報へのアクセス権がない場合は、独自のテスト同意データを取り込み、代わりに既知の ID 値/名前空間に関連付けることができます。

詳しくは、 [ID によるプロファイルの参照](../../../../profile/ui/user-guide.md#browse) 内 [!DNL Profile] プロファイルの詳細を検索する方法に関する特定の手順については、UI ガイドを参照してください。

新しい同意属性は、デフォルトではプロファイルのダッシュボードに表示されません。 したがって、 **[!UICONTROL 属性]** 」タブをクリックして、期待どおりに取り込まれたことを確認します。 詳しくは、 [プロファイルダッシュボード](../../../../profile/ui/profile-dashboard.md) ダッシュボードをニーズに合わせてカスタマイズする方法を説明します。

<!-- (To be included once CJM is GA)
## Handling consent in Customer Journey Management

If you are using Customer Journey Management, after confirming that your profiles and segments contain consent data, you can start honoring customer [marketing preferences](../../../../xdm/data-types/consents.md#marketing) when pulling segments from Platform. Specifically, profiles who have opted out of the email marketing preference should not be included in segments that are targeted for email campaigns.

Customer Journey Management can also send consent-change signals back to Platform. When a customer selects an "unsubscribe" link in an email message, the updated consent preference is sent to Platform and the appropriate profile attributes are updated accordingly.
-->

## 次の手順

このガイドでは、Adobe標準を使用して顧客の同意データを処理するように Platform の操作を設定し、それらの属性を顧客プロファイルで表す方法について説明しました。 セグメント認定やその他のダウンストリームの使用例で、顧客の同意設定を決定要因として統合できるようになりました。

Experience Platformのプライバシー関連機能について詳しくは、 [Platform でのガバナンス、プライバシー、セキュリティ](../../overview.md).

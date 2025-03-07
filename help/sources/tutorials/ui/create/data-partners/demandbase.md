---
title: UI を使用した Demandbase Intent のExperience Platformへの接続
description: Demandbase Intent をExperience Platformに接続する方法を学ぶ
hide: true
hidefromtoc: true
source-git-commit: 81a615b9826ed69bb050cae9c074a4e457ba128a
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 28%

---

# UI を使用した [!DNL Demandbase Intent] のExperience Platformへの接続

ユーザーインターフェイスを使用して [!DNL Demandbase Intent] アカウントをAdobe Experience Platformに接続する方法については、このガイドを参照してください。

## 基本を学ぶ

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [Real-Time CDP B2B edition](../../../../../rtcdp/b2b-overview.md): Real-Time CDP B2B editionは、B2B サービスモデルで業務を行うマーケター向けに設計されています。 複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。
* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## ソースカタログのナビゲート {#navigate}

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*[!UICONTROL B2B]* カテゴリの下の「**[!DNL Demandbase Intent]**」を選択し、「**[!UICONTROL 設定]**」を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントが存在すると、このオプションは **[!UICONTROL データを追加]** に変わります。



## 既存のアカウントを使用 {#existing}

## 新しいアカウントを作成 {#create}

## データフローの詳細を入力 {#provide-dataflow-details}

>[!CONTEXTUALHELP]
>id="platform_sources_demandbase_domain"
>title="ドメインソース"
>abstract="Adobeは XDM accountOrganization.website を使用しますが、一部のお客様は各 web サイトのカスタムフィールドを使用している場合があります。 したがって、ドメインソースが、Experience Platform アカウントに対する Demandbase アカウントレコードと一致するドメイン/web サイトフィールドであることを確認する必要があります。"

## データフローのスケジュール設定 {#schedule-dataflow}

>[!CONTEXTUALHELP]
>id="platform_sources_demandbase_schedule"
>title="データフローのスケジュール設定"
>abstract="Demandbase は、週に 1 回、月曜日の朝の午後 5 時（UTC）にデータをドロップします。 そのため、取り込み開始時刻を午後 5 時（UTC）以降に設定する必要があります。 さらに、ファイルをAdobeにドロップする際にスケジュールが変更される可能性があるので、Demandbase で取り込み時間を確認する必要があります。"

## データフローのレビュー {#review-dataflow}

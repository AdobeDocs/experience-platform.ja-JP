---
description: Destination SDKで作成された宛先でサポートされるターゲット ID を設定する方法について説明します。
title: ID 名前空間の設定
source-git-commit: 118ff85a9fceb8ee81dbafe2c381d365b813da29
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 14%

---


# ID 名前空間の設定

Experience Platformは ID 名前空間を使用して特定の ID のタイプを記述します。 例えば、という ID 名前空間 `Email` 次のような値を識別します。 `name@email.com` 電子メールアドレスとして。

Destination SDKを通じて宛先を作成する場合、 [パートナースキーマの設定](schema-configuration.md) ユーザーがプロファイル属性と id をにマッピングできるように、宛先プラットフォームでサポートされる id 名前空間を定義することもできます。

この場合、ユーザーは、ターゲットプロファイル属性に加えて、ターゲット ID を選択する追加の選択肢を持ちます。

ID 名前空間のExperience Platformについて詳しくは、 [ID 名前空間ドキュメント](../../../../identity-service/namespaces.md).

宛先の ID 名前空間を設定する際に、次のように、宛先でサポートされるターゲット ID マッピングを微調整できます。

* ユーザーが XDM 属性を ID 名前空間にマッピングできるようにします。
* ユーザーによるマッピングの許可 [標準 id 名前空間](../../../../identity-service/namespaces.md#standard) を独自の id 名前空間に追加します。
* ユーザーによるマッピングの許可 [カスタム id 名前空間](../../../../identity-service/namespaces.md#manage-namespaces) を独自の id 名前空間に追加します。

Destination SDKを使用して作成された統合で、このコンポーネントがどこに適合するかを把握するには、 [設定オプション](../configuration-options.md) ドキュメントを参照するか、 [Destination SDKを使用したファイルベースの宛先の設定](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration).

サポートされる ID 名前空間は、 `/authoring/destinations` endpoint. このページに示すコンポーネントを設定できる API 呼び出しの詳細な例については、次の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての ID 名前空間設定オプションについて説明し、Platform UI で顧客に表示される内容を示します。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## サポートされる統合のタイプ {#supported-integration-types}

このページで説明する機能をサポートする統合のタイプについて詳しくは、次の表を参照してください。

| 統合タイプ | 機能をサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベース（バッチ）の統合 | ○ |

## サポートされているパラメーター {#supported-parameters}

宛先がサポートするターゲット ID を定義する際に、次の表で説明するパラメーターを使用して、その動作を設定できます。

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|---|------|
| `acceptsAttributes` | ブール値 | オプション | お客様が、設定中の ID に標準プロファイル属性をマッピングできるかどうかを示します。 |
| `acceptsCustomNamespaces` | ブール値 | オプション | お客様が、設定している ID 名前空間にカスタム ID 名前空間をマッピングできるかどうかを示します。 |
| `acceptedGlobalNamespaces` | - | オプション | どれが [標準 id 名前空間](../../../../identity-service/namespaces.md#standard) ( 例： [!UICONTROL IDFA]) のお客様は、設定中の id にマッピングできます。 |
| `transformation` | 文字列 | オプション | 次を表示： [[!UICONTROL 変換を適用]](../../../ui/activate-segment-streaming-destinations.md#apply-transformation) ソースフィールドが XDM 属性またはカスタム ID 名前空間の場合の、Platform UI のチェックボックス。 書き出し時にソース属性をハッシュ化する機能をユーザーに与えるには、このオプションを使用します。 このオプションを有効にするには、値を `sha256(lower($))`. |
| `requiredTransformation` | 文字列 | オプション | 顧客がこのソース ID 名前空間を選択すると、 [[!UICONTROL 変換を適用]](../../../ui/activate-segment-streaming-destinations.md#apply-transformation) チェックボックスはマッピングに自動的に適用され、ユーザーは無効にできません。 このオプションを有効にするには、値を `sha256(lower($))`. |

{style="table-layout:auto"}


```json
"identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
            }
         }
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   }
```

どの [!DNL Platform] ID の顧客が宛先に書き出すことができるかを示す必要があります。例として、[!DNL Experience Cloud ID]、ハッシュ化されたメール、デバイス ID（[!DNL IDFA]、[!DNL GAID]）などがあります。これらの値は、[!DNL Platform] ID 名前空間であり、宛先から顧客が ID 名前空間にマッピングできます。

ID 名前空間は、[!DNL Platform] と宛先が 1 対 1 で対応している必要はありません。
例えば、顧客は [!DNL Platform] [!DNL IDFA] 名前空間を宛先からの [!DNL IDFA] 名前空間にマッピングすることができ、また顧客は同じ [!DNL Platform] [!DNL IDFA] 名前空間を宛先の [!DNL Customer ID] 名前空間にマッピングすることもできます。

詳しくは、 [id 名前空間の概要](../../../../identity-service/namespaces.md).

## マッピングに関する考慮事項

顧客がソース ID 名前空間を選択し、ターゲットマッピングを選択しない場合、Platform は同じ名前の属性を使用してターゲットマッピングを自動的に入力します。

## オプションのソースフィールドハッシュの設定

Experience Platformのお客様は、データをハッシュ化された形式またはプレーンテキストで Platform に取り込むことを選択できます。 宛先プラットフォームがハッシュ化されたデータとハッシュ化されていないデータの両方を受け入れる場合、顧客は、宛先に書き出す際に、Platform がソースフィールド値をハッシュ化する必要があるかどうかを選択できます。

以下の設定は、オプションの [変換を適用](../../../ui/activate-segment-streaming-destinations.md#apply-transformation) 」オプションを使用してマッピング手順を実行します。

```json {line-numbers="true" highlight="5"}
"identityNamespaces":{
      "Customer_contact":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation": "sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
            },
            "Phone":{
            }
         }
      }
   }
```

ハッシュ化されていないソースフィールドを使用している場合に、このオプションを有効にすると、Adobe Experience Platform でアクティベーション時に自動的にハッシュ化されます。

ハッシュ化されていないソース属性を、宛先でハッシュ化される必要があるターゲット属性にマッピングする場合 ( 例： `email_lc_sha256` または `phone_sha256`)、 **変換を適用** アクティブ化時にAdobe Experience Platformがソース属性を自動的にハッシュ化するオプションが追加されました。

## 必須のソースフィールドハッシュの設定

宛先がハッシュ化されたデータのみを受け入れる場合、書き出された属性を Platform で自動的にハッシュ化するように設定できます。 以下の設定は、 **変換を適用** オプションを `Email` および `Phone` id はマッピングされます。

```json {line-numbers="true" highlight="8,11"}
"identityNamespaces":{
      "Customer_contact":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation": "sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
               "requiredTransformation": "sha256(lower($))"
            },
            "Phone":{
               "requiredTransformation": "sha256(lower($))"
            }
         }
      }
   }
```

## 次の手順 {#next-steps}

この記事を読んだ後、Destination SDKで構築した宛先の ID 名前空間を設定する方法について、より深く理解する必要があります。

その他の宛先コンポーネントについて詳しくは、次の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth 2 認証](oauth2-authentication.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間の設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータの設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)

---
title: _container
description: タグコンテナ全体を 1 つのオブジェクトに表示します。
source-git-commit: 6f8bdfd09023ea48962a40a9539afe017bc108cc
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 3%

---

# `_container`

`_satellite._container` オブジェクトには、ページに読み込まれたタグプロパティの完全な設定とランタイムコンテキストが含まれます。 ルール、データ要素、拡張機能、環境などのすべての情報は、このオブジェクト内で使用できます。 主な用途は、公開または公開されたロジックを正確に確認できるように実装のデバッグを支援することです。

```ts
readonly _satellite._container: {
  buildInfo: BuildInfo;
  company: Company;
  dataElements: { [name: string]: DataElement };
  environment: Environment;
  extensions: { [name: string]: Extension };
  property: Property;
  rules: Rule[];
}
```

>[!IMPORTANT]
>
>このオブジェクトはデバッグ目的でのみ使用されます。 実稼動ロジックをこのオブジェクトに関連付けたり、実装でこのオブジェクトを参照したり、このオブジェクト内の値を編集したりしないでください。 このオブジェクト内のプロパティや名前の使用は、Adobeでいつでも変更できます。

## 使用可能なフィールド

次のオブジェクトを参照できます。

```json
{
  "buildInfo": {
    "minified": true,
    "buildDate": "YYYY-06-13T01:22:12Z"
  },
  "company": {
    "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
    "dynamicCdnEnabled": true,
    "cdnAllowList": [ "assets.adobedtm.com" ]
  },
  "dataElements": { ... },
  "environment": {
    "id": "EN6b2...d6ff2",
    "stage": "production"
  },
  "extensions": {
    "adobe-alloy": { ... },
    "core": { ... },
    "common-web-sdk-plugins": { ... }
  },
  "property": {
    "name": "Example tag property",
    "settings": {
      "domains": [ "example.com" ],
      "undefinedVarsReturnEmpty": false,
      "ruleComponentSequencingEnabled": true
    },
    "id": "PR845...6cf92"
  },
  "rules": [ { ... } ]
}
```

## `buildInfo`

`_satellite._container.buildInfo` オブジェクトには、[`_satellite.buildInfo`](buildinfo.md) のコピーが含まれています。

```ts
readonly _satellite._container.buildInfo: {
  minified: boolean;
  buildDate: string;
  turbineBuildDate: string;
  turbineVersion: string;
}
```

## `company`

`_satellite._container.company` オブジェクトには、[`_satellite.company`](company.md) のコピーが含まれています。

```ts
readonly _satellite._container.company: {
  orgId: string;
  dynamicCdnEnabled: boolean;
  cdnAllowList: string[];
}
```

## `dataElements`

`_satellite._container.dataElements` オブジェクトは、タグプロパティ内のすべてのデータ要素の参照を提供します。

```ts
readonly _satellite._container.dataElements: {
  [name: string]: {
    modulePath: string;
    settings: Settings; // Varies by data element type
  }
}
```

各データ要素には、次のプロパティが含まれます。

| 名前 | タイプ | 説明 |
|---|---|---|
| **`modulePath`** | `string` | そのデータ要素型のロジックを決定するJavaScript ファイルのパス。 |
| **`settings`** | `Settings` | データ要素の設定。 このオブジェクト内のプロパティは、データ要素のタイプによって異なります。 |

## `environment`

`_satellite._container.environment` オブジェクトには、[`_satellite.environment`](environment.md) のコピーが含まれています。

```ts
readonly _satellite._container.environment: {
  id: string;
  stage: string;
}
```

## `extensions`

`_satellite._container.extensions` オブジェクトには、タグプロパティに公開されているすべての拡張機能が一覧表示されます。

```ts
readonly _satellite._container.extensions: {
  [name: string]: {
    displayName: string;
    hostedLibFilesUrl: string;
    modules: Modules; // Extension-specific module definitions
  }
}
```

各拡張機能には、次のプロパティが含まれます。

| 名前 | タイプ | 説明 |
|---|---|---|
| **`displayName`** | `string` | 拡張機能のわかりやすい名前。 |
| **`hostedLibFilesUrl`** | `string` | 拡張機能が存在する CDN 上の場所。 |
| **`modules`** | `Modules` | 拡張機能が使用するすべてのイベント、アクション、条件およびデータ要素のJavaScript ロジック。 このオブジェクトのコンテンツは、拡張機能ごとに異なります。 |

## `property`

`_satellite._container.property` オブジェクトは、タグプロパティ自体に関する情報を提供します。

```ts
readonly _satellite._container.property: {
  id: string;
  name: string;
  settings: {
    domains: string[];
    ruleComponentSequencingEnabled: boolean;
    undefinedVarsReturnEmpty: boolean;
  };
}
```

| 名前 | タイプ | 説明 |
|---|---|---|
| **`id`** | `string` | タグプロパティの一意の ID。 |
| **`name`** | `string` | タグプロパティのわかりやすい名前。 |
| **`settings`** | `PropertySettings` | このプロパティの設定。 次の表を参照してください。 |

| 設定名 | タイプ | 説明 |
|---|---|---|
| **`domains`** | `string[]` | [&#x200B; タグプロパティの設定 &#x200B;](/help/tags/ui/administration/companies-and-properties.md) 時に設定される、プロパティに設定されたドメイン。 |
| **`ruleComponentSequencingEnabled`** | `boolean` | タグプロパティを設定する際に、チェックボックス **[!UICONTROL Run rule components in sequence]** を有効にするかどうかを決定します。 |
| **`undefinedVarsReturnEmpty`** | `boolean` | タグプロパティを設定する際に、チェックボックス **[!UICONTROL Return an empty string for undefined data elements]** を有効にするかどうかを決定します。 |

## `_container.rules`

`rules` オブジェクト配列は、タグプロパティ内のすべてのルールの参照を提供します。

```ts
readonly _satellite._container.rules: {
  id: string;
  name: string;
  events: Event[]; // Rule-specific events
  conditions: Condition[]; // Rule-specific conditions
  actions: Action[]; // Rule-specific actions
}[]
```

各ルールには、次のフィールドが含まれます。

| 名前 | タイプ | 説明 |
|---|---|---|
| **`id`** | `string` | ルールの一意の ID。 |
| **`name`** | `string` | ルールのわかりやすい名前。 |
| **`events`** | `Event[]` | ルールのトリガーとして設定したイベントの配列。 |
| **`conditions`** | `Condition[]` | ルールのトリガーとして設定した条件の配列。 |
| **`actions`** | `Action[]` | ルールがトリガーされたときに実行するように設定したアクションの配列。 |

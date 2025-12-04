---
title: cookie
description: タグプロパティの Cookie を手動で書き込み、編集または削除する。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 7%

---

# `cookie`

`_satellite.cookie` オブジェクトには、タグルールが参照できる cookie の書き込み、編集、削除を可能にするメソッドが含まれています。 これは、そのライブラリの多くのコア機能を含んだ、[`js-cookie`](https://github.com/js-cookie/js-cookie) の部分的なコピーです。

## `cookie.set()`

`set()` メソッドは、タグプロパティが参照できる cookie を書き込みます。

```ts
_satellite.cookie.set(name: string, value: string, attributes?: {
  expires?: number | Date;
  path?: string;
  domain?: string;
  secure?: boolean;
  sameSite?: 'Strict' | 'Lax' | 'None';
}): string
```

次のメソッドパラメーターを使用できます。

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| **`name`** | `string` | ○ | cookie の名前。 |
| **`value`** | `string` | ○ | cookie の値 |
| **`attributes`** | `object` | × | cookie に設定する追加の属性。 |

`attributes` オブジェクトは、次のプロパティをサポートしています。

| 属性名 | タイプ | 必須 | デフォルト | 説明 |
|---|---|---|---|---|
| **`expires`** | `number` 以 [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | × | Cookie はブラウザーセッションの終了時に有効期限が切れます | cookie の有効期限を設定する日数。 |
| **`path`** | `string` | × | サイト全体で cookie が表示される | ドメイン上で cookie が表示される場所。 |
| **`domain`** | `string` | × | 作成されたドメインで表示可能な Cookie | Cookie が表示される有効なドメイン（サブドメインの有無は問わない）。 サブドメインを使用せずにドメインを含めた場合、Cookie はそのドメインの下のすべてのサブドメインに表示されます。 |
| **`secure`** | `boolean` | × | 属性が設定されていません | Cookie がセキュアプロトコル（`https://`）を必要とするかどうかを決定します。 省略した場合、セキュアプロトコルの要件はありません。 |
| **`sameSite`** | `'Strict' \| 'Lax' \| 'None'` | × | 属性が設定されていません | cookie の [`sameSite`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value) 属性を設定できます。 `sameSite` を `None` に設定する場合は、`secure` も `true` に設定する必要があります。 |

```js
// Sets a cookie valid across the entire site, expires on session close
_satellite.cookie.set('simple_session_cookie', 'value');

// Sets a cookie that expires 7 days from now, valid across the entire site
_satellite.cookie.set('seven_day_cookie', 'value', { expires: 7 });

// Sets a cookie that expires 14 days from now, valid only on the current page
_satellite.cookie.set('page_specific_cookie', 'value', { expires: 14, path: '/' });

// Cross-site compatible cookie (requires HTTPS)
_satellite.cookie.set('cross_site_cookie', 'value', { secure: true, sameSite: 'None' });
```

このメソッドを呼び出すと、目的の Cookie が書き込まれ、書き込まれたシリアル化された Cookie 文字列が返されます。 この文字列は主にデバッグやログの目的で使用されます。

```text
'written_cookie=value; path=/'
```

>[!TIP]
>
>`_satellite.setCookie()` で使用されていたタグオブジェクトの以前のバージョン。 `setCookie()` メソッドは廃止され、`_satellite.cookie.set()` に置き換わりました。

## `cookie.get()`

`get()` メソッドは cookie の値を返します。

```ts
_satellite.cookie.get(name: string): string | undefined;
```

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| **`name`** | `string` | ○ | 値の取得元の cookie 名（大文字と小文字を区別）。 |

cookie 名が存在する場合、は cookie の値を返します。 cookie 名が存在しない場合は、`undefined` を返します。

>[!TIP]
>
>`_satellite.getCookie()` で使用されていたタグオブジェクトの以前のバージョン。 `getCookie()` メソッドは廃止され、`_satellite.cookie.get()` に置き換わりました。

## `cookie.remove()`

`remove()` メソッドは、ユーザーが設定した Cookie を削除します。

```ts
_satellite.cookie.remove(name: string, attributes?: {
  path?: string;
  domain?: string;
}): void
```

| 名前 | タイプ | 必須 | 説明 |
|---|---|---|---|
| **`name`** | `string` | ○ | 削除する cookie 名。 |
| **`attributes`** | `object` | × | 削除する cookie に関連付けられた属性。 `path` 属性または `domain` 属性を使用して cookie を設定する場合は、cookie を削除する際に同じ属性を含めます。 これらの属性を含めなかった場合でも、Cookie は削除されません。 |

```js
// Creates a session cookie
_satellite.cookie.set('session_cookie', 'Cookie value');

// Removes the above cookie
_satellite.cookie.remove('session_cookie');

// Creates a cookie that is only visible on the current page
_satellite.cookie.set('page_specific_cookie', 'value', { path: '/' });

// This remove method does nothing because it does not match the path and domain attributes of the cookie set
_satellite.cookie.remove('page_specific_cookie');

// This remove method works correctly for the page-specific cookie
_satellite.cookie.remove('page_specific_cookie', { path: '/' });
```

>[!TIP]
>
>`_satellite.removeCookie()` で使用されていたタグオブジェクトの以前のバージョン。 `removeCookie()` メソッドは廃止され、`_satellite.cookie.remove()` に置き換わりました。

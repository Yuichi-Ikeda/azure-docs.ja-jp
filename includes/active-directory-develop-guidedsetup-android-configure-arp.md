---
title: インクルード ファイル
description: インクルード ファイル
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 0154aac14168c9d897698a15e31b3124b208db46
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142765"
---
## <a name="add-the-applications-registration-to-your-code"></a>アプリケーションの登録をコードに追加する

この手順では、アプリケーション/クライアント ID をプロジェクトに追加する必要があります。

1. `MainActivity` を開きます ([`app`]  >  [`java`]  >  [*`{host}.{namespace}`*] の順)。
2. `final static String CLIENT_ID` で始まる行を以下に置き換えます。
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. [`app`]  >  [`manifests`] \(マニフェスト)  >  [`AndroidManifest.xml`] の順に開きます。
4. `manifest\application` に次のアクティビティを追加します。 `BrowserTabActivity` は、認証の完了後に Microsoft がアプリケーションにコールバックできるようにします。

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

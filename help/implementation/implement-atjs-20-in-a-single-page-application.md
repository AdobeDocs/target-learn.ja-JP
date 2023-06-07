---
title: シングルページアプリケーション (SPA) で at.js 2.0 を実装する方法
description: Adobe Targetの at.js 2.0 は、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行するための機能セットを提供します。 シングルページアプリケーション (SPA) に at.js 2.0 を実装するには、次の手順に従います。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 955f0571-5791-4dbb-9931-e6d5c8bb42a7
source-git-commit: 80208b3ecbc0d627d2afe72f882e91c9800d2726
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 2%

---

# シングルページアプリケーション (SPA) での at.js 2.0 を使用したAdobe Targetの実装

Adobe Target `at.js` 2.0 は、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行するための機能セットを提供します。 このバージョンはアップグレードに重点を置いています `at.js` を使用すれば、シングルページアプリケーション (SPA) と調和したインタラクションを実現できます。

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## SPAで at.js 2.0 を実装する方法

* 実装方法 `at.js` 2.0 の &lt;head> を使用します。
* の実装 `adobe.target.triggerView()` 関数は、SPAで表示が変更された場合に常に機能します。 これをおこなうには、URL ハッシュの変更のリッスン、SPAで実行されるカスタムイベントのリッスン、 `triggerView()` コードをアプリケーションに直接組み込みます。 特定の単一ページアプリケーションに最適なオプションを選択する必要があります。
* ビュー名は、 `triggerView()` 関数に置き換えます。 シンプルで明確で一意の名前を使用して、Target の Visual Experience Composer で簡単に選択できるようにします。
* 小さなビューの変更でトリガービューを表示できます。また、SPA以外のコンテキスト（無限にスクロールするページの半分下など）でも表示できます。
* `at.js` 2.0 および `triggerView()` は、Adobe Experience Platform Launchなどのタグ管理ソリューションを介して実装できます。

## at.js 2.0 の制限

次の制限事項に注意してください： `at.js` 2.0 （アップグレード前）:

* クロスドメイントラッキングは、 `at.js` 2.0
* mboxOverride.browserIp および mboxSession URL パラメーターは、 `at.js` 2.0
* レガシー関数 mboxCreate、mboxDefine、mboxUpdate は、 `at.js` 2.0。デフォルトコンテンツが表示され、ネットワークリクエストはおこなわれません。

## ビデオで使用されるライブラリのフッターコード

以下のコードは、 `at.js` ライブラリをビデオ中に追加しました。 アプリが最初に読み込まれ、次にアプリ内のハッシュの変更時に実行されます。 クリーンアップバージョンのハッシュがビュー名として使用され、ハッシュが空の場合は「home」が使用されます。 SPAを識別するために、コードは URL 内で「react/」というテキストを探します。このテキストは、サイト上で更新する必要がある可能性が高くなります。 また、SPAで `triggerView()` カスタムイベントが表示されない、またはコードを直接アプリに埋め込むことによって削除されます。

```javascript
function sanitizeViewName(viewName) {
  if (viewName.startsWith('#')) {
    viewName = viewName.substr(1);
  }
  if (viewName.startsWith('/')) {
    viewName = viewName.substr(1);
  }
  return viewName;
}
function triggerView(viewName) {
  viewName = sanitizeViewName(viewName) || 'home';
  // Validate if the Target Libraries are available on your website
  if (typeof adobe != 'undefined' && adobe.target && typeof adobe.target.triggerView === 'function') {
    adobe.target.triggerView(viewName);
    console.log('AT: View triggered on page load: '+viewName)
  }
}
//fire triggerView when the SPA loads and when the hash changes in the SPA
if(window.location.pathname.indexOf('react/') >-1){
    triggerView(location.hash);
}
window.onhashchange = function() {
    if(window.location.pathname.indexOf('react/') >-1){
        triggerView(location.hash);
    }
}
```

## その他のリソース

* [at.js 2.0 の仕組みについて（アーキテクチャ図）](understanding-how-atjs-20-works.md)
* [シングルページアプリケーション (SPA VEC) でのAdobe Targetの Visual Experience Composer の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)

---
title: Adobe Targetのat.js 2.0をシングルページアプリケーション(SPA)に実装する
seo-title: Adobe Targetのat.js 2.0をシングルページアプリケーション(SPA)に実装する
description: at.js の最新バージョンは、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行するための機能セットを提供します。この新しいバージョンは、シングルページアプリケーション（SPA）と調和したインタラクションを実現するための at.js のアップグレードに焦点を当てています。
audience: developer
difficulty: 3
author: Daniel Wright
doc-type: implement
activity-type: technical-video
translation-type: tm+mt
source-git-commit: 37443ae4c1cdda387c8db0053201d520fa1ec224
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 8%

---


# Adobe Targetのat.js 2.0をシングルページアプリケーション(SPA)に実装する

The newest version of `at.js` provides rich feature sets that equip your business to execute personalization on next-generation, client-side technologies. This new version is focused on upgrading `at.js` to have harmonious interactions with single page applications (SPAs).

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## SPAにat.js 2.0を実装する方法

* シングルページアプリの&lt;head>に `at.js` 2.0を実装します。
* SPAで表示が変更された場合は常に `adobe.target.triggerView()` 関数を実装します。 これを行うには、URLハッシュの変更のリッスン、SPAで呼び出されるカスタムイベントのリスニング、アプリケーションに直接コードを埋め込むなど、様々な手法を使用できます。 `triggerView()` 特定の単一ページアプリに最適なオプションを選択する必要があります。
* 表示ー名は、関数の最初のパラメーターで `triggerView()` す。 シンプルで明確な一意の名前を付けて、TargetのVisual Experience Composerで簡単に選択できるようにします。
* 表示は、小さな表示の変更時にトリガーできるほか、SPA以外のコンテキスト（無限にスクロールするページの半分下など）でもトリガーできます。
* `at.js` 2.0およびは、Adobe Experience Platform Launchなどのタグ管理ソリューションを使用して導入で `triggerView()` きます。

## at.js 2.0の制限

アップグレードする前に、 `at.js` 2.0の次の制限事項に注意してください。

* クロスドメイントラッキングは `at.js` 2.0ではサポートされていません。
* mboxOverride.browserIpパラメーターとmboxSession URLパラメーターは、 `at.js` 2.0ではサポートされていません
* 2.0では、従来の関数mboxCreate、mboxDefine、mboxUpdateは廃止されました。デフォルトのコンテンツが表示され、ネットワークリクエストは行われません。 `at.js`

## ビデオで使用されるライブラリフッターコード

以下のコードは、ビデオ中にライブラリの「ライブラリフッター」セクションに追加され `at.js` ました。 アプリが最初に読み込まれ、次にアプリ内のハッシュの変更時に発生します。 表示名にはハッシュのクリーンアップバージョンを使用し、ハッシュが空の場合は「home」を使用します。 SPAを識別するために、コードはURL内で「react/」というテキストを探します。このテキストは、サイト上で更新する必要がある可能性が高いことに注意してください。 また、SPAでカスタムイベントの実行を停止したり、コードを直接アプリに埋め込んだりする方が適切な場合があるこ `triggerView()` とにも注意してください。

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

* [at.js 2.0の仕組み（アーキテクチャ図）](understanding-how-atjs-20-works.md)
* [Adobe TargetのVisual Experience Composerをシングルページアプリケーションで使用(SPA VEC)](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.js 1.xからat.js 2.0ドキュメントへのアップグレード](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)
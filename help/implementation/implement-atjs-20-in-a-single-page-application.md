---
title: Adobe Targetのat.js 2.0をシングルページアプリ(SPA)に実装する
seo-title: Adobe Targetのat.js 2.0をシングルページアプリ(SPA)に実装する
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


# Adobe Targetのat.js 2.0をシングルページアプリ(SPA)に実装する

最新バージョンの`at.js`は、次世代のクライアント側テクノロジーでパーソナライゼーションを実行できる豊富な機能セットを提供します。 この新しいバージョンは、単一ページアプリ(SPA)との調和したやり取りを持つ`at.js`のアップグレードに焦点を当てています。

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## at.js 2.0をSPAに実装する方法

* `at.js` 2.0をシングルページアプリの&lt;head>に実装します。
* SPAで表示が変更されるたびに、`adobe.target.triggerView()`関数を実装します。 これを行うには、URLハッシュの変更のリッスン、SPAで実行されるカスタムイベントのリスニング、`triggerView()`コードの直接アプリケーションへの埋め込みなど、様々な手法を使用できます。 特定の単一ページアプリに最適なオプションを選択する必要があります。
* 表示名は`triggerView()`関数の最初のパラメータです。 シンプルで明確な一意の名前を付けて、ターゲットのVisual Experience Composerで簡単に選択できるようにします。
* 表示は、小さな表示の変更時にトリガーすることができます。また、SPA以外のコンテキスト（無限にスクロールするページの半分下など）でもトリガーできます。
* `at.js` 2.0およびは、Adobe Experience Platform Launchなどのタグ管理ソリューションを使用して導入 `triggerView()` できます。

## at.js 2.0の制限

アップグレードする前に、`at.js` 2.0の次の制限事項に注意してください。

* `at.js` 2.0では、クロスドメイントラッキングはサポートされていません
* mboxOverride.browserIpおよびmboxSession URLパラメーターは`at.js` 2.0ではサポートされていません
* 従来の関数mboxCreate、mboxDefine、mboxUpdateは、`at.js` 2.0では廃止されました。デフォルトのコンテンツが表示され、ネットワークリクエストは行われません。

## ビデオで使用されるライブラリフッターコード

次のコードは、ビデオの実行中に`at.js`ライブラリの「ライブラリフッター」セクションに追加されました。 アプリが最初に読み込まれ、次にアプリ内のハッシュの変更時に発生します。 表示名にはハッシュのクリーンアップバージョンを使用し、ハッシュが空の場合は「home」を使用します。 SPAを識別するために、コードはURL内で「react/」というテキストを探します。このテキストは、サイトで更新する必要がある可能性が高いことに注意してください。 また、SPAでカスタムイベントから`triggerView()`を起動したり、コードを直接アプリに埋め込んだりする方が適切である可能性があることにも注意してください。

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
* [Adobe TargetのVisual Experience Composerを使用したシングルページアプリ(SPA VEC)](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)
* [at.js 1.xからat.js 2.0ドキュメントへのアップグレード](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)
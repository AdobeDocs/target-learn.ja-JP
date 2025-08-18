---
title: at.js 2.0 をシングルページアプリケーション（SPA）に実装する方法
description: Adobe Targetの at.js 2.0 には、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行できる豊富な機能セットが用意されています。 次の手順に従って、at.js 2.0 をシングルページアプリケーション（SPA）に実装します。
role: Developer
level: Intermediate
topic: SPA, Architecture, Development
feature: Implementation
doc-type: technical video
kt: null
author: Daniel Wright
exl-id: 955f0571-5791-4dbb-9931-e6d5c8bb42a7
source-git-commit: fcd2273ba373dc2b3bc59a77f1925cdb7b2ed3ee
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Adobe Targetの at.js 2.0 のシングルページアプリケーション（SPA）への実装

Adobe Target `at.js` 2.0 には、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行できる豊富な機能セットが用意されています。 このバージョンは、`at.js` をアップグレードして、単一ページアプリケーション（SPA）との調和のとれたインタラクションを実現することに重点を置いています。

>[!VIDEO](https://video.tv.adobe.com/v/26248?quality=12)

## SPA への at.js 2.0 の実装方法

* シングルページアプリケーションの &lt;head> に `at.js` 2.0 を実装します。
* SPA でビューが変更されるたびに `adobe.target.triggerView()` 関数を実装します。 これを行うには、URL ハッシュの変更をリッスンする、SPA によって実行されるカスタムイベントをリッスンする、`triggerView()` コードをアプリケーションに直接埋め込むなど、様々な手法を採用できます。 特定の単一ページアプリケーションに最適なオプションを選択してください。
* ビュー名は、`triggerView()` 関数の最初のパラメーターです。 Target の Visual Experience Composer で簡単に選択できるように、シンプル、明確、一意の名前を使用します。
* ビューのトリガーは、小さなビューの変化の場合と、SPA 以外のコンテキスト（無限スクロールのページの半分の下など）の場合があります。
* `at.js` 2.0 以 `triggerView()` は、Adobe Experience Platform Launchなどのタグ管理ソリューションを通じて実装できます。

## at.js 2.0 の制限

アップグレードする前に、`at.js` 2.0 の次の制限事項に注意してください。

* クロスドメイントラッキングは `at.js` 2.0 ではサポートされていません
* mboxOverride.browserIp および mboxSession URL パラメーターは、`at.js` 2.0 ではサポートされていません。
* 従来の関数 mboxCreate、mboxDefine、mboxUpdate は、`at.js` 2.0 で非推奨（廃止予定）になりました。デフォルトのコンテンツが表示され、ネットワークリクエストは行われません。

## ビデオで使用されているライブラリフッターコード

ビデオの再生中に、`at.js` ライブラリのライブラリフッターセクションに次のコードが追加されました。 これは、アプリが最初に読み込まれ、次にアプリ内のハッシュが変更されたときに起動されます。 クリーンアップされたバージョンのハッシュをビュー名として使用し、ハッシュが空の場合は「home」を使用します。 SPA を識別するために、コードは URL 内で「react/」というテキストを探します。このテキストはサイトで更新する必要がある可能性が高くなります。 また、SPA では、カスタムイベントから `triggerView()` を実行するか、アプリに直接コードを埋め込むほうが適切な場合があることに注意してください。

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

* [at.js 2.0 の仕組みについて（アーキテクチャ図） ](understanding-how-atjs-20-works.md)
* [Adobe Targetのシングルページアプリケーション用 Visual Experience Composer （SPA VEC）の使用](../experiences/use-the-visual-experience-composer-for-single-page-applications.md)

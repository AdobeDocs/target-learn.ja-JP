---
title: Adobe Target要求の追加
description: 'AdobeMobile Services SDK(v4)は、Adobe Targetのメソッドと機能を提供し、様々なユーザーに対して様々なエクスペリエンスを提供してアプリをパーソナライズできます。   '
role: Developer
level: Intermediate
topic: Mobile, Personalization
feature: Implement Mobile
doc-type: tutorial
kt: 3040
thumbnail: null
exl-id: 88a5be3f-d61f-43e7-997a-574ef56122ed
source-git-commit: a6b645b6d9693a4c8882fd47ee0d61698c0b834d
workflow-type: tm+mt
source-wordcount: '1804'
ht-degree: 0%

---

# Adobe Target要求の追加

AdobeMobile Services SDK(v4)は、Adobe Targetのメソッドおよび機能を提供し、様々なユーザーに対して様々なエクスペリエンスを提供してアプリをパーソナライズできます。 通常、アプリからAdobe Targetに対して1つ以上のリクエストがおこなわれ、パーソナライズされたコンテンツを取得して、そのコンテンツの影響を測定します。

このレッスンでは、 [!DNL Target]リクエストを実装して、パーソナライゼーション用のWe.Travelアプリを準備します。

## 前提条件

必ず[をダウンロードし、サンプルアプリ](download-and-update-the-sample-app.md)を更新してください。

## 学習内容

このレッスンを最後まで学習すると、次の内容を習得できます。

* バッチプリフェッチ要求を使用して、複数の[!DNL Target]オファー（パーソナライズされたコンテンツ）をキャッシュする
* プリフェッチされた[!DNL Target]の場所を読み込む
* [!DNL Target]の場所をリアルタイムで読み込む（プリフェッチされていない）
* プリフェッチされた場所をキャッシュからクリア
* プリフェッチされたリクエストとリアルタイムリクエストの検証

## 用語

このチュートリアルの後半で使用するTargetの主な用語を以下に示します。

* **リクエスト：**  Adobe Targetサーバーへのネットワークリクエスト
* **オファー：**  ユーザーインターフェイス（またはAPIを使用）で定義され、応答で配信されるコードまたはその他のテキストベースのコンテンツのスニペット [!DNL Target] 。通常、[!DNL Target]がネイティブのモバイルアプリで使用されている場合はJSONです。
* **場所：**  リクエストに与えられるユーザー定義の名前。オファーを特定のリクエストに関連付ける [!DNL Target] ためのインターフェイスで使用されます
* **バッチリクエスト：**  複数の場所を含む単一のリクエスト
* **プリフェッチリクエスト：**  オファーを取得し、将来アプリで使用するためにメモリにキャッシュする単一のリクエスト
* **バッチプリフェッチ要求：**  複数の場所のオファーをプリフェッチする単一の要求
* **オーディエンス：**  インターフェイスで定義さ [!DNL Target] れた訪問者のグループ、または他の [!DNL Target] Adobeアプリケーション(例：「iPhone X訪問者」、「Visitors in the California」、「First App Open」)
* **アクティビティ：**  場所、オ [!DNL Target] ファー、オーディエンスをリンクし、パーソナライズされたエクスペリエンスを作成するユーザーインターフェイス（またはAPIを使用）で定義される構成 [!DNL Target] 体

## バッチプリフェッチ要求の追加

We.Travelで実装する最初のリクエストは、ホーム画面上の2つの[!DNL Target]場所を含むバッチプリフェッチリクエストです。 後のレッスンでは、新しいユーザーが予約プロセスを進めるのに役立つメッセージを表示する、これらの場所用のオファーを設定します。

プリフェッチリクエストは、Adobe Targetサーバーの応答（オファー）をキャッシュすることで、可能な限り少ない[!DNL Target]コンテンツを取得します。 バッチプリフェッチリクエストは、複数のオファーを取得してキャッシュし、それぞれが異なる場所に関連付けられます。 プリフェッチされたすべての場所は、ユーザーセッションで将来使用するためにデバイス上にキャッシュされます。 ホーム画面の複数の場所を取得することで、後で訪問者がアプリ内を移動する際に使用するオファーを取得できます。 プリフェッチメソッドの詳細については、[プリフェッチのドキュメント](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html?lang=en)を参照してください。

### バッチプリフェッチ要求の追加

アプリ/メイン/java/com.wetravel/Controllerの下にあるHomeActivityコントローラー（ホーム画面のソースコード）を更新します。 赤で表示される2つのコードブロックを追加します。

まず、HomeActivityコントローラ（Home Screenのソースコード）を使用します。このコントローラは、app/main/java/com.wetravel/Controllerの下にあります。

赤で表示される2つのコードブロックを追加します。

![HomeActivityプリフェッチコード](assets/homeactivity.jpg)

HomeActivityのコードの最後まで下にスクロールし、以下に示すコードを`setHeader()`関数の後に追加し、*replacing*&#x200B;を現在の`onResume()`関数に追加します。

```java
@Override
protected void onResume() {
    super.onResume();
    targetPrefetchContent();
}

public void targetPrefetchContent() {
    List<TargetPrefetchObject> prefetchList = new ArrayList<>();
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_home, null));
    prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_search, null));
    Target.TargetCallback<Boolean> prefetchStatusCallback = new Target.TargetCallback<Boolean>() {
        @Override
        public void call(final Boolean status) {
            HomeActivity.this.runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    String cachingStatus = status ? "YES" : "NO";
                    System.out.println("Received Response from prefetch : " + cachingStatus);
                    setUp();

                }
            });
        }};
    Target.prefetchContent(prefetchList, null, prefetchStatusCallback);
}
```

IDEでは、[!DNL Target]クラスがファイルにインポートされていないことを警告する場合があります。 次の赤で示すように、HomeActivityコントローラの上部に[!DNL Target]クラスを必ずインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

![ターゲットクラスのインポート](assets/import.jpg)

また、「cannot find symbol variable wetravel_engage_home」と「cannot find symbol variable wetravel_engage_search」のエラーが表示される場合もあります。 これらを`Constant.java`ファイル(app > src > main > java > com > wetravel > Utils)に追加します。

```java
public static final String wetravel_engage_home = "wetravel_engage_home";
public static final String wetravel_engage_search = "wetravel_engage_search";
```

![Constant.javaファイルに場所名を追加します](assets/constants.jpg)

### バッチプリフェッチ要求コードの説明

| コード | 説明 |
|--- |--- |
| `targetPrefetchContent()` | [!DNL Target]メソッドを使用して2つの[!DNL Target]の場所を取得してキャッシュする、ユーザー定義の関数（SDKの一部ではありません）。 |
| `prefetchContent()` | プリフェッチ要求を送信する[!DNL Target] SDKメソッド |
| `Constant.wetravel_engage_home` | ホーム画面にオファーコンテンツを表示する[!DNL Target]ロケーション名をプリフェッチ |
| `Constant.wetravel_engage_search` | 検索結果画面にオファーコンテンツを表示する[!DNL Target]ロケーション名をプリフェッチ。 これはプリフェッチの2番目の場所なので、このプリフェッチ要求は「プリフェッチバッチ要求」と呼ばれます。 |
| setUp() | [!DNL Target]オファーがプリフェッチされた後にアプリのホーム画面をレンダリングする、ユーザー定義関数 |

### 非同期と同期について

実装したコードを使用すると、プリフェッチ要求は、ホーム画面がレンダリングされる直前に、同期のブロック呼び出しとしておこなわれます。 新しいコードをHomeActivityコントローラーに貼り付けたとき、`setUp()`関数の実行を`onResume()`関数からTargetリクエストの後に移動しました。 これは、アプリを最初に開いたときにコンテンツをパーソナライズするシナリオで役立ちます。最初の画面がレンダリングされる前に、Targetサーバーからパーソナライズされたコンテンツが返される（またはタイムアウトする）からです。 リクエストが（バックグラウンドで）非同期で読み込まれるようにするには、代わりに`onCreate()`関数内で`setUp()`を呼び出します。

### バッチプリフェッチ要求の検証

アプリを再構築し、Androidエミュレーターを開きます。 (以下のスクリーンショットは、Android Qバージョン9以降（APIレベル29）のピクセル2を使用しています)。 プリフェッチ応答は、「プリフェッチ応答を受信しました」となります。

ホーム画面がレンダリングされたら、プリフェッチ要求を読み込む必要があります。 Logcatで、[!DNL "Target"]をフィルターして、要求と応答を確認します。

![ホーム画面での要求の検証](assets/prefetch_validation.jpg)

正常な応答が表示されない場合は、HomeActivityファイルの`ADBMobileConfig.json`ファイルおよびコード構文の設定を確認します。

これで、2つの場所がデバイスにキャッシュされます。 場所名は[!DNL Target]インターフェイスに遅延読み込みされ、アクティビティで使用する際に、様々なドロップダウンメニューで選択できます。

### キャッシュされた各場所に対する読み込み要求の追加

場所がプリフェッチされ、応答がデバイスにキャッシュされたので、オファーコンテンツをキャッシュから取得してアプリケーションを更新できる`Target.loadRequest()`メソッドを追加します。 プリフェッチ要求で実行される新しいカスタムメソッド`engageMessage()`を追加します。 `engageMessage()` が呼び出されま `Target.loadRequest()`す。`engageMessage()` はの前に実 `setUp()` 行され、読み込みリクエストが呼び出されてから画面が設定されます。

まず、HomeActivityのwetravel_engage_homeの場所に`engageMessage()`呼び出しとメソッドを追加します。

![最初の読み込みリクエストの追加](assets/wetravel_engage_home_loadRequest.jpg)

更新されたコードは次のとおりです。

```java
    public void targetPrefetchContent() {
        List<TargetPrefetchObject> prefetchList = new ArrayList<>();
        Map<String, Object> params1;
        params1 = new HashMap<String, Object>();
        params1.put("at_property", "your at_property value goes here");
        prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_home, params1));
        prefetchList.add(Target.createTargetPrefetchObject(Constant.wetravel_engage_search, params1));
        Target.TargetCallback<Boolean> prefetchStatusCallback = new Target.TargetCallback<Boolean>() {
            @Override
            public void call(final Boolean status) {
                HomeActivity.this.runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        String cachingStatus = status ? "YES" : "NO";
                        System.out.println("Received Response from prefetch : " + cachingStatus);
                        engageMessage();
                        setUp();
                    }
                });
            }};
        Target.prefetchContent(prefetchList, null, prefetchStatusCallback);
    }
    public void engageMessage() {
        Target.loadRequest(Constant.wetravel_engage_home, "", null, null, null,
            new Target.TargetCallback<String>(){
                @Override
                public void call(final String s) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            System.out.println("Engage Message : " + s);
                            if(s != null && !s.isEmpty()) Utility.showToast(getApplicationContext(), s);
                        }
                    });
                }
            });
    }
```

次に、SearchBusActivityのwetravel_engage_searchの場所の`engageMessage()`呼び出しとメソッドを追加します。 `engageMessage()`呼び出しは、`setUpSearch()`の呼び出しの前に`onResume()`メソッドで設定され、画面が設定される前に実行されます。

![2回目の読み込み要求の追加](assets/wetravel_engage_search_loadRequest.jpg)

更新されたコードは次のとおりです。

```java
    @Override
    public void onResume() {
        super.onResume();
        engageMessage();
        setUpSearch();
    }
    public void engageMessage() {
        Target.loadRequest(Constant.wetravel_engage_search, "", null, null, null,
                new Target.TargetCallback<String>(){
                    @Override
                    public void call(final String s) {
                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                System.out.println("Engage Message : " + s);
                                if(s != null && !s.isEmpty()) Utility.showToast(getApplicationContext(), s);
                            }
                        });
                    }
                });
    }
```

SearchBusActivityにTargetメソッドを追加したばかりなので、必ず[!DNL Target]クラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

## リアルタイムリクエストの追加

次に、アプリに追加するリクエストは、「ありがとうございました」画面でのリアルタイムリクエストになります。 「リアルタイム」とは、リクエストがおこなわれ、応答が即座に適用されることを意味します（後でキャッシュされることはありません）。 後のレッスンでは、このリクエストを使用して、ユーザーの旅行先に合わせてパーソナライズされたエクスペリエンスを作成します。

では、ありがとう画面でリアルタイムリクエストを追加します。 ThankYouActivityファイルで、赤で表示される変更を行います。
![ありがとう画面にリアルタイムの場所を追加](assets/thankyou.jpg)

ThankYouActivityファイルの最後までスクロールします。 `getRecommandations()`関数の3行をコメントアウトし、`targetLoadRequest()`関数の呼び出しを追加します。

```java
// AppDialogs.dialogLoaderHide();
// recommandations.addAll(recommandation.recommandations);
// recommandationbAdapter.notifyDataSetChanged();
```

次のコード行を`getRecommandations()`関数に追加します。

```java
targetLoadRequest(recommandation.recommandations);
```

次に、`targetLoadRequest()`関数を定義する必要があります。
![ありがとう画面にリアルタイムの場所を追加する](assets/thankyou2.jpg)

`filterRecommendationBasedOnOffer()`関数の後に次のコードブロックを追加します。

```java
public void targetLoadRequest(final ArrayList<Recommandation> recommandations) {
    Target.loadRequest(Constant.wetravel_context_dest, "", null, null, null, new Target.TargetCallback<String>() {
        @Override
        public void call(final String response) {
            try {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        AppDialogs.dialogLoaderHide();
                        filterRecommendationBasedOnOffer(recommandations, response);
                        recommandationbAdapter.notifyDataSetChanged();
                    }
                });
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    });
}
```

TargetメソッドをThankYouActivityに追加したばかりなので、必ずTargetクラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

### targetLoadRequest()コードの説明

| コード | 説明 |
|--- |--- |
| `targetLoadRequest()` | wetravel_context_destの場所を読み込んで表示する`Target.loadRequest()`を実行するユーザー定義関数（SDKの一部ではありません） |
| `Target.loadRequest()` | Targetサーバーにリクエストを送信するSDKメソッド |
| Constant.wetravel_context_dest | [!DNL Target]インターフェイスでアクティビティを作成する際に後で使用する、リクエストに割り当てられる場所名 |
| `filterRecommendationBasedOnOffer()` | 場所のオファーをTargetの応答から取得し、オファーのコンテンツに基づいてアプリの変更方法を決定する、アプリのユーザー定義関数 |
| `recommandations.addAll()` | ThankYou画面が読み込まれたときにデフォルトで実行するために使用される、アプリ内のユーザー定義関数ですが、Target応答を受け取り、`filterRecommendationBasedOnOffer()`で解析した後に実行されるようになりました。 |

これは、アプリをより高度なアップデートした後、ホーム画面に追加したリクエストを使用しておこなったので、少し時間を割いて操作内容を確認します。

1. コードの行をコメントアウトすることで、アプリの以前の動作で3つのデフォルトのプロモーションを表示するのを中断しました
1. 代わりに、targetLoadRequestという任意の名前を付けた新しい関数を実行するようにアプリに指示しました。
1. Target.loadRequestメソッドを使用してTargetにリクエストを送信する`targetLoadRequest`関数を定義し、[!DNL Target]オファーの応答を受け取るとすぐに`filterRecommendationBasedOnOffer()`関数を実行します
1. `filterRecommendationBasedOnOffer()`関数は応答を解釈し、画面に適用するプロモーションを決定します

これは、モバイルアプリで[!DNL Target]を使用する場合の一般的な使用パターンです。  モバイルアプリのほとんどすべての側面をパーソナライズできる点は、どちらも非常に強力です。 また、アプリのコードと、後で[!DNL Target]インターフェイスで定義するオファーとの間の調整も必要です。 この調整のため、一部のパーソナライゼーションの使用例では、アクティビティを起動するために、App Store内のアプリを更新する必要が生じる場合があります。

### リアルタイムリクエストの検証

Androidエミュレーターを開き、すべての手順を実行して旅行を予約します。ホーム/バス検索結果/座席選択、支払いオプション（空のデータを含むすべての支払いオプションが機能します）。

最後の「ありがとうございました」画面で、Logcatの反応を見てください。 応答は、「wetravel_context_destに対してデフォルトコンテンツが返されました」となります。

![ありがとうございました画面でのリアルタイムの場所の追加](assets/thankyou_validation.jpg)

## プリフェッチされた場所のキャッシュからの消去

セッション中にプリフェッチされたロケーションをクリアする必要が生じる場合があります。 例えば、予約が発生した場合、ユーザーが「関与」し、予約プロセスを理解したので、キャッシュされた場所を消去すると効果的です。 セッション中に別の旅行を予約する場合、ホーム画面や検索結果画面の元の場所を使用して予約をガイドする必要はありません。 キャッシュから場所をクリアし、割引された2回目の予約や他の関連シナリオ用の新しいオファーをプリフェッチする方が、より合理的です。 セッション中に予約がおこなわれた場合に新しい場所をプリフェッチするためのロジックをホーム画面と検索結果画面に追加できます。

この例では、予約がおこなわれた際に、セッションで事前に取得された場所を消去します。 これは、 `Target.clearPrefetchCache()`関数を呼び出すことで行われます。 次に示すように、`targetLoadRequest()`関数内に関数を設定します。

```java
Target.clearPrefetchCache()
```

![プリフェッチされた場所をキャッシュからクリア](assets/clearPrefetch.jpg)

おめでとう！ これで、アプリにパーソナライゼーションのフレームワークが追加されました。 次のレッスンでは、これらの場所にパラメーターを追加して、パーソナライゼーション機能を強化します。

**[次へ：「パラメータの追加」>](add-parameters.md)**

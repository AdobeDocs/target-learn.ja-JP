---
title: 追加Adobe Target要求
description: 'Adobe Mobile Services SDK(v4)は、様々なユーザーに対して様々なエクスペリエンスを使用してアプリをパーソナライズするためのAdobe Target方法と機能を提供します。   '
feature: mobile
kt: 3040
audience: developer
doc-type: tutorial
activity-type: implement
translation-type: tm+mt
source-git-commit: b331bb29c099bd91df27300ebe199cafa12516db
workflow-type: tm+mt
source-wordcount: '1804'
ht-degree: 0%

---


# 追加Adobe Target要求

Adobe Mobile Services SDK(v4)は、様々なユーザーに対して様々なエクスペリエンスを使用してアプリをパーソナライズするためのAdobe Target方法と機能を提供します。 通常、パーソナライズされたコンテンツを取得し、そのコンテンツの影響を測定するために、アプリからAdobe Targetに1つ以上のリクエストが行われます。

このレッスンでは、リクエストを実装して、パーソナライゼーション用のWeb.Travelアプリを準備し [!DNL Target] ます。

## 前提条件

サンプルアプリを必ず [ダウンロードして更新してください](download-and-update-the-sample-app.md)。

## 学習目標

このレッスンを終了すると、次のことができます。

* バッチプリフェッチ要求を使用した複数の [!DNL Target] オファー（パーソナライズされたコンテンツなど）のキャッシュ
* 事前に読み込まれた場所を読み込み [!DNL Target] ます
* リアルタイムでの [!DNL Target] 場所の読み込み（事前に取得されていません）
* キャッシュから事前に取得された場所を消去
* プリフェッチされたリクエストとリアルタイムリクエストの検証

## 用語

以下に、このチュートリアルの残りの部分で使用する主なTarget用語の一部を示します。

* **リクエスト：**  Adobe Targetサーバーへのネットワーク要求
* **オファー:**  ユーザーインターフェイス（またはAPI）で定義され、応答で配信されるコードまたはその他のテキストベースのコンテンツのスニペット [!DNL Target] 。 ネイティブモバイルアプリで [!DNL Target] 使用される場合は通常、JSONです。
* **場所：**  オファーを特定の要求に関連付けるためにインター [!DNL Target] フェイスで使用される、要求に与えられるユーザー定義名
* **バッチリクエスト：**  複数の場所を含む単一の要求
* **プリフェッチ要求：**  オファーを取得し、将来アプリで使用するためにメモリにキャッシュする単一のリクエスト
* **バッチプリフェッチ要求：**  複数の場所のオファーを事前に取得する単一の要求
* **オーディエンス:**  インター [!DNL Target] フェイスで定義された訪問者、または他のアドビアプリケーション [!DNL Target] (例： 「iPhone X訪問者」、「訪問者in the California」、「First App Open」)
* **アクティビティ:**  場所、 [!DNL Target] オファー、オーディエンスをリンクし、パーソナライズされたエクスペリエンスを作成する [!DNL Target] ユーザーインターフェイス（またはAPI）で定義される構成体

## バッチ追加プリフェッチ要求

Web.Travelで最初に実装するリクエストは、ホーム画面に2か所あるバッチプリフェッチ要求 [!DNL Target] です。 後のレッスンでは、新しいユーザが予約プロセスを進める際に役立つように、メッセージを表示する場所のオファーを設定します。

プリフェッチ要求は、Adobe Targetサーバの応答(オファー)をキャッシュすることにより、可能な限り [!DNL Target] 少なくコンテンツを取得します。 バッチプリフェッチ要求は、複数のオファーを取得してキャッシュし、それぞれが異なる場所に関連付けられています。 事前に取り込まれた場所は、後でユーザーセッションで使用できるように、デバイス上にキャッシュされます。 ホーム画面で複数の場所をプリフェッチすることで、後で訪問者がアプリ内を移動する際に使用するオファーを取得できます。 プリフェッチ方法の詳細については、 [プリフェッチのドキュメント](https://docs.adobe.com/content/help/en/mobile-services/android/target-android/c-mob-target-prefetch-android.html) を参照してください。

### バッチ追加プリフェッチ要求

HomeActivityコントローラー（ホーム画面のソースコード）を更新します。これは、app > main > java > com.wetravel > Controllerの下にあります。 赤で示す2つのコードブロックを追加します。

HomeActivityコントローラー（ホーム画面のソースコード）との開始は、app > main > java > com.wetravel > Controllerの下にあります。

赤で示す2つのコードブロックを追加します。

![HomeActivityプリフェッチコード](assets/homeactivity.jpg)

HomeActivityのコードの最後まで下にスクロールし、以下に示すコードを関数の後に追加して、 `setHeader()` 現在の *関数を*`onResume()` 置き換えます。

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

IDEでは、ファイルにインポートされた [!DNL Target] クラスがないことを警告する場合があります。 次の赤で示すように、HomeActivityコントローラーの上部にある [!DNL Target] クラスを必ずインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

![Targetクラスの読み込み](assets/import.jpg)

また、「シンボル変数が見つかりません」と「シンボル変数が見つかりませんwetravel_engage_home」、「シンボル変数が見つかりません」というエラーが表示される場合もあります。 これら追加のファイルは、 `Constant.java` ファイル(app > src > main > java > com > wetravel > Utils)に追加します。

```java
public static final String wetravel_engage_home = "wetravel_engage_home";
public static final String wetravel_engage_search = "wetravel_engage_search";
```

![Constant.追加javaファイルの場所の名前](assets/constants.jpg)

### バッチプリフェッチ要求コードの説明

| コード | 説明 |
|--- |--- |
| `targetPrefetchContent()` | 2つの場所を取得およびキャッシュする [!DNL Target] メソッドを使用する、（SDKの一部ではなく）ユーザー定義の関数 [!DNL Target] 。 |
| `prefetchContent()` | プリフェッチ要求を送信する [!DNL Target] SDKメソッド |
| `Constant.wetravel_engage_home` | ホーム画面にオファーの内容を表示する、事前に取得した [!DNL Target] 場所の名前 |
| `Constant.wetravel_engage_search` | オファーの内容を検索結果画面に表示する、事前に取得された [!DNL Target] 場所の名前。 これはプリフェッチの2番目の場所なので、このプリフェッチ要求は「プリフェッチバッチ要求」と呼ばれます。 |
| setUp() | オファーが事前に取得された後にアプリのホーム画面をレンダリングするユーザー定義の関数 [!DNL Target] |

### 非同期と同期について

先ほど導入したコードを使用して、プリフェッチ要求は、ホーム画面がレンダリングされる直前に、同期呼び出し（ブロック呼び出し）として行われます。 新しいコードをHomeActivityコントローラに貼り付けたとき、関数の実行を関数からTarget要求の後まで移動しました。 `setUp()``onResume()` これは、アプリを最初に開いたときにコンテンツをパーソナライズするシナリオで有益です。最初の画面がレンダリングされる前に、Targetサーバーからパーソナライズされたコンテンツが返された（またはタイムアウトした）ことを確認できるからです。 リクエストを（バックグラウンドで）非同期に読み込むには、代わりに `setUp()``onCreate()` 関数内で呼び出すだけです。

### バッチプリフェッチ要求の検証

アプリを再構築し、Androidエミュレーターを開きます。 （以下のスクリーンショットは、Android Qバージョン9以降のAPIレベル29でピクセル2を使用しています）。 プリフェッチ応答は「プリフェッチ応答受信」と読み取る必要があります。

ホーム画面がレンダリングされるときは、プリフェッチ要求を読み込む必要があります。 Logcatを使用して、リクエストと応答 [!DNL "Target"] を表示するフィルターを作成します。

![ホーム画面での要求の検証](assets/prefetch_validation.jpg)

正常な応答が表示されない場合は、HomeActivityファイルの `ADBMobileConfig.json` ファイルおよびコード構文の設定を確認します。

現在は、2つの場所がデバイスにキャッシュされます。 場所の名前は、インター [!DNL Target] フェイスに間もなく遅延読み込みされ、アクティビティで使用する場合に、様々なドロップダウンメニューで選択できます。

### キャッシュされた各場所の追加要求の読み込み

場所が事前に取得され、応答がデバイスにキャッシュされたので、オファーの内容をキャッシュから取得する `Target.loadRequest()` メソッドを追加して、アプリケーションを更新できるようにします。 プリフェッチ要求で実行する新しいカスタムメソッド `engageMessage()` を追加します。 `engageMessage()` が呼び出 `Target.loadRequest()`されます。 `engageMessage()` の前に実行 `setUp()` され、読み込み要求が呼び出された後で画面が設定されます。

最初に、HomeActivity内のwetravel_engage_homeの場所の `engageMessage()` 呼び出しとメソッドを追加します。

![追加最初の読み込み要求](assets/wetravel_engage_home_loadRequest.jpg)

次に、更新されたコードを示します。

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

次に、SearchBusActivityのwetravel_engage_searchの場所の `engageMessage()` 呼び出しとメソッドを追加します。 呼び出しの前に `engageMessage()` メソッド内で `onResume()` 呼び出しが設定され、画面が設定される前 `setUpSearch()` に呼び出しが実行されます。

![2回目の読み込み要求](assets/wetravel_engage_search_loadRequest.jpg)

次に、更新されたコードを示します。

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

SearchBusActivityにTargetメソッドを追加したばかりなので、必ず次の [!DNL Target] クラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

## リ追加アルタイムリクエスト

次にアプリに追加するリクエストは、「ありがとうございます」画面にリアルタイムでリクエストされます。 「リアルタイム」とは、リクエストが行われ、応答が直ちに適用される（後でキャッシュされるのではない）という意味です。 後のレッスンでは、このリクエストを使用して、ユーザーの旅行先に合わせてパーソナライズしたエクスペリエンスを作成します。

では、「ありがとうございます」画面にリアルタイムのリクエストを追加します。 ThankYouActivityファイルで、赤で表示される変更を行います。
![「ありがとう追加ございます」画面のリアルタイムの場所](assets/thankyou.jpg)

ThankYouActivityファイルの最後までスクロールします。 関数内の3行をコメントアウトし、関数の呼び出しを追加し `getRecommandations()` ま `targetLoadRequest()` す。

```java
// AppDialogs.dialogLoaderHide();
// recommandations.addAll(recommandation.recommandations);
// recommandationbAdapter.notifyDataSetChanged();
```

関追加数に対するコードの次の行： `getRecommandations()`

```java
targetLoadRequest(recommandation.recommandations);
```

次に、 `targetLoadRequest()` 関数を定義する必要があります。
![「ありがとう追加ございます」画面のリアルタイムの場所](assets/thankyou2.jpg)

この追加コードは、 `filterRecommendationBasedOnOffer()` 関数の後に次のようにブロックされます。

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

ThankYouActivityにTargetメソッドを追加した後なので、必ずTargetクラスをインポートしてください。

```java
import com.adobe.mobile.Target;
import com.adobe.mobile.TargetPrefetchObject;
```

### targetLoadRequest()コードの説明

| コード | 説明 |
|--- |--- |
| `targetLoadRequest()` | 読み込んでwetravel_context_destの場所を表示するユーザー定義関数（SDKの一部ではない） `Target.loadRequest()` |
| `Target.loadRequest()` | Targetサーバーにリクエストを送信するSDKメソッド |
| Constant.wetravel_context_dest | インターフェイスでアクティビティを構築する際に、後で使用する要求に割り当てられる場所の名前です。 [!DNL Target] |
| `filterRecommendationBasedOnOffer()` | オファーの応答から場所のオファーを取得し、Targetのコンテンツに基づいてアプリの変更方法を決定する、アプリのユーザー定義関数 |
| `recommandations.addAll()` | 「ありがとうございます」画面が読み込まれたときにデフォルトで実行されるアプリ内のユーザー定義関数です。ただし、Target応答が受信され、解析された後に実行されるようになりました `filterRecommendationBasedOnOffer()` |

これは、ホーム画面に追加したリクエストを使用してアプリに対して行ったより高度な更新です。次に、行った操作を確認します。

1. コード行をコメントアウトして、アプリの以前の3つのデフォルトのプロモーション表示の動作を中断しました。
1. 代わりに新しい関数を実行するようにアプリに指示しました。この関数の名前は、任意にtargetLoadRequestという名前にしました。
1. Target.loadRequestメソッドを使用してTargetにリクエストを行う `targetLoadRequest` 関数を定義し、 `filterRecommendationBasedOnOffer()`[!DNL Target] オファーの応答を受け取ったらすぐにこの関数を実行するようにしました
1. この `filterRecommendationBasedOnOffer()` 関数は応答を解釈し、どのプロモーションを画面に適用するかを決定します

これは、モバイルアプリでを使用する場合の非常に一般的な使用パターン [!DNL Target] です。  どちらも非常に強力で、モバイルアプリのほとんどすべての側面をパーソナライズできます。 また、アプリのコードと、後でインター [!DNL Target] フェイスで定義するオファーとの間の調整も必要です。 この調整のため、一部のパーソナライゼーションの使用例では、アクティビティを起動するためにアプリストアのアプリを更新する必要がある場合があります。

### リアルタイムリクエストの検証

Androidエミュレーターを開き、次のすべての手順に従って旅行を予約します。 ホーム/バス検索結果/座席選択、支払いオプション（空白のデータを含む支払いオプションはすべて有効）。

最後の「ありがとうございます」画面で、Logcatの回答を見てください。 応答には、「Default content was returned for &quot;wetravel_context_dest&quot;:

![「ありがとう追加ございます」画面のリアルタイムの場所](assets/thankyou_validation.jpg)

## プリフェッチされた場所をキャッシュからクリア

セッション中に、プリフェッチされた場所をクリアする必要がある場合があります。 例えば、予約が発生した場合、ユーザーが「関与」しているので、キャッシュされている場所をクリアして予約プロセスを理解した方が効果的です。 セッション中に別の旅行を予約した場合、ホーム画面や検索結果画面で予約の案内をする際に、元の場所は必要ありません。 キャッシュから場所をクリアし、新しいオファーをプリフェッチして、2回目の予約を割り引かれたり、別の関連シナリオに対応したりする方が、より効果的です。 セッション中に予約が発生した場合に新しい場所を先読みするロジックをホーム画面および検索結果画面に追加できます。

この例では、予約が発生したセッションで事前に取得された場所を消去します。 これは、 `Target.clearPrefetchCache()` 関数を呼び出すことで行われます。 関数内に次のように設定し `targetLoadRequest()` ます。

```java
Target.clearPrefetchCache()
```

![プリフェッチされた場所をキャッシュから消去](assets/clearPrefetch.jpg)

おめでとう！ これで、アプリにパーソナライゼーションのフレームワークが追加されました。 次のレッスンでは、これらの場所にパラメータを追加することで、パーソナライゼーション機能を強化します。

**[次へ： &quot;追加パラメーター&quot; >](add-parameters.md)**

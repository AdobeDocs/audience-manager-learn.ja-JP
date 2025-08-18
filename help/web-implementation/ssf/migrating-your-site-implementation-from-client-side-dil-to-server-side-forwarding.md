---
title: サイトのAudience Managerの実装をクライアントサイドのDILからサーバーサイド転送に移行する
description: サイトのAudience Manager（AAM）の実装をクライアントサイドのDILからサーバーサイド転送に移行する方法を説明します。 このチュートリアルは、AAMとAdobe Analyticsの両方があり、DIL（Data Integration Library）コードを使用してページからAAMにヒットを送り、また、ページからAdobe Analyticsにもヒットを送る場合に適用されます。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# サイトのAudience Managerの実装をクライアントサイドのDILからサーバーサイド転送に移行する {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

このチュートリアルは、Adobe Audience Manager（AAM）とAdobe Analyticsの両方があり、現在、DIL（[!DNL Data Integration Library]）コードを使用してページからAAMにヒットを送り、また、ページからAdobe Analyticsにもヒットを送っている場合に適用されます。 これらのソリューションは両方とも存在し、どちらもAdobe Experience Cloudの一部なので、サーバーサイド転送を有効にするベストプラクティスに従うことができます。これにより、クライアントサイドのコードでページからAAMにさらにヒットを送るのではなく、[!DNL Analytics] データ収集サーバーがサイト分析データをリアルタイムでAudience Managerに転送できます。 このチュートリアルでは、古いクライアントサイド DIL実装から新しいサーバーサイド転送方式に切り替える手順について説明します。

## クライアントサイド（DIL）とサーバーサイドの比較 {#client-side-dil-vs-server-side}

Adobe Analytics データをAAMに取り込むこれら 2 つの方法を比較および対比する場合、最初に次の図の違いを視覚化すると役立つ場合があります。

![ クライアントサイドからサーバーサイドへ ](assets/client-side_vs_server-side_aam_implementation.png)

### クライアントサイド DILの実装 {#client-side-dil-implementation}

この手法を使用してAdobe Analytics データをAAMに送信すると、web ページから 2 つのヒットが発生します。1 つは [!DNL Analytics] に送信され、もう 1 つはAAMに送信されます（web ページに [!DNL Analytics] データをコピーした後）。 [!UICONTROL Segments] はAAMからページに返され、パーソナライゼーションに使用できます。 これはレガシー実装と見なされ、推奨されません。

これはベストプラクティスに従わないという事実に加えて、この方法を使用するデメリットには次のものがあります。

* ページからの 1 つではなく 2 つのヒット
* AAM オーディエンスを [!DNL Analytics] にリアルタイムで共有するにはサーバーサイド転送が必要なので、クライアントサイド実装ではこの機能（および将来の他の機能）が許可されません

AAM実装のサーバーサイド転送方式に移行することをお勧めします。

### サーバーサイド転送の実装 {#server-side-forwarding-implementation}

上の画像に示すように、ヒットは、web ページからAdobe Analyticsに到達します。 [!DNL Analytics] は、そのデータをリアルタイムでAAMに転送し、ヒットがページから直接来たかのように、訪問者はAAMの特性と [!UICONTROL segments] に評価されます。

[!UICONTROL Segments] は、同じリアルタイムヒットで [!DNL Analytics] に返され、パーソナライゼーションなどのために web ページに応答を転送します。

サーバーサイド転送に移行する際のタイミングのデメリットはありません。 Adobeでは、Audience Managerと [!DNL Analytics] の両方を持っているユーザーには、この実装方法を使用することを強くお勧めします。

## これには、主に次の 2 つのタスクがあります {#you-have-two-main-tasks}

このページにはかなり多くの情報があり、もちろん、それはすべて重要です。 ただし、**すべては、次の 2 つの主な作業に要約されます**。

1. コードをクライアントサイドのDIL コードからサーバーサイド転送コードに変更する
1. [!DNL Analytics] [!DNL Admin Console] でスイッチをフリップして、データの実際の転送を開始します（[!UICONTROL report suite] あたり）

これらのタスクのいずれかをスキップすると、サーバーサイド転送は正しく機能しません。 このドキュメントには、設定に合わせてこれらの 2 つの手順を正しく実行できるように、手順と追加データが追加されています。

## 実装オプション {#implementation-options}

クライアントサイド転送からサーバーサイド転送に移行する際に行うタスクの 1 つは、コードを新しいサーバーサイド転送コードに変更することです。 これは、次のいずれかのオプションを使用して行います。

* Adobe Experience Platform タグ - Adobeで web プロパティに推奨される実装オプション。 Platform タグがすべての困難な作業を代行してくれるので、これは簡単な作業です。
* ページ上 – （まだ）Adobe Launch を使用していない場合は、新しい SSF コードを `doPlugins` ファイル内の `appMeasurement.js` 関数に直接配置することもできます
* その他のタグマネージャー – これらは前の（ページ上の）オプションと同じように扱うことができます。他のタグマネージャーが `doPlugins` コードを保存している場所では [!DNL AppMeasurement] コードを配置し続けるからです

_コードの更新_ の節では、以下にそれぞれについて説明します。

## 実装手順 {#implementation-steps}

次の手順で実装を説明します。

### 手順 0：前提条件：Experience Cloud ID サービス（ECID） {#step-prerequisite-experience-cloud-id-service-ecid}

サーバーサイド転送に移行するための主な前提条件は、Experience Cloud ID サービスを実装することです。 これは、Experience Platform Launch を使用している場合に最も簡単に実行できます。この場合は、ECID 拡張機能をインストールするだけで、残りの処理が実行されます。

Adobe以外の TMS を使用している場合、または TMS をまったく使用していない場合は、ECID を実装して、他のAdobe ソリューションを **事前** 実行してください。 詳しくは、[ECID ドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) を参照してください。 もう 1 つの前提条件はコードバージョンに関するものなので、次の手順でコードの最新バージョンを適用するだけなので、問題ありません。

>[!NOTE]
>
>実装する前に、このドキュメント全体をお読みください。 以下の「タイミング」の節では、ECID を含む各要素を実装する *タイミング* に関する重要な情報を示します（まだ実装されていない場合）。

### 手順 1:DIL コードから現在使用されているオプションを記録する {#step-record-currently-used-options-from-dil-code}

クライアントサイドのDIL コードからサーバーサイド転送に移行する準備が整ったら、最初の手順は、カスタム設定やAAMに送信されるデータなど、DIL コードで行っているすべてを特定することです。 注意し、考慮すべき事項は次のとおりです。

* [!DNL Analytics] DIL モジュールを使用した通常の `siteCatalyst.init` 変数 – この問題を心配する必要はありません。これは、通常の [!DNL Analytics] 変数を送信するだけなので、サーバーサイド転送を有効にするだけで発生します。
* Partner Subdomain - `DIL.create` 関数で、`partner` パラメーターをメモします。 これは、「パートナーサブドメイン」、または場合によっては「パートナー ID」と呼ばれ、新しいサーバーサイド転送コードを配置する際に必要になります。
* [!DNL Visitor Service Namespace] - 「[!DNL Org ID]」または「[!DNL IMS Org ID]」とも呼ばれ、新しいサーバーサイド転送コードを設定する際にも必要になります。 書き留めてください。
* containerNSID、uuidCookie、その他の詳細オプション – 使用している追加の詳細オプションをメモして、サーバーサイド転送コードでも設定できるようにします。
* 追加のページ変数 – ページからAAMに送信されている他の変数がある場合（siteCatalyst.init で処理される通常の [!DNL Analytics] 変数に加えて）、サーバーサイド転送（spoiler alert:[!DNL contextData] 変数）で送信できるようにメモする必要があります。

### 手順 2：コードを更新する {#step-updating-the-code}

[ 実装オプション ](#implementation-options) （上記）では、サーバーサイド転送を実装する方法と場所に関して複数のオプションが示されています。 このセクションを有効にするには、次のセクションに分ける必要があります（そのうちの 2 つを組み合わせる）。 必要に応じて、このセクションの方法を参照してください。

#### Adobe Experience Platform タグ {#launch-by-adobe}

Experience Platform Launch でクライアントサイドのDIL コードからサーバーサイド転送に実装オプションを移行する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### &quot;ページ上&quot;またはAdobe以外のタグマネージャー {#on-the-page-or-non-adobe-tag-manager}

ファイルまたはAdobe以外のタグ管理システムに格納された、クライアントサイドのDIL コードからサーバーサイド転送に実装オプションを移 [!DNL AppMeasurement] する方法については、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 手順 3：転送の有効化（[!UICONTROL Report Suite] あたり） {#step-enabling-the-forwarding-per-report-suite}

このチュートリアルでは、コードをクライアントサイドのDIL コードからサーバーサイド転送に切り替えるのに時間を費やしました。 それは、より難しい部分であるため、問題ありません。 この節は、非常に簡単ですが、コードを更新するのと同じくらい重要です。 このビデオでは、Analytics からAudience Managerへの実際のデータ転送を有効にするスイッチの切り替え方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** ビデオで説明しているように、転送の有効化がExperience Cloud バックエンドに完全に実装されるまでに、最大 4 時間かかることに注意してください。

## タイミング {#timing}

クライアントサイドのDILからサーバーサイド転送に移行するには、主に次の 2 つのタスクがあります。

1. コードの更新
1. [!DNL Analytics] [!DNL Admin Console] ード内でスイッチをフリップする

しかし問題は君が最初にするのは何だ？ それは重要ですか？ OK、ごめんなさい、それは 2 つの質問でした。 しかし、答えは…それは依存し、はい、それは *することができます* 重要です。 あいまいな話か？ 話を細かく分けよう。 しかし、最初に、多数のサイトを持つ大企業の場合に発生する可能性がある追加の質問：一度にすべてを行う必要がありますか？ あの方が少しやさしい。 いいえ。 あなたは少しずつそれをすることができる。

### 少し深く掘り下げる {#a-little-deeper-dive}

タイミングと注文が重要な理由は、転送の _実際には_ の仕組みによります。次の技術的な事実に要約できます。

* Experience Cloud ID サービス（ECID）を実装しており、[!DNL Analytics] [!DNL Admin Console] のスイッチ（「スイッチ」）がオンになっている場合は、コードがまだ更新されていなくても、データは [!DNL Analytics] からAAMに転送されます。
* ECID が実装されていない場合、スイッチがオンになっていて、サーバーサイド転送コードが設定されていても、データは転送されません。
* （Platform タグ内でもページ上でも）サーバーサイド転送コードが実際に応答を処理するので、移行を完了するために必要です。
* サーバーサイド転送スイッチは [!UICONTROL report suite] で有効になっていますが、コードは Platform タグのプロパティで処理されます。また、Platform タグを使用しない場合は [!DNL AppMeasurement] ファイルで処理されます。

### ベストプラクティス {#best-practices}

これらの技術的な詳細に基づいて、実行するタイミングとタイミングに関する推奨事項を次に示します。

#### ECID がまだ実装されていない場合 {#if-you-do-not-have-ecid-yet-implemented}

1. サーバーサイド転送を有効にする [!DNL Analytics] ごとに、スイッチを [!UICONTROL report suite] に切り替えます。

   1. ECID がないため、転送がまだ開始しません。

1. サイトごとに、クライアントサイドのDILからサーバーサイド転送（Platform タグの場合があります）またはページでコードを更新します（上記の別の節で説明しました）。

   1. これで転送がフローし（ECID を追加した場合）、[!DNL Analytics] ビーコンに対する適切な JSON 応答も受信されます（詳しくは、以下の検証とトラブルシューティングのセクションを参照してください）。

#### ECID が実装されている場合 {#if-you-do-have-ecid-implemented}

1. DILからサーバーサイド転送 PER [!UICONTROL report suite] にコードを更新する準備ができて計画を立て、サーバーサイド転送を有効にします。

   1. スイッチを [!DNL Analytics] にフリップして、サーバーサイド転送を有効にします。

      1. ECID が有効になっているため、転送が開始されます。

   1. コードをクライアントサイドのDILからシングルサイド転送にできるだけ早く更新します（Platform タグ内の転送や、ページ上の転送など、前述の別の節で説明した方法が考えられます）。

      1. [!DNL Analytics] ビーコンに対する適切な JSON 応答が返されます（詳しくは、以下の [ 検証とトラブルシューティング ](#validation-and-troubleshooting) の節を参照してください）。

>[!NOTE]
>
>上記の手順 1 と 2 の間では、データが重複してAAMに送信されるので、これら 2 つの手順は可能な限り近い場所で行うことが重要です。 つまり、シングルサイド転送は [!DNL Analytics] からAAMにデータを送信し始めており、DIL コードがまだページに残っているので、ページからAAMに直接ヒットすることもあり、データが 2 倍になります。 DILからサーバーサイド転送にコードを更新するとすぐに、この問題が軽減されます。

>[!NOTE]
>
>データの重複を小さくするのではなく、データに小さな不一致を生じさせる場合は、上記の手順 1 と 2 の順序を切り替えることができます。 コードをDILからサーバーサイド転送に移動すると、[!UICONTROL report suite] ーバーのサーバーサイド転送をオンにするようにスイッチを切り替えられるまで、AAMへのデータフローが停止します。 通常、お客様は、訪問者が特性や [!UICONTROL segments] ータを見落とすのではなく、データを少し倍増させたいと考えています。

#### 多数のサイトと [!UICONTROL report suites] ージがある場合の移行タイミング {#migration-timing-when-you-have-many-sites-and-report-suites}

このトピックについては、前の節で簡単に説明しました。その中で、主な戦略は次の方法で要約できます。

一度に 1 つのサイト/[!UICONTROL report suite] （またはサイト/[!UICONTROL report suites] のグループ）を移行します。

ただし、これは、いくつかの考えられるシナリオに基づいて少し難しくなる可能性があります。

* 複数の異なる [!UICONTROL report suites] を含むサイトがある場合
* 複数のサイトを含む [!UICONTROL report suite] ージがある場合（グローバル [!UICONTROL report suite] など）
* 1 つの Platform タグプロパティを使用して複数のサイトをカバーする
* サイトごとに異なる開発チームがあります

これらのアイテムのため、それは少し複雑になる可能性があります。 私が提案できる最善のことは次のとおりです。

* 前述のことに基づいて、サーバーサイド転送への移行戦略を立てるため、少し時間をかけます
* Platform タグ（または単一の [!DNL AppMeasurement] ファイル）の単一のプロパティは通常 1 つまたは 2 つの異なる [!UICONTROL report suites] にマッピングされるという事実に基づいて、エンタープライズからサーバーサイドへの転送を更新し、これらの異なるグループで 1 つずつ動作するプランを作成できる可能性があります
* Adobe Consultingを使用している場合は、必要に応じてサポートを受けられるように、移行プランについて話し合います

## 検証とトラブルシューティング {#validation-and-troubleshooting}

サーバーサイド転送が正常に稼働していることを検証する主な方法は、アプリからのAdobe Analyticsのヒットに対する応答を調べることです。

[!DNL Analytics] からAudience Managerへのサーバーサイド転送を行っていない場合は、（2 x 2 ピクセルに加えて） [!DNL Analytics] ビーコンへの応答はありません。 ただし、サーバーサイド転送を行っている場合は、[!DNL Analytics] のリクエストと応答で確認できる項目があり、Audience Managerと正しく通信し、ヒットを転送して応答を受け取 [!DNL Analytics] ていることを知らせます。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>偽の「成功」に注意してください。 応答があり、すべてが機能しているように見える場合は、応答に `stuff` オブジェクトがあることを確認します。 そうでない場合は、`"status":"SUCCESS"` というメッセージが表示される場合があります。 これはクレイジーに聞こえますが、これは実際にそれが正しく機能していない証拠です。
>
>このメッセージが表示される場合は、Platform タグまたは [!DNL AppMeasurement] のコードの更新は完了しているが、[!DNL Analytics] [!DNL Admin Console] の転送はまだ完了していないことを意味します。 この場合、サー [!DNL Analytics] スの [!DNL Admin Console] [!UICONTROL report suite] でサーバーサイド転送が有効になっていることを確認する必要があります。 まだ 4 時間経っていない場合は、バックエンドで必要な変更をすべて行うのに時間がかかる可能性があるので、しばらくお待ちください。


![false success](assets/falsesuccess.png)

サーバーサイド転送について詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=ja) を参照してください。

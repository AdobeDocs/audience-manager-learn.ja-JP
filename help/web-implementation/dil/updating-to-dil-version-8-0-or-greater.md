---
title: Adobe Audience Manager DIL バージョン 8.0以降へのアップデート
description: この記事では、Adobe Audience Manager（AAM）Data Integration Library（DIL）コードをバージョン 8.0以降にアップデートする手順と推奨事項について説明します。 これは、Adobe Analytics データのサーバーサイド転送ではなく、「クライアントサイド」DIL実装を指しており、Adobe タグマネージャーソリューションを使用しないDTM、Adobe Cloud Platform Launch、および実装について説明します。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
TQID: https://experienceleague.adobe.com/uM1GY5cQLRo0qsxnsfrbAuEB-1EJhDyCMyvbmCSbBbQ
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
subfeature_v2:
  - id: d7e573ad-4eda-46ec-90c4-239e75362af9
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: df401a2a-327d-468c-a5e4-b7b7ccd071a0
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 1196
ht-degree: 6%

---

# Adobe Audience ManagerのDIL バージョン 8.0以降へのアップデート {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

この記事では、Adobe Audience Manager （AAM） [!DNL Data Integration Library] （DIL） コードをバージョン 8.0以降にアップデートする手順と推奨事項について説明します。 これは、Adobe Analytics データのサーバーサイド転送ではなく、「クライアントサイド」DIL実装を指しており、Adobe タグマネージャーソリューションを使用しないDTM、Adobe Cloud Platform Launch、および実装について説明します。

## 概要 {#overview}

Audience Managerの[!DNL Data Integration Library] （DIL） コードを使用すると、Web サイトにAAMを実装できます*。 以前のバージョンのDILを実装する場合、AdobeのExperience Cloud ID サービス（ECID）も実装する必要はありませんでした（非常に良いアイデアでしたが）。 DIL バージョン 8.0以降では、ECID バージョン 3.3以降にハード依存関係があります。 ECID 3.3なしで、または以前のバージョンでDIL 8.0以降を実装する場合は、エラーが発生し、機能しません。 AAMの導入方法はいくつかありますが、ここでは、いくつかのレコメンデーションを紹介するだけでなく、いくつかのステップを紹介するためにこのページを作成しました。 以下では、プラットフォーム/実装方法ごとに、これらのステップと推奨事項を説明します。 DILについて詳しくは、[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)を参照してください。

* このページの説明に記載されているように、Adobe Analyticsを使用していないAAMのお客様が使用するDILの「クライアントサイド」実装のみを対象とします。 Adobe Analyticsがある場合は、AAMを実装するサーバーサイド転送方式を使用する必要があります。 この方法については、[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html?lang=ja)を参照してください。

## エレメントとメソッドの重複と非推奨 {#duplicate-and-deprecated-elements-and-methods}

以前のバージョンのDILとECIDでは、重複するメソッド（DILとECIDの両方で同じ機能を実行するメソッド）があり、どのメソッドを使用するかについて混乱が生じていました。 通常、両方を使用してマッチングさせる必要がありますが、そのメッセージは顧客に十分に伝わっていませんでした。 DIL 8.0以降、これらの重複したメソッドとエレメントはDILで非推奨（廃止予定）となり、ECID バージョンを使用することをお勧めします。

例：

* [!DNL DIL.create]を使用する場合、一部の要素は非推奨（廃止予定）になっており、代わりにECID要素を使用する必要があります。 これらの要素は、[[!DNL DIL.create]  ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)で説明されています。
* [!DNL idSync] インスタンスレベルのメソッドも、メソッドの[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)で説明されているように、非推奨（廃止予定）になりました。

## 顧客IDで同期するID {#id-syncing-with-a-customer-id}

AAMでは、マシン上のUUID （匿名の一意のユーザーID）を顧客IDと同期できるため、その顧客に関するオフラインデータをアップロードし、オンライン行動と結び付けて、顧客をより深く理解できます。 以前は、次の2つの方法のいずれかで行われていました。

* [!DNL idSync] インスタンスレベルのメソッド
* [!DNL DIL.create]の[!DNL declaredId]要素

これらの古い方法のいずれかを使用して顧客IDと同期している場合は、ECID サービスの一部である[!DNL setCustomerIDs] メソッドを使用してに更新することを強くお勧めします。 [!DNL setCustomerIDs]の詳細については、メソッドの[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=ja)を参照してください。

**簡単なヒント：**&#x200B;以前に上記のいずれかの方法を使用していた場合、AAM [!UICONTROL Data Source]を[!UICONTROL Data Source] ID （別名「DPID」）で参照していました。 [!DNL setCustomerIDs]に更新する場合は、代わりにAAM [!UICONTROL Data Source]の「[!UICONTROL Integration Code]」を使用する必要があります。 同じ[!UICONTROL Data Source]を指していますが、識別子が異なるだけです。 これは下のビデオに示されています。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下の節では、実装方法に基づいてDIL 8.0にアップデートする手順と推奨事項を示します。

## Adobe Experience Platform タグでのDIL 8.0へのアップデート {#updating-to-dil-in-experience-platform-launch}

DIL 8.0へのアップデートの基本的な手順

1. 8.0より前のDILを使用している場合は、アップグレードする前に、AAM拡張機能のDIL設定に移動し、使用している高度なオプション（後続の手順で使用する）をメモします。
1. AAM拡張機能をバージョン 8.0以降に更新します。
1. Experience Cloud ID サービス拡張機能がバージョン 3.3.0以降であることを確認します。
1. 8.0より前のAAM拡張機能またはDILのカスタムコードにある非推奨のメソッド/エレメント（`disableIDSyncs`など）の場合は、ECID拡張機能でECID メソッドを有効にします。

   1. （DIL） `disableDestinationPublishingIframe` -> （ECID） `disableIdSyncs`
   1. （DIL） `disableIDSyncs` -> （ECID） `disableIdSyncs`
   1. （DIL） `iframeAkamaiHTTPS` -> （ECID） `dSyncSSLUseAkamai`
   1. （DIL） `declaredId` -> （ECID） `setCustomerIDs`

1. 変更を公開します。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## Adobe DTMでのDIL 8.0へのアップデート {#updating-to-dil-in-adobe-dtm}

1. AAM ツールをバージョン 8.0以降に更新します。 このバージョン設定は、AAM ツールの「一般」セクションにあります。
1. 8.0より前のAAM ツールのDIL用カスタムコードに含まれていた非推奨のメソッドやエレメント（`disableIDSyncs`など）については、（ECID ツールに追加できるように）そのメソッドをメモし、AAM ツールのカスタム [!DNL DIL code]から削除します。
1. Experience Cloud ID サービス拡張機能をバージョン 3.3.0以降に更新する
1. AAM ツールのカスタムコードから削除したECID ツールに、詳細オプションを追加します。
1. 変更を公開

## Adobe Tag Management ソリューションを使用しないDIL 8.0へのアップデート {#additional-resources}

ページ上で直接コードを更新する場合は、上記のようにDILからECIDにメソッド/エレメントを移動する必要がある場合を除いて、古いアイテムを新しいアイテムに置き換えるだけです。 この場合は、DILの場所の古いmethod/elementを、ECIDの場所のECID method/elementに置き換えるだけです。

Adobeのタグ管理者以外の場合も同様です。 そのタグ管理ソリューションの古いバージョンがある場合は、次の手順に従って、そのバージョンを新しいコードに置き換えます。

1. DIL ライブラリを最新バージョン（8.0以上）に更新する – 現在、公開場所では利用できないため、最新のDIL コードをAdobe ConsultingまたはAdobe カスタマーケアから取得する必要があります。 次に、古いDILライブラリコードを新しいDILライブラリコードに置き換え、次のステップに進みます（今すぐ停止しないでください。または、問題が発生します）。
1. [!DNL ECID Service]をインストールするか、既存のバージョンを3.3.0以降に更新してください。 最新のExperience Cloud ID サービス リリース [は、GitHub ページ &#x200B;](https://github.com/Adobe-Marketing-Cloud/id-service/releases)からダウンロードできます。 この問題についてサポートが必要な場合は、[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)を参照するか、Adobe コンサルタントにお問い合わせください。

1. DILのカスタムコードに含まれている非推奨のメソッドまたはエレメントが、ECID メソッドに移動されていることを確認します。

   1. （DIL） `disableDestinationPublishingIframe` -> （ECID） `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=ja)

   1. （DIL） `disableIDSyncs` -> （ECID） `disableIdSyncs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html?lang=ja)

   1. （DIL） `iframeAkamaiHTTPS` -> （ECID） `idSyncSSLUseAkamai`

      [ドキュメント](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. （DIL） `declaredId` -> （ECID） `setCustomerIDs`

      [ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html?lang=ja)

### 1. 結論

**Universal Editor 前提での EDS AEM Importer は、既存サイトのコンテンツを “EDS 用の構造” に寄せて再取り込みし、UE で扱える AEM 側コンテンツやパッケージを作るための移行・再構造化ツール**として捉えるのが実態です。  
単なる HTML 取り込みではなく、**既存 AEM/Page Editor ベースの資産や外部 CMS のページを、Edge Delivery Services の block モデルへ寄せながら、UE で運用可能な形に持っていくための橋渡し**として使われています。

**重要な前提**として、Slack では一貫して **「既存 AEM コンポーネントをそのまま EDS で使う」のではなく、Edge blocks ベースへ移行するのが推奨** とされています。AEM Importer は、その移行時の **コンテンツ再投入・再編成の主力手段** です。

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Crosswalk - Overview](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Overview)
- [Crosswalk - Documentation](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Documentation)
- [slack thread on converter vs importer](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [slack thread on AEM as author instance for EDS](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)

---

### 2. AEM Importer の機能解説（UE/EDS 前提）

#### 2-1. 何を入力にするか

Wiki では、AEM Importer の基本は **既存 Web ページの DOM を入力**にし、変換ルールを適用して出力を作る方式です。加えて、**複数 URL の bulk import**、**crawl による URL リスト生成**、**ページの inspect** といった支援機能も整理されています。

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Edge Delivery Services](https://wiki.corp.adobe.com/display/PremierSupport/Edge+Delivery+Services)
- [EDS Build Migration Advisory](https://wiki.corp.adobe.com/display/CLOUDSME/%5BEDS%5D+%5BBuild%5D+Migration+Advisory)

#### 2-2. UE 前提だと何を出力するか

Slack / Wiki の両方から見ると、UE 前提では Importer は **従来の `docx` 出力だけではなく、Xwalk content package / JCR package 的な出力**を担う方向で使われています。  
特に Slack では、**UE プロジェクトを UE JSON で認識し、JCR package を生成する**という説明が明示されています。別の Slack では、**既存サイトから生成した JCR content package を Author 環境に入れ、semantic HTML components + Universal Editor で配信できる**という実験・プロトタイプも共有されています。

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [slack thread on importer recognizing UE projects and JCR package output](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1744351915.586459)
- [slack thread on hackathon JCR package prototype](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1709824941.978509)

#### 2-3. 何が “新しめのポイント” か

Slack では、Bulk Importer の更新として **対応コンテンツソースが拡張され、Universal Editor authoring with AEM as a Cloud Service / 6.5 をサポート**したことが案内されています。  
つまり、**UE は Importer の周辺的ユースケースではなく、正式な対応対象として取り込まれつつある**という見方ができます。

**Sources:**

- [slack announcement on Bulk Importer update for UE support](https://adobe.enterprise.slack.com/archives/C0G2SHZD3/p1747347784.293819)

#### 2-4. Importer がやらないこと

Importer は **“既存 AEM コンポーネントをそのまま EDS 互換にする魔法の変換器” ではありません**。  
Slack では、**従来の AEM Sites Page Editor 用コンポーネントは EDS 上ではそのまま動かず、Edge blocks への移行・再実装が必要**と繰り返し説明されています。Importer は主に **コンテンツ移行・再配置・再構造化** の役割です。

**Sources:**

- [slack thread on AEM as author instance for EDS](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)
- [slack thread on EBRD migration scope and importer usage](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1747729141.173039)
- [Crosswalk - FAQ](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+FAQ)

---

### 3. 実務での位置づけ

#### 3-1. Converter の代替・卒業手段

Slack でかなりはっきりしているのは、**Converter approach は非推奨寄り**で、代わりに **AEM Importer で一度または段階的に再インポートする**のが望ましい、という整理です。  
理由はシンプルで、Converter を残すと **AEM components + Edge blocks + converter** の三重管理になりがちだからです。  
そのため、UE 前提の Importer は、**“既存ページを Edge blocks 中心の世界へ移すための one-time / staged migration ツール”** とみるのが実務に近いです。

**Sources:**

- [slack thread on converter vs importer](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [slack thread on hackathon JCR package prototype](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1709824941.978509)
- [Crosswalk - Overview](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Overview)

#### 3-2. “完全自動移行ツール” ではなく、再設計を支えるツール

Wiki の Crosswalk 周辺と Slack の複数スレッドを合わせると、UE/EDS 移行は **コンテンツだけ移せば終わり**ではなく、**frontend の block 化・Git 側設定・AEM 連携・authoring モデル見直し**を含む作業です。  
AEM Importer はその中で、**コンテンツ移行の摩擦を下げる要**ですが、**コード再構築や block 設計そのものを不要にはしない**、という整理が妥当です。

**Sources:**

- [Crosswalk - Documentation](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Documentation)
- [Crosswalk - Overview](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Overview)
- [slack thread on EBRD migration scope and importer usage](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1747729141.173039)
- [slack thread on migration from AEM 6.5 custom components to XWalk/UE](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)

---

### 4. ユースケース整理


| ユースケース                         | Importer の使い方                                          | 向いている状況                                                              |
| ------------------------------ | ------------------------------------------------------ | -------------------------------------------------------------------- |
| 既存 AEM Sites から UE/EDS へ移したい   | 既存ページを再インポートし、UE で扱う新しいコンテンツ構造へ寄せる                     | Page Editor / Core Components / custom components から blocks 中心に移行したい |
| 外部 CMS から AEM + UE + EDS へ移したい | 既存サイト DOM から bulk import / script 生成 / package 化の起点にする | Sitecore, WordPress, legacy CMS などから段階移行したい                          |
| Converter 依存を減らしたい             | one-time import や section-by-section import を行う        | 二重・三重保守を避けたい                                                         |
| 短納期の lift-and-shift をしたい       | content package を作って AEM Cloud に入れる                    | UE で急ぎ移行したいが agentic tooling はまだ使えない                                 |
| PoC / demo / 部分移行をしたい          | 対象ページだけ import し、reference block を作って handover する      | 全部一括ではなく、1ページ・1セクションから始めたい                                           |


**Sources:**

- [slack thread on converter vs importer](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [slack thread on importer recognizing UE projects and JCR package output](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1744351915.586459)
- [slack thread on aggressive lift-and-shift recommendation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770047840.448829)
- [Vitamix - Crosswalk](https://wiki.corp.adobe.com/display/AEMSites/Vitamix+-+Crosswalk)
- [2024-07-11 - Chat about importer use in demos with Chris B](https://wiki.corp.adobe.com/display/AEMSites/2024-07-11+-+Chat+about+importer+use+in+demos+with+Chris+B)

---

### 5. 事例・実例（Slack / Wiki ベース）

#### 5-1. **Vitamix**: 代表的な「部分移行 + handover」型

Wiki の `Vitamix - Crosswalk` は、UE 前提での Importer 活用イメージがかなり具体的です。  
流れは **既存 AEM サイトとコードを共有 → 新しい AEM 環境を用意 → Edge/Crosswalk を立ち上げ → ページの content を import → reference block を 1つ作る → 残りは empty block を準備 → handover して顧客エンジニアが続ける** という形です。

これは、**Importer を “完成品を一括生成するツール” ではなく、“立ち上がりを大きく前進させる移行アクセラレータ” として使う典型例**です。

**Sources:**

- [Vitamix - Crosswalk](https://wiki.corp.adobe.com/display/AEMSites/Vitamix+-+Crosswalk)

#### 5-2. **Experience League**: UE を使う新規/再構築系の大規模例

Wiki の `Crosswalk - Experience League` では、Experience League が **Universal Editor authoring + Edge Delivery Services** の実例として整理されています。  
ここでは **新規マーケティング／ブラウズ系ページを UE で運用しつつ、既存 Markdown 系は別ルートで活かす**というハイブリッドな構成が見えます。  
Importer そのものの詳細事例ページではありませんが、**“すべてを一気に置き換える” のではなく、UE が効く領域から新しい authoring パターンを導入する**という使い方の参考になります。

**Sources:**

- [Crosswalk - Experience League](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Experience+League)
- [Benefits of AEM with Edge Delivery Services (EDS)](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=3091626538)

#### 5-3. **UPS**: eBusiness / commerce 文脈での UE + EDS 実装言及

Slack では、**UPS.com が EDS + UE の実装例**として言及されています。  
詳細な importer フローまでは出ていませんが、**OpenText からの移行の文脈**が示されており、Importer を含む移行系の会話に結びつけやすい実例です。  
特に「既存大規模サイトを段階的に UE/EDS へ寄せる」説明では使いやすい参照です。

**Sources:**

- [slack thread mentioning UPS on EDS with UE](https://adobe.enterprise.slack.com/archives/C0KPLP2CT/p1741804775.618799)

#### 5-4. **Subway 向け議論**: 短納期 bulk migration の推奨パターン

Slack では、Experience Catalyst が UE をまだ十分サポートしないタイミングで、**短納期の lift-and-shift なら AEM Importer tooling で content package を生成して AEM Cloud にアップロードする**という推奨が共有されています。  
これは本番 go-live 事例ではなく **推奨パターン**ですが、**急ぎ案件で Importer が現実的な打ち手になる**ことを示しています。

**Sources:**

- [slack thread on aggressive lift-and-shift recommendation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770047840.448829)

#### 5-5. **プリセールス / デモ用途**

Wiki の会議メモでは、Chris が **customer demo 前に importer を使ってサイトを素早く立ち上げる**ユースケースが共有されています。  
また、**全面移行よりも select content import のほうが cost-effective** な場合がある、という観点も出ています。  
このため Importer は、本番 migration だけでなく **PoC、営業デモ、section pilot** にも向いています。

**Sources:**

- [2024-07-11 - Chat about importer use in demos with Chris B](https://wiki.corp.adobe.com/display/AEMSites/2024-07-11+-+Chat+about+importer+use+in+demos+with+Chris+B)

---

### 6. Universal Editor 前提で押さえるべき注意点

#### 6-1. UE 対応 = 既存 AEM サイトの無変換延命、ではない

Slack では、**UE を使うからといって HTL / classic AEM components がそのまま EDS 化されるわけではない**と明確に説明されています。  
**コンテンツ移行** と **frontend/block 再設計** は切り分けて考える必要があります。

**Sources:**

- [slack thread on AEM as author instance for EDS](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)
- [slack thread on migration from AEM 6.5 custom components to XWalk/UE](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)

#### 6-2. 特に既存 heavy AEM 実装では「import + 再設計」が基本

Slack では、**数百 custom components を抱えた AEM 6.5 実装を UE/XWalk に移すなら、基本は block ベースで再実装**という意見が強いです。  
つまり Importer は **現行アーキをそのまま保存する道具**というより、**アーキ刷新時のコンテンツ移行レイヤ**として使うほうが筋が良いです。

**Sources:**

- [slack thread on migration from AEM 6.5 custom components to XWalk/UE](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)

#### 6-3. UE 用サイトは AEM 内で別セットアップになることが多い

Slack では、Cloud Manager の EDS site 作成は document-based 側で、**UE ベースの WYSIWYG flow は AEM 側で別途セットアップ**する流れが案内されています。  
このため Importer の成果物を受ける先も、**“既存 AEM サイトそのまま” より “UE 用に整えた AEM/EDS プロジェクト”** と考えるほうが実態に近いです。

**Sources:**

- [slack thread on Cloud Manager doc-based vs UE setup](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1744184107.477109)

---

### 7. 実務向けの要約

#### 一言でいうと

**AEM Importer は、Universal Editor を使う EDS プロジェクトにおいて、既存サイトのコンテンツを “UE で編集できる Edge blocks 中心の構造” へ移すための移行アクセラレータ**です。

#### 実務上の理解

- **コンテンツ移行**には強い
- **既存 AEM component の延命**には向かない
- **Converter を減らしたい案件**で価値が高い
- **部分移行・段階移行・PoC** と相性が良い
- **JCR/content package 化**が UE 案件で重要なポイント
- **frontend/block 設計の再実装**は別途必要になりやすい

#### 社内説明向けの短い言い回し

> *“UE 前提の AEM Importer は、既存サイトをそのまま EDS 化するツールではなく、コンテンツを UE/Edge blocks の世界へ再投入するための移行ツール”*

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [slack thread on importer recognizing UE projects and JCR package output](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1744351915.586459)
- [slack announcement on Bulk Importer update for UE support](https://adobe.enterprise.slack.com/archives/C0G2SHZD3/p1747347784.293819)
- [slack thread on converter vs importer](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Vitamix - Crosswalk](https://wiki.corp.adobe.com/display/AEMSites/Vitamix+-+Crosswalk)

### 8. AEM Importer を利用する前提条件

#### 8-1. まず必要な前提理解

AEM Importer は、**既存サイトをそのまま無変換で UE/EDS に載せ替えるツールではなく**、**既存ページのコンテンツを Edge blocks 中心の構造へ再投入するための移行ツール**として使う前提です。  
特に **Universal Editor + Edge Delivery Services** では、**既存 Page Editor 用コンポーネントはそのまま使えず、blocks ベースへ移行する**のが前提になります。

**Sources:**

- [Crosswalk - Demo](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Demo)
- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C03M9AHGK4H/p1741175557.527619)

#### 8-2. ソースが「取り込める状態」であること

Wiki では Importer の基本入力は **既存 Web ページの DOM** です。  
そのため前提として、**取り込み対象のページが HTML/DOM として取得可能**である必要があります。  
また、AOE delivery model 側の記述では、**公開 HTML としてアクセスできること**、**認証/VPN の内側にあるサイトは対象外**、**複雑な動的コンテンツはそのままでは扱いにくい**という整理です。

**Sources:**

- [EDS Build Migration Advisory](https://wiki.corp.adobe.com/display/CLOUDSME/%5BEDS%5D+%5BBuild%5D+Migration+Advisory)
- [Experience Modernization - AOE Delivery Model](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3696778769/Experience+Modernization+-+AOE+Delivery+Model)

#### 8-3. UE/EDS 側の受け皿が先に必要

Slack では、UE with Edge Delivery の開始点として **UE tutorial ベースの正しいセットアップ**、**AEM 側の Edge Delivery Services Configuration**、**repo / site / path の整合**が繰り返し出ています。  
つまり Importer を先に回すより、**受け先の AEM + UE + EDS プロジェクト構成**を用意してから使うのが前提です。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1747729141.173039)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1742221167.896279)

#### 8-4. import script を扱える体制

Wiki の onboarding でも、AEM Importer 利用前提として **import.js のガイド確認**、**custom import.js の試行** が挙がっています。  
現実的には、**完全ノーコードで全案件を処理する前提ではなく、import script を調整できる開発者/実装担当がいること**が重要です。

**Sources:**

- [Import As a Service - Onboarding](https://wiki.corp.adobe.com/display/AEMSites/Import+As+a+Service+-+Onboarding)
- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)

#### 8-5. 大量ページ時はローカル実行前提を避ける

Wiki では、従来の importer UI は **開発者のローカル環境で bulk import を回すとリソース不足やクラッシュが起きやすい**ため、Import as a Service が必要になった経緯が明示されています。  
よって、**数百〜数万ページ規模はローカル一発処理を前提にしない**ほうが安全です。

**Sources:**

- [Import - UI Proposals](https://wiki.corp.adobe.com/display/AEMSites/Import+-+UI+Proposals)
- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)

---

### 9. 気をつけるべきポイント

#### 9-1. 「Importer がある = 自動移行できる」ではない

Wiki / Slack の両方で、**すべての案件を自動化できるわけではない**、**AEM Importer はある程度のところまで助けるが、全案件を push-button 化するものではない**という前提が出ています。  
特に複雑な案件では、**import script・block mapping・frontend 再実装**が残ります。

**Sources:**

- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)
- [2024-03-21 - Chat 'Import as a Service' with Gillian](https://wiki.corp.adobe.com/display/AEMSites/2024-03-21+-+Chat+%27Import+as+a+Service%27+with+Gillian)

#### 9-2. 既存 AEM コンポーネント資産が多いほど難易度が上がる

AEM 6.5 / heavy custom components の Slack 議論では、**数百コンポーネント規模を UE/XWalk に持っていくなら、既存資産の延命ではなく block ベースの再設計が現実的**とされています。  
**JCR 依存、Java API 依存、複雑なテンプレート構造**が強いほど、Importer だけで救えません。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)
- [Crosswalk - Demo](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Demo)

#### 9-3. source HTML の癖に引っ張られる

Wiki の importer exercise では、**redirect によって期待と異なる page naming になる**、**不要 DOM を除去しないとノイズが多い**、**metadata の扱いが素直ではない** など、実際の import 調整ポイントが出ています。  
要するに、**source site の HTML がきれいとは限らず、最初の数ページでルール調整する前提**が必要です。

**Sources:**

- [Import - implementationdetails.dev](https://wiki.corp.adobe.com/display/AEMSites/Import+-+implementationdetails.dev)

#### 9-4. スタイルは別問題

Wiki の deeper dive では、**importer は現在スタイル抽出を主目的にしていない**と明示されています。  
Slack の JCR package 話でも、**styles は package に含めず customer の GitHub project 側で管理**とされています。  
つまり、**コンテンツ移行と見た目再現は別タスク**です。

**Sources:**

- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1709824941.978509)

#### 9-5. 既存 AEM サイトからの段階移行では URL / CDN / martech の整理が必要

Slack では、ページ単位の段階移行は可能だが、**ページコピー戦略**、**CDN で旧 origin / EDS origin を分ける設計**、**analytics / martech script の両対応**が必要とされています。  
Importer はコンテンツ側の補助であって、**配信切替や計測設計まで面倒を見るわけではない**点に注意です。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1741156419.837679)

#### 9-6. UE / EDS の運用条件も別途確認が必要

たとえば Slack では、**Forms 機能は UE で使うなら Forms license が必要**、**Marketo は EDS Forms では不可で AEM Forms CS with UE なら可**、**multifield は未サポート**など、Importer 以前に UE/EDS の制約があります。  
Importer で content を持ち込めても、**最終的な authoring 要件を UE が満たすか**は別チェックが必要です。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C014Z066Z0R/p1761214671.618729)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C09TJBA3G/p1759865468.640079)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1753419388.827179)

---

### 10. プロジェクトごとのカスタマイズ

#### 10-1. 基本は `import.js` / transformation ベース

Wiki の onboarding と capabilities から、AEM Importer のプロジェクト差分は主に **transformation / import script** で吸収します。  
何を除外するか、DOM のどこを block にマップするか、metadata をどう構築するか、といった部分が案件ごとの作り込みになります。

**Sources:**

- [Import As a Service - Onboarding](https://wiki.corp.adobe.com/display/AEMSites/Import+As+a+Service+-+Onboarding)
- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Import - implementationdetails.dev](https://wiki.corp.adobe.com/display/AEMSites/Import+-+implementationdetails.dev)

#### 10-2. block mapping は案件ごとの差が大きい

Wiki の deeper dive では、**generic parser を block ごとに作る**、**manual section mapping / blueprint detector** といった話が出ています。  
つまり、案件ごとの差はほぼ **「どの source pattern をどの block に落とすか」** に集約されます。  
共通化は進められていますが、**結局はサイトごとの DOM パターン差**が大きいです。

**Sources:**

- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)
- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)

#### 10-3. AEM source の場合は core components mapping の恩恵がある

Wiki では、AEM source 専用に **Core Components → Edge blocks** のマッピング最適化アイデアが整理されています。  
Title → Heading、Tabs → Tabs、Accordion → Accordion など、**既存 AEM の規律が保たれているほど変換戦略を組みやすい**です。  
逆に **custom blocks / custom components は special handling** が必要です。

**Sources:**

- [Import - AEM with Core Components](https://wiki.corp.adobe.com/display/AEMSites/Import+-+AEM+with+Core+Components)

#### 10-4. 外部 CMS は ETL 色が強くなる

Slack では、外部 CMS → AEM + UE + EDS では **CTT/CAM ではなく、軽量 ETL script + Bulk Import + Importer** の組み合わせが現実的とされています。  
つまり、**ソースが AEM か、WordPress/Sitecore/その他 CMS か**でカスタマイズの重さはかなり変わります。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C01ERV08CES/p1747855587.277849)

#### 10-5. UE project recognition / JCR package 化も案件依存

Slack では、Importer が **UE JSON を見て UE project と認識し、JCR package 生成オプションを出す**という説明があります。  
このため、**UE 用 repository / config / site structure が正しく作られていること**自体が、Importer の成果物を左右します。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1744351915.586459)

#### 10-6. 生成後も block 側設定が必要

Crosswalk demo でも、UE で block を扱うには `**component-models.json` / `component-definition.json` / `component-filters.json`** の整備が必要です。  
Importer で content を入れても、**authoring 面の block 定義まで自動で完成するとは限らない**ため、プロジェクトごとの block モデル整備は残ります。

**Sources:**

- [Crosswalk - Demo](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Demo)

---

### 11. AEM Importer でできること / できないこと

#### 11-1. できること

##### できること1: 既存ページの DOM を入力にコンテンツを抽出する

- 単一ページ import
- 複数 URL の bulk import
- crawl による URL リスト生成
- inspect によるロゴ/色/フォント把握

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)

##### できること2: transformations で不要要素を除外し、block 向けに整形する

- nav / footer / dialogs / hidden elements などの除外
- DOM の並び替え、cleanup
- metadata の組み立て
- block mapping の調整

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Import - implementationdetails.dev](https://wiki.corp.adobe.com/display/AEMSites/Import+-+implementationdetails.dev)

##### できること3: UE/EDS 向けの content package を作る

- `docx`
- Xwalk content package
- UE project 向け JCR package

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1744351915.586459)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1709824941.978509)

##### できること4: section 単位・段階移行の支援

Slack / Wiki ともに、**one-time import** や **section-by-section reimport** の用途が強く出ています。  
大規模サイトでも、**部分移行や pilot 移行**の形で使えます。

**Sources:**

- [Vitamix - Crosswalk](https://wiki.corp.adobe.com/display/AEMSites/Vitamix+-+Crosswalk)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1741156419.837679)

##### できること5: 急ぎ案件の bulk lift-and-shift の暫定打ち手

Experience Catalyst がまだ使えない場面では、Slack で **Importer tooling + content package upload** が推奨されています。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770047840.448829)

---

#### 11-2. できないこと / 過信しないほうがよいこと

##### できないこと1: 既存 AEM コンポーネントをそのまま EDS 互換にすること

classic AEM templates / components は、そのままでは EDS で動きません。  
**block ベースへの移行または再実装**が必要です。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)

##### できないこと2: frontend 再実装を不要にすること

Importer は content migration を助けますが、**HTML/CSS/JS の block 化や frontend rebuild を不要にはしません**。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1747729141.173039)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C03M9AHGK4H/p1741175557.527619)

##### できないこと3: 複雑な backend / server-side ロジックをそのまま持ち込むこと

Slack では、AEM サービス呼び出しなどの business logic は **HTTP API 化して client side / edge worker / serverless に寄せる**方向が推奨されています。  
Importer 自体は、こうした backend behavior を移植しません。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)

##### できないこと4: source site の見た目を完全再現すること

スタイル抽出や brand 維持は roadmap / opportunity としてはありますが、現時点では **content import と style fidelity は別作業**です。

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [2024-04-30 - Deeper dive of AEM Importer with Alex and David](https://wiki.corp.adobe.com/display/AEMSites/2024-04-30+-+Deeper+dive+of+AEM+Importer+with+Alex+and+David)

##### できないこと5: すべての authoring 要件を満たす保証

UE/EDS 側には、少なくとも現時点で

- multifield 未サポート
- RTE 制約
- 一部 forms/integration 制約
- classic Content Fragment component 相当の再利用不可  
などの限界があります。Importer はこれを解決しません。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1753419388.827179)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770680573.898719)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770127154.749539)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C09TJBA3G/p1759865468.640079)

##### できないこと6: local UE workflow をそのまま Crosswalk に当てはめること

Slack では **local Universal Editor は Crosswalk ではサポートされず、必要でもない**とされています。  
ローカル開発戦略は Edge Delivery のやり方に合わせて考える必要があります。

**Sources:**

- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1742889153.030279)

---

### 12. 実務向けの追加まとめ

#### AEM Importer を使う前に確認すべきこと

1. **移行先は UE + EDS の正しい構成になっているか**
2. **既存コンポーネントを延命したいのか、blocks に寄せたいのか**
3. **source HTML は取得できるか**
4. **import script を触れる体制があるか**
5. **style / martech / URL / CDN は別タスクとして見積もっているか**
6. **UE 側の制約で authoring 要件を落とさないか**

#### 一言でいうと

> **AEM Importer は “移行の加速装置” であって、“既存 AEM 実装の完全自動変換機” ではない**。

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Import As a Service - Onboarding](https://wiki.corp.adobe.com/display/AEMSites/Import+As+a+Service+-+Onboarding)
- [2024-03-21 - Chat 'Import as a Service' with Gillian](https://wiki.corp.adobe.com/display/AEMSites/2024-03-21+-+Chat+%27Import+as+a+Service%27+with+Gillian)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)


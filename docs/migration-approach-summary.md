### Slide 1 — 何をどう位置づけるか

#### タイトル

**AEM Importer は “既存サイトを UE/EDS に寄せるための移行アクセラレータ”**

#### 伝えたいこと

- **Universal Editor + Edge Delivery Services** では、既存 AEM Page Editor 用コンポーネントはそのまま使わず、**Edge blocks 前提**に寄せるのが基本
- **AEM Importer** は、既存サイトの HTML/DOM や既存 AEM コンテンツを、**UE で扱える構造へ再投入するための移行ツール**
- 役割は **コンテンツ移行の加速** であって、**frontend 再実装を不要にする魔法の変換機ではない**
- 社内的には、**Converter より Importer / UE straight-way** の推奨が強い

#### 一言メッセージ

> **“既存実装を温存する” のではなく、 “コンテンツを再利用しながら Edge blocks に移る” のが本筋**

#### 話し方メモ

- 既存 AEM 顧客には「全部捨てる話」ではなく、**コンテンツ資産を活かしながら delivery / authoring モデルを刷新する話**として説明
- UE は新しい editor、Importer はその移行を速める手段、と切り分ける
- まず **content migration** と **code modernization** は別だと明確にする

**Sources:**

- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [Crosswalk - Demo](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Demo)
- [Import - AEM Importer Next](https://wiki.corp.adobe.com/display/AEMSites/Import+-+AEM+Importer+Next)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1745439445.490419)

---

### Slide 2 — どんな案件で刺さるか

#### タイトル

**AEM Importer が向く案件 / 向かない案件**

#### 向く案件

- **既存 AEM Sites → UE/EDS** に移したい
- **Converter の二重保守を避けたい**
- **部分移行 / section-by-section 移行**で始めたい
- **短納期の lift-and-shift** でまず形にしたい
- **PoC / demo / pilot** で既存サイトの一部を早く見せたい
- **外部 CMS → AEM + UE + EDS** で、まず content migration の起点が欲しい

#### 向きにくい案件

- **既存 AEM コンポーネントをそのまま延命したい**
- **Java / JCR / server-side rendering 依存が強い**
- **frontend 再実装なしで UE/EDS に行きたい**
- **複雑な authoring 要件を今すぐ全部満たしたい**
- **完全自動・ノーコード移行を期待している**

#### 一言メッセージ

> **Importer は “移行の摩擦を減らす” が、“移行そのものをゼロコストにはしない”**

#### 話し方メモ

- 営業向けには「**time-to-value を前倒しする**」
- SE向けには「**block mapping / import script / package 化で初速を作る**」
- 顧客向けには「**big bang ではなく、部分移行も可能**」

**Sources:**

- [Vitamix - Crosswalk](https://wiki.corp.adobe.com/display/AEMSites/Vitamix+-+Crosswalk)
- [2024-07-11 - Chat about importer use in demos with Chris B](https://wiki.corp.adobe.com/display/AEMSites/2024-07-11+-+Chat+about+importer+use+in+demos+with+Chris+B)
- [2024-03-21 - Chat 'Import as a Service' with Gillian](https://wiki.corp.adobe.com/display/AEMSites/2024-03-21+-+Chat+%27Import+as+a+Service%27+with+Gillian)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770047840.448829)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1752501159.698269)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1741156419.837679)

---

### Slide 3 — 提案時の注意点と推奨トーク

#### タイトル

**提案時に外してはいけない注意点**

#### 先に言うべきこと

- **既存コンポーネントはそのまま EDS で動かない**
- **content migration と frontend rebuild は別作業**
- **styles / martech / CDN / URL routing は別タスク**
- **UE 側にも制約がある**  
例: multifield 制約、RTE 制約、一部 forms/integration 制約
- **大量ページはローカル一括 import 前提にしない**

#### 推奨トーク

- **新規 or 再設計案件**  
→ *UE + blocks を straight に採る。Importer は content migration 用*
- **既存 AEM 大規模案件**  
→ *段階移行で一部を Importer で再投入、block ベースに寄せる*
- **短納期案件**  
→ *Importer で package を作り、まず動く範囲を先に作る*
- **複雑案件 / 共同実装したい案件**  
→ *Experience Catalyst / VIP / co-innovation を検討*

#### 一言メッセージ

> **“Importer は最短距離を作るが、設計判断までは代行しない”**

#### クロージング用の一文

> **おすすめは、最初に 1ページまたは 1セクションで importer を使って block mapping と authoring fit を検証し、その後に本格展開する進め方です。**

**Sources:**

- [Import - UI Proposals](https://wiki.corp.adobe.com/display/AEMSites/Import+-+UI+Proposals)
- [Import As a Service - Onboarding](https://wiki.corp.adobe.com/display/AEMSites/Import+As+a+Service+-+Onboarding)
- [Import - implementationdetails.dev](https://wiki.corp.adobe.com/display/AEMSites/Import+-+implementationdetails.dev)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1753419388.827179)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770680573.898719)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C014Z066Z0R/p1761214671.618729)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1742889153.030279)


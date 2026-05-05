### 比較表


| 観点               | Converter                                           | AEM Importer                                      | Experience Catalyst                           |
| ---------------- | --------------------------------------------------- | ------------------------------------------------- | --------------------------------------------- |
| 主な役割             | 既存 AEM 出力を EDS 向け semantic HTML に変換                 | 既存サイト/既存コンテンツを UE/EDS 向け構造へ再投入                    | AI/agentic を含む、より広い移行自動化と delivery 支援         |
| 基本思想             | 既存実装を活かしつつ delivery を Edge 化                        | コンテンツを活かしつつ authoring / block モデルを刷新              | 移行作業全体を Adobe 側支援込みで加速                        |
| 向く案件             | 既存 AEM を短期で Edge delivery につなぎたい暫定ケース               | UE/EDS に寄せたい案件、部分移行、段階移行、PoC                      | 複雑案件、大規模案件、co-innovation 前提案件                 |
| authoring モデル    | Page Editor 継続寄り                                    | UE 前提で使いやすい                                       | UE / DA を含む広い移行支援                             |
| frontend         | AEM components + EDS blocks + converter の多層構成になりやすい | blocks 前提へ寄せやすい                                   | 最終的には blocks/EDS モデルへ寄せる前提                    |
| コンテンツ移行          | 弱い / 別手段が必要                                         | 強い。import script, bulk import, package 化          | 強いが availability /対応範囲は時期依存                   |
| JCR / package 生成 | 主眼ではない                                              | UE project 認識 + JCR package 生成の流れあり               | 案件支援の中で扱えるが self-serve 前提ではない                 |
| 自動化レベル           | 低い                                                  | 中程度。script / mapping 調整が前提                        | 高めを志向。ただし機能成熟度と対象範囲は確認要                       |
| プロジェクト別カスタマイズ    | converter と block の両方で調整                            | import.js / block mapping / metadata / package 調整 | Agent / workflow 側で吸収を目指すが完全自動ではない            |
| メリット             | 既存 authoring を残しやすい                                 | 推奨アーキに近づけやすい、段階移行しやすい                             | Adobe 支援込みで加速しやすい                             |
| デメリット            | 二重・三重保守、開発体験が悪くなりやすい                                | frontend 再実装は残る、案件ごとの script 調整が必要                | まだ heavy development / availability 制約がある場面あり |
| 現在の社内推奨感         | 非推奨寄り                                               | 実務上の主力                                            | 有望だが案件条件とタイミング次第                              |
| 一言で言うと           | **延命策**                                             | **現実的な移行アクセラレータ**                                 | **次世代の包括的移行支援**                               |


### ざっくり推奨

#### 1. 新規サイト / 再設計前提

**AEM Importer か、可能なら Experience Catalyst を優先**  
Converter は基本避ける

#### 2. 既存 AEM 大規模案件

**まず 1セクション単位で Importer**  
Converter は「暫定」「例外」扱いに留める

#### 3. 短納期でまず形にしたい

**Importer が最も現実的**  
Experience Catalyst は availability を確認

#### 4. 顧客が “既存 AEM コンポーネントをそのまま使いたい” と言う場合

**期待値調整が必要**  
その願いに最も近いのは Converter だが、長期的には保守負債になりやすい

**Sources:**

- [Import - AEM Importer Next](https://wiki.corp.adobe.com/display/AEMSites/Import+-+AEM+Importer+Next)
- [Importer - Capabilities](https://wiki.corp.adobe.com/display/AEMSites/Importer+-+Capabilities)
- [2024-03-21 - Chat 'Import as a Service' with Gillian](https://wiki.corp.adobe.com/display/AEMSites/2024-03-21+-+Chat+%27Import+as+a+Service%27+with+Gillian)
- [Crosswalk - Demo](https://wiki.corp.adobe.com/display/AEMSites/Crosswalk+-+Demo)
- [Experience Modernization - AOE Delivery Model](https://wiki.corp.adobe.com/spaces/AEMSites/pages/3696778769/Experience+Modernization+-+AOE+Delivery+Model)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C061MH1RX42/p1715226686.645709)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C05QU7MMRNF/p1770047840.448829)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C0G2SHZD3/p1747347784.293819)
- [Slack conversation](https://adobe.enterprise.slack.com/archives/C053BCZD37G/p1725239500.809529)

### 営業/SE向けの短い言い分け

- **Converter**  
*“既存実装をなるべく触らずに Edge 化したい時の暫定策”*
- **AEM Importer**  
*“既存コンテンツを活かして UE/EDS に寄せる、今いちばん現実的な移行手段”*
- **Experience Catalyst**  
*“より広い移行自動化を狙う本命だが、案件条件と対応状況を見て使う”*


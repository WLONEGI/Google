# Google Cloud Japan 面接対策

## ポジション

**フォワード デプロイド エンジニア、生成 AI、Google Cloud（日本語・英語）**
Tokyo / 中級（L4〜L5相当）

> エンタープライズ顧客の環境に埋め込まれ、GoogleのAI製品を使って本番グレードのシステムを実際に構築・デプロイする「現場ビルダー」ロール。

---

## 選考スケジュール

| ステップ | 日程 | ステータス |
|---|---|---|
| 書類選考 | 完了 | ✅ 通過済み |
| リクルータースクリーニング（20分） | 6月30日 | 🔥 直近 |
| テクニカル面接（4〜5ラウンド） | TBD | ⏳ 待機 |
| チームマッチング | TBD | ⏳ 待機 |

所要期間の目安：応募〜内定まで **6〜8週間**

---

## 自分の現状と最大の課題

### 強み
- ML/AIエンジニアとしてのベース（Python・モデル知識・AI的思考）がそのまま活きる
- 生成AI・LLMの技術的な知見がある

### ギャップ
- 求人が求める「エンタープライズ本番デプロイ経験」に対し、現状はPoC/社内ツールレベル

### ギャップへの対応戦略

FDEの役割は「PoC止まりのAIを本番に引き上げる専門家」。この視点でギャップを逆手に取る：

> 「PoC〜社内展開を経験し、本番移行を阻む壁（データ品質・状態管理・評価・セキュリティ境界）を痛感した。その壁を顧客と一緒に突破するのがFDEであり、これが応募の理由」

弱点を志望動機に変えるフレームとして一貫して使う。

---

## ラウンド別優先度

| 優先度 | ラウンド | フォルダ |
|---|---|---|
| ★★★ | リクルータースクリーニング | `01_recruiter_screen/` |
| ★★★ | Agentic & ML システムデザイン | `04_agentic_ml_system_design/` |
| ★★ | Googleyness（行動面接） | `05_googleyness/` |
| ★★ | Vibe Coding | `03_vibe_coding/` |
| ★ | コーディング & アルゴリズム | `02_coding_algorithms/` |
| ★ | 英語面接練習（全ラウンド並行） | `06_english_practice/` |

---

## フォルダ構成

```
Google/
├── README.md                        ← 本ファイル（全体戦略）
├── 01_recruiter_screen/             ← 6/30スクリーニング対策（最優先）
├── 02_coding_algorithms/            ← DSA・コーディング対策
├── 03_vibe_coding/                  ← 曖昧な要件での実装練習
├── 04_agentic_ml_system_design/     ← RAG・マルチエージェント設計対策
├── 05_googleyness/                  ← 行動面接・STARストーリーバンク
└── 06_english_practice/             ← 英語面接練習（各ラウンド対応）
```

---

## 今週の集中ポイント（6/24〜6/30）

6/30のスクリーニング突破が最優先。ここで落ちると以降の準備がすべて無意味になる。

- [ ] 志望動機（Why FDE / Why Google Cloud / Why Now）を60秒で言えるようにする
- [ ] AIプロジェクトストーリー1本を構造化する（PoC経験でも本番移行の難しさが語れるもの）
- [ ] 自己紹介30秒（日本語・英語両方）
- [ ] 逆質問2つ用意する

詳細は `01_recruiter_screen/` を参照。

---

## 求人要件（参照用）

### 必須
- STEM学士号または同等の実務経験
- Python および ML パッケージ（Keras、PyTorch、HF Transformers）2年以上
- 事前学習済みモデルを中心としたAIシステム構築（プロンプトエンジニアリング・ファインチューニング・RAG・エージェント）
- GCPでのソリューション設計・デプロイ・管理経験
- 日本語・英語による高い対話能力

### 歓迎
- AI/CS関連の修士・博士号
- マルチエージェントシステムの実装経験（LangGraph・CrewAI・Google ADK、ReAct・自己分析・階層的委任などのパターン）
- LLMネイティブ指標（トークン/秒・リクエストあたりコスト）の最適化手法への精通

---

## 主な参考リソース

- [求人票](https://www.google.com/about/careers/applications/jobs/results/106853610842661574)
- [Google FDE Interview Guide - Exponent](https://www.tryexponent.com/guides/google-forward-deployed-engineer-interview)
- [Google Careers - 採用プロセス](https://www.google.com/about/careers/applications/how-we-hire)

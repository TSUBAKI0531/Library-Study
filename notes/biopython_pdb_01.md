# Biopython Bio.PDB 学習ノート #01：SMCRA構造の理解と基本操作

## 1. Bio.PDBの概要

Bio.PDBは、PDBやmmCIFなどの複雑な3D構造データを、Pythonのオブジェクトとして直感的に操作可能にするモジュールである。最大の特徴は、階層的なデータ管理モデル（SMCRA構造）を採用している点にある。


## 2. SMCRA構造（タンパク質の住所システム）

Bio.PDBは、以下の5つの階層で構造を保持する。これを**SMCRA構造**と呼ぶ。

**Structure (S)**: 構造全体。最上位コンテナ。

**Model (M)**: 空間モデル。結晶構造なら通常1つ（ID 0）、NMRなら複数の揺らぎが含まれる。

**Chain (C)**: ポリペプチド鎖。

**Residue (R)**: アミノ酸残基。IDは `(hetfield, resseq, icode)` のタプル形式。

**icode（挿入コード）**: 歴史的なナンバリングを維持するための仕組み。

**Atom (A)**: 原子。座標データ（XYZ）を保持する最小単位。


## 3. PDBList：構造データのオンライン取得

解析したいPDB IDを指定して、RCSB PDBなどのデータベースから直接ファイルをダウンロードする。

```python
from Bio.PDB import PDBList

pdbl = PDBList()
# '1FAT' の構造データを取得し、カレントディレクトリに保存
pdbl.retrieve_pdb_file('1FAT', pdir='.', file_format='pdb')

```

## 4. 最小構成の実装コード（Chain Aの残基名抽出）

ダウンロードしたファイル（または手元のファイル）を解析する最短ステップ。

```python
from Bio.PDB.PDBParser import PDBParser

# 1. パーサーの作成
parser = PDBParser()

# 2. PDBファイルの読み込み
structure = parser.get_structure("sample_id", "1fat.pdb")

# 3. Model 0 の Chain A を取得（ショートカット利用）
chain_a = structure[0]["A"] 

# 4. 残基をループで回し、残基名を出力
for residue in chain_a:
    print(residue.get_resname())

```

## 5. 実践：構造解析のフルワークフロー

Bio.PDBを用いた標準的な解析の流れは以下の通り。

1. **Fetch**: `PDBList` でデータをオンライン取得。

2. **Parse**: `PDBParser` や `MMCIFParser` でSMCRA階層へ変換。

3. **Traverse**: `Selection.unfold_entities` などで対象のオブジェクトを抽出。

4. **Analyze**: `Superimposer` や `NeighborSearch` などで幾何学計算や比較を実施。

5. **Output**: `PDBIO` を用いて解析結果をファイルに保存。


---

### 💡 本日の振り返り

NotebookLMの音声で聞いた「住所システム（連番を振り直さない icode の重要性）」や「Disorder の隠蔽」といった概念が、実際のコード（`get_resname()` や `structure[0]["A"]`）と結びついた。

---

ノートの作成、お疲れ様です！これで第1週（5/4）の到達点である「PDBファイルを読み込み、チェーン・残基を出力するスクリプト」の基盤が完璧に整いました。

このノートを GitHub に保存したら、本日の学習は完了ですね。来週の「Week 2：ミニスクリプトの実装」で、実際にコードを動かすのが楽しみです。

何か他にノートに付け加えたいことや、来週に向けて確認しておきたいことはありますか？

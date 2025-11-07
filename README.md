kikikikkikikikkkikikikikikkikikikikikikikikikikikikolololololololololololp;pp;p;p;@aqswdedfrgthyhujukikolp;;


int32からHEXに変換してからarm64命令に変えるやつ
AI作なのでMITライセンス

## 使い方
`` <script src="https://raw.githubusercontent.com/kiyu4776/arm64/main/js/arm-tool.js"></script> ``か
`` <script src="https://raw.githubusercontent.com/kiyu4776/arm64/main/js/arm-tool-lite.js"></script> ``

liteは改行コメントなし

https://kiyu4776.github.io/arm64/

## サポートされてるやつ

以下がこのARM64 Instruction Toolでサポートされている命令の一覧です：

算術演算命令
ADD (即値、シフト済みレジスタ)

ADDS (シフト済みレジスタ)

SUB (即値、シフト済みレジスタ)

SUBS (シフト済みレジスタ)

CMP (即値、シフト済みレジスタ) - SUBSのエイリアス

論理演算命令
AND (即値、シフト済みレジスタ)

ORR (シフト済みレジスタ)

EOR (シフト済みレジスタ)

データ転送命令
MOV (ワイド即値、レジスタ)

LDR (即値オフセット)

STR (即値オフセット)

LDP (ペアロード)

STP (ペアストア)

LDRB, LDRH, LDRSB, LDRSH (バイト/ハーフワードロード)

分岐命令
B (無条件分岐)

BL (リンク付き分岐)

BR (レジスタ分岐)

BLR (リンク付きレジスタ分岐)

B.cond (条件分岐)

CBZ (ゼロ比較分岐)

CBNZ (非ゼロ比較分岐)

TBZ (ビットテストゼロ分岐)

TBNZ (ビットテスト非ゼロ分岐)

特殊命令
NOP (ノーオペレーション)

RET (リターン)

SVC (スーパーバイザコール)

HLT (ハルト)

アドレス計算命令
ADR (PC相対アドレス計算)

ADRP (PC相対ページアドレス計算)

シフト命令
LSL (論理左シフト)

レジスタ指定
32ビットレジスタ: w0-w30, wsp

64ビットレジスタ: x0-x30, sp, xzr

スタックポインタ: sp (x31), wsp (w31)

ゼロレジスタ: xzr (x31)

対応する命令形式
即値演算 (12ビット即値、オプションで12ビットシフト)

レジスタ演算 (シフト付き、シフトなし)

メモリ操作 (符号なしオフセット、プレ/ポストインデックス)

分岐 (相対オフセット、レジスタ間接)

条件分岐 (各種条件コード対応)

エンコーディング/デコーディング機能
Int32 ↔ HEX ↔ ARM64命令 の相互変換

リトルエンディアン/ビッグエンディアン対応

命令のエンコード/デコード処理

このツールはARM64アセンブリの学習、デバッグ、リバースエンジニアリングに役立つように設計されています。主要なARM64命令の大部分をカバーしており、実用的な使用が可能です。

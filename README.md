kikikikkikikikkkikikikikikkikikikikikikikikikikikikolololololololololololp;pp;p;p;@aqswdedfrgthyhujukikolp;;


int32からHEXに変換してからarm64命令に変えるやつ
AI作なのでMITライセンス

## 使い方
`` <script src="https://raw.githubusercontent.com/kiyu4776/arm64/main/js/arm-tool.js"></script> ``か
`` <script src="https://raw.githubusercontent.com/kiyu4776/arm64/main/js/arm-tool-lite.js"></script> ``

liteは改行コメントなし

https://kiyu4776.github.io/arm64/

## サポートされてるやつ
ざっくり説明

この arm-tool-lite.js は ARM64 のごく一部の命令をデコード（Hex/Int → アセンブリ）／エンコード（アセンブリ → 32bit エンコーディング） する軽量なツールだよ。
主な機能は次の通り：

int32ToHex(value) — 32bit 整数 → 0x付き16進（ゼロ埋め）

hexToInt32(hex) — 0x16進 → 符号付き 32bit 整数

reverseBytes(value) — 32bit 値のバイト順を反転（LE/BE 切替に使う）

decodeArm64(value, littleEndian) — 32bit の値を見て可能な限りアセンブリ文字列を返す（分からなければ 'Unknown instruction'）

encodeArm64(instruction, littleEndian) — サポートしているアセンブリ命令を 32bit エンコーディングに変換（対応外なら例外を投げる）

processInput() — HTML 上の入力欄を読むワンクリック実行用の関数（スニペットに合わせた UI を想定）

サポートされている命令（主なもの）

※完全網羅ではない。コードを見た範囲で対応している命令：

nop

adr / adrp

ret

add / adds（レジスタ操作、即値シフト付き）

sub / subs

mov（レジスタからレジスタ / immediate (mov #imm16 << hw)）

ldr/str（いくつかのロード・ストアフォーマット）

ldp/stp

b / bl / b.cond

cbz / cbnz

tbz / tbnz

br / blr

svc, hlt

cmp

and/orr/eor/and immediate forms（ある程度）

lsl（左シフト immediate）

その他に「Unknown」判定するパターンもいくつかある

短所：命令セットは限定的で、浮動小数点や複雑な拡張命令、SIMD、メモリのあらゆるバリエーションは未対応。完全な逆アセンブラではない点に注意してね。

リトルエンディアンについて

littleEndian を true にすると、エンコード／デコードの際に reverseBytes を適用してバイト順を反転する（つまり入力/出力が LE フォーマットのときに使う）。ARM64 の実際のメモリ表現が little-endian なら littleEndian = true を使えば OK。

使い方 — すぐ試せる HTML（そのまま保存してブラウザで開ける）

以下の HTML に arm-tool-lite.js を同じディレクトリに置いて使って。processInput() が既存の関数を呼び出すよ。

<!doctype html>
<html lang="ja">
<head>
  <meta charset="utf-8" />
  <title>arm-tool-lite デモ</title>
  <style>
    body{font-family:system-ui, sans-serif; padding:16px}
    textarea{width:100%;height:120px}
    pre{background:#111;color:#eee;padding:12px;border-radius:6px}
  </style>
</head>
<body>
  <h1>arm-tool-lite デモ</h1>
  <p>数値、16進、またはアセンブリを入力して「実行」</p>

  <label>入力（例: 0xd503201f /  -788462593 / mov x0, x1 / add x0, x1, x2）</label>
  <textarea id="input">0xd503201f</textarea><br/>

  <label><input type="checkbox" id="littleEndian" /> 入力はリトルエンディアン（バイト反転）</label><br/>

  <button onclick="processInput()">実行</button>

  <h3>出力</h3>
  <pre id="output"></pre>

  <script src="arm-tool-lite.js"></script>
</body>
</html>

使い方例（ブラウザのコンソールで直接呼ぶ）

スクリプトを読み込んでいれば、DevTools のコンソールで直接呼べる：

// デコード（NOP）
decodeArm64(0xd503201f, false)        // -> "nop"

// デコード（リトルエンディアン表現がファイルにある場合）
decodeArm64(0xd503201f, true)         // バイトを反転して解釈（状況により使い分け）

// エンコード
let enc = encodeArm64('mov x0, x1', false); // -> 32bit 数値
int32ToHex(enc)                             // -> "0x..." 表示

// 16進文字列を正しく符号付き int にする
hexToInt32('0xd503201f')  // -> 符号付き整数に変換


注意：encodeArm64 はサポートしてない命令を投げる（例外）ので try/catch で扱ってね。

Node.js で使いたい場合

このスクリプトはモジュール化されていない（グローバル関数を想定）。手早く Node で使うなら少しだけラップするか、単に HTML とブラウザで試すのが手っ取り早い。もし Node 用に変換してほしければ、私が module.exports でラップしたバージョンを作るから言って。

出力例（具体的）

入力：0xd503201f → decode → nop

入力：mov x0, x1 → encode → 例: 0xaa0103e0（例は仮。実際は int32ToHex(encodeArm64(...)) で確認）

入力：add x0, x1, x2 → encode → 32bit value、int32ToHex で16進表示

（実数値はその場で実行すると確実。ここで示したのは概念的な例。）

注意点（安全性と信頼性）

このスニペットは 教育・補助目的の軽量ツール。完全なディスアセンブルや全命令サポートは期待しないで。

正確さ：多くの命令に対してビットチェックをしているが、未対応命令は 'Unknown instruction' や例外になる。

実機でバイナリを書き換えるなど危険な操作をする前に、出力が本当に正しいか別のツール（objdump、capstone、radare2 など）で検証することを強く勧める。

次に何が欲しい？

このまま ブラウザUI を整えたHTMLファイル を作る？（出力整形／バイト表示などを追加）

ある具体的な HEX（例：0xXXXXXXXX）をこれでデコードしてほしい？ → ここに貼ってくれれば即解析する。

Node 用に モジュール化 してコマンドラインで encode/decode できるようにする？

やりたいことを教えて。分からない点は「分からない」と正直に言うので、気軽にどうぞ。

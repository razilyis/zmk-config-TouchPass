# TouchPass for Zmk

> [!NOTE]
> 本プロジェクトは、[tobiasweissert/TouchPass](https://github.com/tobiasweissert/TouchPass) の素晴らしい実装を ZMK 向けにポーティングしたものです。

指紋センサーを使用してパスワードを管理・入力するための、XIAO nRF52840 をベースとした ZMK ファームウェアプロジェクトです。

## 概要
- **指紋認証**: R502-A シリーズの指紋センサーを使用。
- **常時待機**: 指をセンサーに置くだけで認証し、登録されたパスワードを自動入力します（設定で変更可能）。
- **ウェブ設定ツール**: `config.html` を通じてブラウザから指紋登録やパスワード設定が可能。

## 配線 (Wiring)
XIAO nRF52840 と R502-A センサーを以下のように接続します。**3.3V** 電源を使用してください。

| R502-A センサー | XIAO nRF52840 |
| :--- | :--- |
| **VCC (Red)** | **3.3V** |
| **GND (Black)** | **GND** |
| **TX (Yellow)** | **D7 (RX)** |
| **RX (White)** | **D6 (TX)** |

## ビルド環境の構築
本プロジェクトは ZMK モジュール `zmk-module-TouchPass` を利用します。

```bash
west init -l config
west update
```

## ビルドと書き込み

### GitHub Actions (推奨)
1. 本プロジェクトを GitHub のリポジトリにプッシュします。
2. 依存している `zmk-module-TouchPass` も GitHub（ユーザー名: `razilyis`）にプッシュされている必要があります。
3. プッシュ後、GitHub の "Actions" タブからビルド進捗を確認できます。
4. 完了すると、Artifacts から `zephyr.uf2` をダウンロードできます。
    - **`touchpass-seeeduino_xiao_ble-zmk.uf2`**: 通常のファームウェア。
    - **`settings_reset-seeeduino_xiao_ble-zmk.uf2`**: フラッシュメモリの設定（ペアリング情報や内部ストレージ）を初期化するためのツールです。トラブル時や Arduino 版から移行する際に最初に使用してください。

### ローカルビルド
```bash
west build -b seeeduino_xiao_ble -- -DSHIELD=touchpass -DZMK_CONFIG="$(pwd)/config"
```

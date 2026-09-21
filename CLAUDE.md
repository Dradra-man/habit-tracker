# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

毎日の習慣(運動・読書・早寝)をチェックして記録する、1枚もののWebページ。ビルドツールや依存パッケージは一切なく、`index.html` 1ファイルにHTML・CSS(`<style>`)・JavaScript(`<script>`)がすべてインライン記述されている。

## 開発コマンド

ビルド・lint・テストの仕組みは存在しない。`index.html` をブラウザで直接開く(ダブルクリック、または `start index.html`)だけで動作確認できる。変更後は毎回ブラウザでリロードして目視確認すること。

## アーキテクチャ

- **ファイル構成の制約**: このプロジェクトはファイルを `index.html` の1つだけに保つ方針。新しいCSS/JSファイルやコンポーネントに分割しない。
- **データモデル**: 習慣ごとのチェック状態は `localStorage` に **日付ごとのキー**(`habit-tracker-YYYY-MM-DD`、`YYYY-MM-DD` は `new Date().toISOString().slice(0, 10)`)で保存される。日付が変わると新しいキーになるため、チェックは自動的に「その日だけの記録」としてリセットされる仕様。
- **習慣リスト**: `habits` 配列(`exercise` / `reading` / `sleep` の3つのid)が唯一の習慣定義。チェックボックスのDOM要素(`id="exercise"` など)とこの配列のidが対応している。習慣を増減する場合は、`habits` 配列と対応する `<li>` のチェックボックス・ラベルの両方を変更する必要がある。
- **状態の読み書き**:
  - 読み込み時に該当日の `localStorage` エントリを読み、各チェックボックスの `checked` に反映(`li.done` クラスで見た目にも反映)
  - チェックボックスの `change` イベントで即座に `localStorage` へ保存
  - `updateSummary()` が「今日の達成：X / Y」の表示(`#summary`)をチェック状態から都度再計算する

---
layout: post
title: Genkai (Kyushu Univ. HPC) with docker
date: 2026-01-12 00:00:00
description: Kyushu Univ. HPC - Genkai with docker
tags: work
categories: sample-posts
featured: false
---


## Genkai（九州大学スパコン）で Docker ベースの開発環境を使うためのテンプレートを公開しました


九州大学情報基盤研究開発センターが提供するスーパーコンピュータ **Genkai** は、研究用途では非常に強力な計算資源ですが， 
一方で「普段ローカルやクラウドで使っている Docker ベースの開発環境を、そのまま持ち込んで使う」ことには少し工夫が必要です．

今回，そのギャップ解消のため
**Docker で作った開発環境を Genkai 上で使うための最小構成テンプレート**を [GitHub で公開しました](https://github.com/rishiyama/genkai-docker-templete)
．

---

## モチベーション

近年における機械学習分野の研究・開発の初期段階では

- Dockerfile で環境を固定
- ローカル or CI でビルド
- 同じ環境をそのまま本番（HPC やクラスタ）に持っていく

という流れが当たり前になりつつあります．

しかし Genkai では **計算ノード上で Docker を直接動かす操作は期待されておらず**， 
代わりに Singularity / Apptainer を使う必要があります．
これは，Genkaiに限らず，産総研スパコンABCIも同様で Sigularity 環境が整備されています．
Docker環境をSigularityで利用する方法については[公式のガイダンス](https://www.cc.kyushu-u.ac.jp/scp/system/Genkai/software/Singularity.html)はこちらに記されています．


リポジトリでは，
ローカルで使用しているDocker環境をHPCで利用するまでの流れを行間を埋める形で整理しています．

---

## このテンプレートでできること

公開したリポジトリでは，次のような流れを想定しています．

1. **手元のDokcerfileをDocker Hub に push**
2. **Genkai のログインノードで Singularity により SIF を生成**
3. **インタラクティブジョブやバッチジョブで実行**
   - `jobenv=singularity`
   - `singularity exec --nv` による GPU 利用

README には， 
Docker 側のコマンド，Genkai 側の `pjsub`、`module load`、`nvidia-smi` の確認まで  
最低限必要な操作をまとめています．

---

## 対象

このリポジトリは，特に次のような人を想定しています．

- Genkai の利用を検討していて，Docker ベースの開発に慣れている人
- Python / CUDA / ML 系の実験環境を再現性高く回したい人

逆に，Singularity や HPC の詳細な仕組みを解説するものではありません．


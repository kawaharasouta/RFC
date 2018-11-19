## 1. INTRODUCTION

>The Transmission Control Protocol (TCP) is intended for use as a highly reliable host-to-host protocol between hosts in packet-switched computer communication networks, and in interconnected systems of such networks.

相互接続されたパケット交換ネットワークの高信頼なhost-to-hostプロトコル として使われることを目的としている.

>This document describes the functions to be performed by the Transmission Control Protocol, the program that implements it, and its interface to programs or users that require its services.

このドキュメントは TCPで発揮される機能, 実装, サービスを必要とするプログラムもしくはユーザへのインタフェース を示す.

### 1.1. Motivation

>Computer communication systems are playing an increasingly important role in military, government, and civilian environments.

コンピュータコミュニケーションシステムは 軍事, 政治, 民間環境 でますます重要な役割を担っている.

>This document focuses its attention primarily on military computer communication requirements, especially robustness in the presence of communication unreliability and availability in the presence of congestion, but many of these problems are found in the civilian and government sector as well.

この文書は TCPの主な目的である軍事コンピュータ通信要件, 特に 通信の信頼性の欠如がある場合のでのロバスト性(丈夫さ) and 輻輳がある中での可用性 にフォーカスしているが, それらの多くの問題は民間や政府の環境でよく見つかる.

>As strategic and tactical computer communication networks are developed and deployed, it is essential to provide means of interconnecting them and to provide standard interprocess communication protocols which can support a broad range of applications.

戦略的および戦術的な computer communication network が開発 and デプロイされるにつれて, それらを相互接続する手段 and 高範囲のアプリケーションをサポートする標準的なプロセス間通信プロトコル を提供することが不可欠である.

>In anticipation of the need for such standards, the Deputy Undersecretary of Defense for Research and Engineering has declared the Transmission Control Protocol (TCP) described herein to be a basis for DoD-wide inter-process communication protocol standardization.

そのような標準のニーズを見越して, the Deputy Undersecretary of Defense for Research(研究工学のための国防副次官補)は 本文書で説明されるTCPは DoD-wide のプロセス間通信プロトコル標準化の基礎になるものである. と宣言した.

>TCP is a connection-oriented, end-to-end reliable protocol designed to fit into a layered hierarchy of protocols which support multi-network applications.

TCPは, multi-network appをサポートするプロトコル階層 にfitするように設計された, 接続指向(コネクション型的なやつ)でend-to-endな信頼性のあるプロトコルである. 

>The TCP provides for reliable inter-process communication between pairs of processes in host computers attached to distinct but interconnected computer communication networks.

<!---
TCPは
信頼性のあるプロセス間コミュニケーション
プロセスの組 in
--->




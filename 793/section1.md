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

TCPは, 異種のコンピュータが相互接続されたネットワークに接続されたホスト内のプロセスの組 の信頼性のあるプロセス間コミュニケーションを提供する.

>Very few assumptions are made as to the reliability of the communication protocols below the TCP layer.

TCP層の下の通信プロトコルの信頼性はほとんど仮定されない.

>TCP assumes it can obtain a simple, potentially unreliable datagram service from the lower level protocols.

TCPは 下位のプロトコルから単純で本質的に信頼性のないデータグラムサービスを獲得できる と仮定している.

>In principle, the TCP should be able to operate above a wide spectrum of communication systems ranging from hard-wired connections to packet-switched or circuit-switched networks.

原則的に, TCOは ハードワイヤーからパケット交換, 回線交換までの幅広い通信システム上で 動作できるべきである

>TCP is based on concepts first described by Cerf and Kahn in [1].

TCPは, [1]のCerfとKahnによって最初に記述されたコンセプトに基づいている.

>The TCP fits into a layered protocol architecture just above a basic Internet Protocol [2] which provides a way for the TCP to send and receive variable-length segments of information enclosed in internet datagram "envelopes".

TCPは, TCPにインターネットデータグラム「封筒」(パケットのこと)に包まれた可変長な情報を送受信する方法を提供している基本的なInternet Protocol[2]の直上の階層プロトコルアーキテクチャにfitしている.

>The internet datagram provides a means for addressing source and destination TCPs in different networks.

インターネットデータグラムは 異なるネットワークに存在する送信元and送信先TCPにaddressingする方法 を提供する.

>The internet protocol also deals with any fragmentation or reassembly of the TCP segments required to achieve transport and delivery through multiple networks and interconnecting gateways.

インターネットプロトコルはまた, 複数のネットワークおよび相互接続されたゲートウェイを経由した transport and delivery を達成するために必要なTCPセグメントのフラグメンテーションやリアセンブルを処理する.

>The internet protocol also carries information on the precedence, security classification and compartmentation of the TCP segments, so this information can be communicated end-to-end across multiple networks.

インターネットプロトコルはまた, 優先順位, セキュリティ分類, TCPセグメントの区画の情報を渡すので, この情報は複数のネットワークにわたってエンドツーエンドで通信できる.
<div style="text-align: center;">
Protocol Layering
</div>

                        +---------------------+
                        |     higher-level    |
                        +---------------------+
                        |        TCP          |
                        +---------------------+
                        |  internet protocol  |
                        +---------------------+
                        |communication network|
                        +---------------------+
<div style="text-align: center;">
Figure 1
</div>

>Much of this document is written in the context of TCP
implementations which are co-resident with higher level protocols in the host computer.

この文書の多くはホストコンピュータの上位プロトコルと共存するTCPの実装のコンテクストで書かれている.

>Some computer systems will be connected to networks via front-end computers which house the TCP and internet protocol layers, as well as network specific software.

いくつかのコンンピュータシステムはTCPおよびインターネットプロトコルを収容するフロントエンドコンピュータを介してネットワークに接続される(だろう). 
ネットワーク特有のソフトウェアと同様に.

>The TCP specification describes an interface to the higher level protocols which appears to be implementable even for the front-end case, as long as a suitable host-to-front end protocol is implemented.

TCP仕様は 適切なホスト対フロントエンドプロトコルが実装されている限り フロントエンドでも実装可能と思われる 上位プロトコルへのインターフェース を説明する.

### 1.2. scope

>The TCP is intended to provide a reliable process-to-process communication service in a multinetwork environment.

TCPは マルチネットワーク環境で, 信頼性のある プロセス間通信サービスを提供する ことを目的としている.

### 1.3.  About this Document

>This document represents a specification of the behavior required of any TCP implementation, both in its interactions with higher level protocols and in its interactions with other TCPs.

この文書は 任意のTCP実装に必要な動作の仕様を 表している.
上位プロトコルの相互作用と, 他のTCPとの相互作用の両面において.

>The rest of this section offers a very brief view of the protocol interfaces and operation.

このセクションの残り部分は 
とても簡潔に 観点プロトコルインターフェースと操作 を説明します.

>Section 2 summarizes the philosophical basis for the
TCP design.

セクション2は TCP設計の哲学的な基礎を 要約する.

>Section 3 offers both a detailed description of the actions required of TCP when various events occur (arrival of new segments, user calls, errors, etc.) and the details of the formats of TCP segments.

セクション3は 様々なイベントが起きた時(新しいセグメントの到着, ユーザのコール, エラーなど)の 詳細なTCPの動作の説明 and TCPセグメントのフォーマットの詳細 を説明する.

### 1.4.  Interfaces

>The TCP interfaces on one side to user or application processes and on the other side to a lower level protocol such as Internet Protocol.

TCPは, 一方ではユーザまたはアプリケーションサイドにインタフェースし, 他方ではインターネットプロトコルのような下位プロトコルにインタフェースする.

>The interface between an application process and the TCP is illustrated in reasonable detail.

アプリケーションプロセスとTCPの間のインタフェースは 合理的な詳細で図示されている??

>This interface consists of a set of calls much like the calls an operating system provides to an application process for manipulating files.

インタフェースは 
OSがアプリケーションプロセスに提供するファイル操作を呼び出すようなコールの集合(API的な)
を含む

>For example, there are calls to open and close connections and to send and receive data on established connections.

例えば, 
コネクションを open/close するコールや establishedなコネクションからデータを send/receive するコールがある.

>It is also expected that the TCP can asynchronously communicate with application programs.

また, TCPはアプリケーションプログラムと非同期に通信できることが期待されている

>Although considerable freedom is permitted to TCP implementors to design interfaces which are appropriate to a particular operating system environment, a minimum functionality is required at the TCP/user interface for any valid implementation.

TCPの実装者には, 特定のOS環境に適切であるようなインタフェースをデザインするために, かなりの自由が許されている. が,  
有効な実装のために TCP/user インターフェイスに最低限の機能が必要とされている.

>The interface between TCP and lower level protocol is essentially unspecified except that it is assumed there is a mechanism whereby the two levels can asynchronously pass information to each other.

TCPと下位プロトコル間のインタフェースは本質的には不特定である.
それによって2つの層が非同期にお互いに情報を交換できるようなメカニズムがあることを除いて.
(各層は各層に対してオブジェクティブであるが, 情報をやり取りするAPI的なものは持っているということ)

>Typically, one expects the lower level protocol to specify this interface.  

普通, 下位プロトコルがこのインターフェースを特定することを期待している.

>TCP is designed to work in a very general environment of interconnected networks.

TCPは相互接続されたネットワークの至極一般的な環境で動作するように設計されている.

>The lower level protocol which is assumed throughout this document is the Internet Protocol [2].

このドキュメント全体を通して想定されている下位レベルのプロトコルはインターネットプロトコル[2]です.

### 1.5.  Operation

>As noted above, the primary purpose of the TCP is to provide reliable, securable logical circuit or connection service between pairs of processes.

上記のように, 







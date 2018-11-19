## 2. PHILOSOPHY
### 2.1. Elements of the Internetwork System

>
The internetwork environment consists of hosts connected to networks which are inturn interconnected via gateways.  

インターネットの環境は含んでいる. ゲートウェイを介して相互接続されたホストを.

>
It is assumed here that the networks may be either local networks (e.g., the ETHERNET) or large networks (e.g., the ARPANET), but in any case are based on packet switching technology.  

それは仮定している. ネットワークはethernetのようなローカル(地域的な)ネットワーク or ARPANETのような大きなネットワークどちらかであると. 
どのケースであってもパケット交換技術がベースにされている.
(TCPはパケット交換方式ですよということ)

>
The active agents that produce and consume messages are processes.  

メッセージを生み出し, 消費する(受け取ってその後そのメッセージは外に出さない)activeなagentはプロセスである.

>
Various levels of protocols in the networks, the gateways, and the hosts support an interprocess communication system that provides two-way data flow on logical connections between process ports.

ネットワークにはいくつかのレベルのプロセスがある. ゲートウェイ, 二つのプロセスポート間を論理的に接続するデータの流れを二つ提供するプロセス間通信システムをサポートしたホスト. (つまり登りくだりと２本あるということ.)

>
The term packet is used generically here to mean the data of one transaction between a host and its network.  

パケットという言葉はここでは一般的にネットワークの中のホストの間での１つの取引のデータを意味する.

>
The format of data blocks exchanged within the a network will generally not be of concern to us.

netowork の中で交換されるデータブロックのフォーマットは, 一般的に我々が心配することではない.

>
Hosts are computers attached to a network, and from the communication network's point of view, are the sources and destinations of packets.

ホストはネットワークに接続されたコンピュータであって, communication networkの観点で言うと, パケットのsources and destination である.

>
Processes are viewed as the active elements in host computers (in accordance with the fairly common definition of a process as a program in execution).  

プロセスは、ホストコンピュータのアクティブな要素として表示されます.
（実行中のプログラムとしてのプロセスのかなり一般的な定義に従って）

>
Even terminals and files or other I/O devices are viewed as communicating with each other through the use of processes. 

端末やファイル、または他のI / Oデバイスであっても、プロセスを使用して相互に通信しているとみなされます。

>
Thus, all communication is viewed as inter-process communication.

したがって、すべての通信はプロセス間通信とみなされる.

>
Since a process may need to distinguish among several communication streams between itself and another process (or processes), we imagine that each process may have a number of ports through which it communicates with the ports of other processes.

プロセスは、それ自体(を相手にするストリーム)と別のプロセス（または複数のプロセス）(を相手にする)との間のいくつかの通信ストリームを区別する必要があるかもしれないので、各プロセスは、他のプロセスのポートと通信するいくつかのポートを有すると想像する.

### 2.2. Model of Operation
>
Processes transmit data by calling on the TCP and passing buffers of data as arguments.  

プロセスはデータを運ぶ. TCPをコール and データバッファを引数として渡すことによって.
(L5APP目線)

>
The TCP packages the data from these buffers into segments and calls on the internet module to transmit each segment to the destination TCP.  

TCPは, これらの(引数として渡された)バッファからのデータをセグメントにパッケージし, 各セグメントを宛先TCPに送信するためにインターネットモジュール上で呼び出す.

>
The receiving TCP places the data from a segment into the receiving user's buffer and notifies the receiving user. 

受信側TCPは, セグメントからのデータを受信ユーザのバッファに入れ, 受信ユーザに通知する.

>
The TCPs include control information in the segments which they use to ensure reliable ordered data transmission.

TCPはセグメント内に「control information」をもつ. 信頼性があり順序のあるデータ通信(transmission)を保証するために使われる.

>
The model of internet communication is that there is an internet protocol module associated with each TCP which provides an interface to the local network.  

インターネットコミュニケーションのモデルは ローカルエリアへのインタフェースを提供しているそれぞれのTCPに関係のあるインターネットプロトコル(IP)モジュールがある ことである.

>
This internet module packages TCP segments inside internet datagrams and routes these datagrams to a destination internet module or intermediate gateway.  

このインターネットモジュール(ネットワークスタック)は TCPセグメントをインターネットデータグラムの中にパッケージングし, 宛先インターネットモジュール or 中間ゲートウェイにルーティングする.

>
To transmit the datagram through the local network, it is embedded in a local network packet.

ローカルネットワークを通してデータグラムを転送するために, データグラムはローカルネットワークパケットに埋め込まれます.

>
The packet switches may perform further packaging, fragmentation, or other operations to achieve the delivery of the local packet to the destination internet module.

パケットスイッチ(各ルータとかのことを指してる?)は、さらなるパッケージング, フラグメンテーション, または他の動作を実行することができる.
宛先パケットの宛先インターネットモジュールへの配信を達成するために.

>
At a gateway between networks, the internet datagram is "unwrapped" from its local packet and examined to determine through which network the internet datagram should travel next.  

ネットワークの間のゲートウェイでは, インターネットデータグラムは “unwrappded” され, 次にどのネットワークを通ってデータグラムが移動するかを決めるために調査される.

>
The internet datagram is then "wrapped" in a local packet suitable to the next network and routed to the next gateway, or to the final destination.

インターネットデータグラムは, 次のネットワークに適したローカルパケットに「wrapped」され、次のゲートウェイ or 最終宛先にルーティングされる.

>
A gateway is permitted to break up an internet datagram into smaller internet datagram fragments if this is necessary for transmission through the next network. 

ゲートウェイはインターネットデータグラムを 小さなフラグメントに分けることを許可する. もし次のネットワークを通すのに必要であるならば.

>
To do this, the gateway produces a set of internet datagrams; each carrying a fragment.  
これを行うために, ゲートウェイは一連のインターネットデータグラムを生成する. それぞれがフラグメントを運んでいる.
(フラグ化されたパケットが連なって送られる)

>
Fragments may be further broken into smaller fragments at subsequent gateways.  

フラグメントは, その後のゲートウェイでより小さなフラグメントに分割されるかもしれません.

>
The internet datagram fragment format is designed so that the destination internet module can reassemble fragments into internet datagrams.

The internet datagram fragment format は 宛先インターネットモジュールでフラグメントをインターネットデータグラムに reassemble できるようにデザインされている.

>
A destination internet module unwraps the segment from the datagram (after reassembling the datagram, if necessary) and passes it to the destination TCP.

宛先インターネットモジュールは, セグメントをデータグラムからアンラップし(必要に応じてデータグラムを再構成した後), それを宛先TCPに渡す.

>
This simple model of the operation glosses over many details.  

この単純な操作モデルは、多くの詳細を隠している.
(L5APPから見てオブジェクティブですと言っている.)

>
One important feature is the type of service.  

一つの重要な特徴は type of service です.

>
This provides information to the gateway (or internet module) to guide it in selecting the service parameters to be used in traversing the next network. 

type of service は ゲートウェイ(or インターネットモジュール)に対して情報を提供する.
次のネットワークへ送られる際に使用されるサービスパラメータを選択するのをガイドするために.

>
Included in the type of service information is the precedence of the datagram. 

type of service には, データグラムの優先順位が含まれます.

>
Datagrams may also carry security information to permit host and gateways that operate in multilevel secure environments to properly segregate datagrams for security considerations.

また, データグラムは, マルチレベルの安全な環境で動作するホストとゲートウェイがセキュリティ上の考慮事項のためにデータグラムを適切に分離することを可能にするセキュリティ情報を運ぶことができます.
## 3. FUNCTIONAL SPECIFICATION

### 3.1. Header Format

>TCP segments are sent as internet datagrams.

TCPセグメントはインターネットデータグラムとして送られる.

>The Internet Protocol header carries several information fields, including the source and destination host addresses [2].

インターネットプロトコルヘッダはいくつかの情報フィールドを運ぶ.
source and destination アドレスを含んで.

>A TCP header follows the internet header, supplying information specific to the TCP protocol.

TCPヘッダはインターネットヘッダに続き, TCPプロトコル固有の情報を提供します.

>This division allows for the existence of host level protocols other than TCP.

この区分は, TCP以外のホストレベルプロトコルの存在を可能にしている.

<div style="text-align: center;">
TCP Header Format
</div>

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |       Destination Port        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Acknowledgment Number                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Data |           |U|A|P|R|S|F|                               |
| Offset| Reserved  |R|C|S|S|Y|I|            Window             |
|       |           |G|K|H|T|N|N|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Checksum            |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                             data                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

<div style="text-align: center;">
TCP Header Format
</div>
<div style="text-align: center;">
Note that one tick mark represents one bit position.
</div>
<div style="text-align: center;">
Figure 3.
</div>

>Source Port: 16 bits
The source port number.

略

>Destination Port: 16 bits
The destination port number.

略

>Sequence Number: 32 bits
The sequence number of the first data octet in this segment(except when SYN is present). If SYN is present the sequence number is the initial sequence number (ISN) and the first data octet is ISN+1.

略

>Acknowledgment Number: 32 bits
If the ACK control bit is set this field contains the value of the next sequence number the sender of the segment is expecting to receive. Once a connection is established this is always sent.

略

>Data Offset: 4 bits
The number of 32 bit words in the TCP Header. This indicates where the data begins. The TCP header (even one including options) is an integral number of 32 bits long.

略

>Reserved: 6 bits
Reserved for future use. Must be zero.

略

>Control Bits: 6 bits (from left to right):
URG: Urgent Pointer field significant
ACK: Acknowledgment field significant
PSH: Push Function
RST: Reset the connection
SYN: Synchronize sequence numbers
FIN: No more data from sender

略

>Window: 16 bits
The number of data octets beginning with the one indicated in the acknowledgment field which the sender of this segment is willing to accept.

略

>Checksum: 16 bits
The checksum field is the 16 bit one's complement of the one's complement sum of all 16 bit words in the header and text. If a segment contains an odd number of header and text octets to be checksummed, the last octet is padded on the right with zeros to form a 16 bit word for checksum purposes. The pad is not transmitted as part of the segment. While computing the checksum, the checksum field itself is replaced with zeros.

略

>The checksum also covers a 96 bit pseudo header conceptually prefixed to the TCP header.

チェックサムはまた, 96bitの 概念的にはTCPヘッダの前に置かれている 疑似ヘッダもカバーしている.

>This pseudo header contains the Source Address, the Destination Address, the Protocol, and TCP length.

この疑似ヘッダは Sourceアドレス, Destinationアドレス, プロトコル, TCP length を含んでいる.

>This gives the TCP protection against misrouted segments.

これ(疑似ヘッダ)は 誤配送されたセグメントに対するTCP保護 を提供する

>This information is carried in the Internet Protocol and is transferred across the TCP/Network interface in the arguments or results of calls by the TCP on the IP.

この情報はインターネットプロトコルで運ばれ, IP上のTCPによって呼び出された引数または結果でTCP /ネットワークインターフェイスを介して転送される.

```

+--------+--------+--------+--------+
|           Source Address          |
+--------+--------+--------+--------+
|         Destination Address       |
+--------+--------+--------+--------+
|  zero  |  PTCL  |    TCP Length   |
+--------+--------+--------+--------+

```

>The TCP Length is the TCP header length plus the data length in octets (this is not an explicitly transmitted quantity, but is computed), and it does not count the 12 octets of the pseudo header.

略

>Urgent Pointer: 16 bits
This field communicates the current value of the urgent pointer as a positive offset from the sequence number in this segment. The urgent pointer points to the sequence number of the octet following the urgent data. This field is only be interpreted in segments with the URG control bit set.

このフィールドは、緊急ポインタの現在の値を、このセグメント内のシーケンス番号からの正のオフセットとして通信します。 緊急ポインタは、緊急データに続くオクテットのシーケンス番号を指し示す。 このフィールドは、URG制御ビットが設定されたセグメントでのみ解釈されます。

>Options: variable
Options may occupy space at the end of the TCP header and are a multiple of 8 bits in length. All options are included in the checksum. An option may begin on any octet boundary. 
There are two cases for the format of an option:
Case 1: A single octet of option-kind.
Case 2: An octet of option-kind, an octet of option-length, and the actual option-data octets.

オプションは、TCPヘッダーの最後にスペースを占有し、8ビットの倍数です。 すべてのオプションはチェックサムに含まれています。 オプションは任意の八重奏の境界で始まるかもしれません。
ケース1：option-kindの単一オクテット。
ケース2：オプション種別のオクテット、オプション長のオクテット、実際のオプションデータオクテット。

>Note that the list of options may be shorter than the data offset field might imply. 

オプションのリストは、データオフセットフィールドが含意するよりも短くてもよいことに注意してください。

>The content of the header beyond the End-of-Option option must be header padding (i.e., zero).

End-of-Optionオプションを越えるヘッダーの内容は、ヘッダーの埋め込み（つまり、ゼロ）でなければなりません。

>A TCP must implement all options.

略

>Currently defined options include (kind indicated in octal):

現在定義されているオプションには、（種類は8進数で表示されます）

```

Kind Length Meaning
---- ------ -------
 0     -    End of option list.
 1     -    No-Operation.
 2     4    Maximum Segment Size.

```

Specific Option Definitions  

**End of Option List**

```

+--------+
|00000000|
+--------+
 Kind=0

```

>This option code indicates the end of the option list. This might not coincide with the end of the TCP header according to the Data Offset field. This is used at the end of all options, not the end of each option, and need only be used if the end of the options would not otherwise coincide with the end of the TCP header.

このオプションコードは、オプションリストの終わりを示します。 これは、Data Offsetフィールドに従ってTCPヘッダーの終わりと一致しないことがあります。 これは、各オプションの終わりではなく、すべてのオプションの最後に使用され、オプションの終わりがTCPヘッダーの終わりと一致しない場合にのみ使用する必要があります。

**No-Operation**

```

+--------+
|00000001|
+--------+
 Kind=1

```

>This option code may be used between options, for example, to align the beginning of a subsequent option on a word boundary. There is no guarantee that senders will use this option, so receivers must be prepared to process options even if they
do not begin on a word boundary.

このオプションコードは、オプション間で使用して、後続のオプションの先頭を単語境界に揃えるなどに使用できます。 送信者がこのオプションを使用する保証はないため、受信者は単語の境界で始まらなくてもオプションを処理する準備ができている必要があります。

**Maximum Segment Size**

```

+--------+--------+---------+--------+
|00000010|00000100|   max seg size   |
+--------+--------+---------+--------+
 Kind=2   Length=4

```

>Maximum Segment Size Option Data: 16 bits
If this option is present, then it communicates the maximum receive segment size at the TCP which sends this segment. This field must only be sent in the initial connection request (i.e., in segments with the SYN control bit set). If this option is not used, any segment size is allowed.

このオプションが存在する場合、このセグメントを送信するTCPで最大受信セグメントサイズを通信します。 このフィールドは、初期接続要求（SYN制御ビットが設定されたセグメント）でのみ送信する必要があります。 このオプションを使用しない場合、セグメントサイズは許されます。

>Padding: variable
The TCP header padding is used to ensure that the TCP header ends and data begins on a 32 bit boundary. The padding is composed of zeros.

TCPヘッダーパディングは、TCPヘッダーが終了し、データが32ビット境界で始まることを保証するために使用されます。 パディングはゼロで構成されています。

### 3.2. Terminology (用語)

>Before we can discuss very much about the operation of the TCP we need to introduce some detailed terminology.

TCPの動作について(非常に?)議論する前に, いくつかの詳細な用語を紹介する必要がある. 

>The maintenance of a TCP connection requires the remembering of several variables.

TCPコネクションの維持は いくつかの変数の記憶 を要求する.

>We conceive of these variables being stored in a connection record called a Transmission Control Block or TCB.

それらの変数は Transmission Control Block(TCB) と呼ばれるコネクションレコードに 保存される.

>Among the variables stored in the TCB are the local and remote socket numbers, the security and precedence of the connection, pointers to the user's send and receive buffers, pointers to the retransmit queue and to the current segment.

TCBに保存される変数の中には, ローカルとリモートのソケット番号, 接続のセキュリティと優先順位, ユーザのsend and receiveバッファのポインタ, 再送キューのポインタ, 現在のセグメントが含まれる.

>In addition several variables relating to the send and receive sequence numbers are stored in the TCB.

さらに, send and receive シーケンス番号 に関係しているいくつかの変数が TCBに保存されている.  

**Send Sequence Variables**
```
SND.UNA - send unacknowledged
SND.NXT - send next
SND.WND - send window
SND.UP  - send urgent pointer
SND.WL1 - segment sequence number used for last window update
SND.WL2 - segment acknowledgment number used for last window update
ISS     - initial send sequence number
```

**Receive Sequence Variables**
```
RCV.NXT - receive next
RCV.WND - receive window
RCV.UP  - receive urgent pointer
IRS     - initial receive sequence number
```

>The following diagrams may help to relate some of these variables to the sequence space.

次の図は いくつかのそれらの変数シーケンススペースに関連付けるのに 役立つだろう.

**Send Sequence Space**

```
    1          2          3          4
----------|----------|----------|----------
       SND.UNA    SND.NXT    SND.UNA
                            +SND.WND

1 - old sequence numbers which have been acknowledged
2 - sequence numbers of unacknowledged data
3 - sequence numbers allowed for new data transmission
4 - future sequence numbers which are not yet allowed
```
<div style="text-align: center;">
Send Sequence Space
Figure 4.
</div>

>The send window is the portion of the sequence space labeled 3 in figure 4.

send window は 図4で3とラベル付けされたシーケンススペースの部分である.
(3がsend window です)

**Receive Sequence Space**

```
    1          2          3
----------|----------|----------
       RCV.NXT    RCV.NXT
                 +RCV.WND

1 - old sequence numbers which have been acknowledged
2 - sequence numbers allowed for new reception
3 - future sequence numbers which are not yet allowed
```

<div style="text-align: center;">
Receive Sequence Space
Figure 5.
</div>

>The receive window is the portion of the sequence space labeled 2 in figure 5.

receive window 図5で2とラベル付けされたシーケンススペースの部分である.

>There are also some variables used frequently in the discussion that take their values from the fields of the current segment.

また, いくつかの変数が 現在のセグメントのフィールドから値をとるディスカッションに よくつかわれる.

**Current Segment Variables**

```
SEG.SEQ - segment sequence number  
SEG.ACK - segment acknowledgment number  
SEG.LEN - segment length  
SEG.WND - segment window  
SEG.UP  - segment urgent pointer  
SEG.PRC - segment precedence value
```

>A connection progresses through a series of states during its lifetime.

コネクションは その生存期間中に一連の状態を経て進行する.

>The states are: LISTEN, SYN-SENT, SYN-RECEIVED, ESTABLISHED, FIN-WAIT-1, FIN-WAIT-2, CLOSE-WAIT, CLOSING, LASTACK, TIME-WAIT, and the fictional state CLOSED.

ステートの紹介

>CLOSED is fictional because it represents the state when there is no TCB, and therefore, no connection.

CLOSEDは架空の(実際の状態としては存在しえない)状態である. 
because それは TCBがない, すなわちコネクションが張られていないときの状態を表しているからである.

>Briefly the meanings of the states are:

ステートのせつめい.

>LISTEN - represents waiting for a connection request from any remote TCP and port.

LISTEN  
remote TCP and port からのコネクション要求を待っている

>SYN-SENT - represents waiting for a matching connection request after having sent a connection request.

SYN-SENT
リクエストを送った後にコネクションマッチングを待っている
(クライアントがSYNを送った状態)

>SYN-RECEIVED - represents waiting for a confirming connection request acknowledgment after having both received and sent a connection request.

SYN-RECEIVED
リクエストを受信, 送信した後に 確認接続要求的なやつを待っている状態.
(サーバがSYNを受信した後SYN, ACKおくって待ってる)

>ESTABLISHED - represents an open connection, data received can be delivered to the user. The normal state for the data transfer phase of the connection.

ESTABLISHED
オープン接続を表していて, 受信したデータをユーザに送ることができる.
コネクションのデータ送信フェーズの標準のステート.
(netstatすりゃわかる)

>FIN-WAIT-1 - represents waiting for a connection termination request from the remote TCP, or an acknowledgment of the connection termination request previously sent.

FIN-WAIT-1
リモートTCPからのコネクション終了リクエスト or 以前に送信したコネクション終了要求の確認 を待っている
(appがクローズしてFINを送った状態. アクティブ)

>FIN-WAIT-2 - represents waiting for a connection termination request from the remote TCP.

FIN-WAIT-2
リモートTCPからのコネクション終了要求を待っている.
(FIN-WAIT-1 からACKを受信して, その後FINを待っている状態. アクティブ)

>CLOSE-WAIT - represents waiting for a connection termination request from the local user.

CLOSE-WAIT
ローカルユーザからのコネクション終了要求を待っている.
(FINを受信しACKを送信した後, appからのクローズを待つ. パッシブ)

>CLOSING - represents waiting for a connection termination request acknowledgment from the remote TCP.

CLOSING
リモートTCPからのコネクション終了要求の確認応答を待っている.
(FIN-WAIT-2と若干似ているが, こちらは同時クローズな状態の場合. すなわち, FINを送ってあってACKは受信していないがその前にFINを受信している状態. アクティブ)

>LAST-ACK - represents waiting for an acknowledgment of the connection termination request previously sent to the remote TCP (which includes an acknowledgment of its connection termination request).

LAST-ACK
リモートTCPに以前に送信されたコネクション終了要求の確認応答（コネクション終了要求の確認応答を含む）を待つことを表す。
(CLOSE-WAITからappのクローズを受けた後, FINを送ってACKを待っている状態. パッシブ)

>TIME-WAIT - represents waiting for enough time to pass to be sure the remote TCP received the acknowledgment of its connection termination request.

リモートTCPがその接続終了要求の確認応答を受信したことを確実にするのに十分な時間待つことを表す.
(2MSLのやつ. アクティブ)

>CLOSED - represents no connection state at all.

略

>A TCP connection progresses from one state to another in response to events.

TCPコネクションはあるステートからイベントに反応して他の状態へ進行する.

>The events are the user calls, OPEN, SEND, RECEIVE, CLOSE, ABORT, and STATUS; the incoming segments, particularly those containing the SYN, ACK, RST and FIN flags; and timeouts.

略

>The state diagram in figure 6 illustrates only state changes, together with the causing events and resulting actions, but addresses neither error conditions nor actions which are not connected with state changes.

図6のステート図(状態遷移図)は状態の変化のみを きっかけとなるイベントとその結果のアクションとともに 表している, 
but エラー状態や状態変更に関連しないアクションには対処しない.

>In a later section, more detail is offered with respect
to the reaction of the TCP to events.

この後のセクションでは, 
イベントに対するTCPの反応に関してより詳細に表します.

>NOTE BENE:  this diagram is only a summary and must not be taken as the total specification.

この図は要約に過ぎず, 全体の仕様とみなしてはならない.

```
                            +---------+ ---------\      active OPEN
                              |  CLOSED |            \    -----------
                              +---------+<---------\   \   create TCB
                                |     ^              \   \  snd SYN
                   passive OPEN |     |   CLOSE        \   \
                   ------------ |     | ----------       \   \
                    create TCB  |     | delete TCB         \   \
                                V     |                      \   \
                              +---------+            CLOSE    |    \
                              |  LISTEN |          ---------- |     |
                              +---------+          delete TCB |     |
                   rcv SYN      |     |     SEND              |     |
                  -----------   |     |    -------            |     V
 +---------+      snd SYN,ACK  /       \   snd SYN          +---------+
 |         |<-----------------           ------------------>|         |
 |   SYN   |                    rcv SYN                     |   SYN   |
 |   RCVD  |<-----------------------------------------------|   SENT  |
 |         |                    snd ACK                     |         |
 |         |------------------           -------------------|         |
 +---------+   rcv ACK of SYN  \       /  rcv SYN,ACK       +---------+
   |           --------------   |     |   -----------
   |                  x         |     |     snd ACK
   |                            V     V
   |  CLOSE                   +---------+
   | -------                  |  ESTAB  |
   | snd FIN                  +---------+
   |                   CLOSE    |     |    rcv FIN
   V                  -------   |     |    -------
 +---------+          snd FIN  /       \   snd ACK          +---------+
 |  FIN    |<-----------------           ------------------>|  CLOSE  |
 | WAIT-1  |------------------                              |   WAIT  |
 +---------+          rcv FIN  \                            +---------+
   | rcv ACK of FIN   -------   |                            CLOSE  |
   | --------------   snd ACK   |                           ------- |
   V        x                   V                           snd FIN V
 +---------+                  +---------+                   +---------+
 |FINWAIT-2|                  | CLOSING |                   | LAST-ACK|
 +---------+                  +---------+                   +---------+
   |                rcv ACK of FIN |                 rcv ACK of FIN |
   |  rcv FIN       -------------- |    Timeout=2MSL -------------- |
   |  -------              x       V    ------------        x       V
    \ snd ACK                 +---------+delete TCB         +---------+
     ------------------------>|TIME WAIT|------------------>| CLOSED  |
                              +---------+                   +---------+
```

### 3.3.  Sequence Numbers

>A fundamental notion in the design is that every octet of data sent over a TCP connection has a sequence number.

設計における基本的な概念は
TCPコネクションを介して送られるデータの全オクテットがシーケンス番号を持っている.
(オクテットごとにシーケンス番号が振られている. みたいな感じ.)

>Since every octet is sequenced, each of them can be acknowledged.

すべてのオクテットがシーケンスされているので、それぞれのオクテットを確認することができます.

>The acknowledgment mechanism employed is cumulative so that an acknowledgment of sequence number X indicates that all octets up to but not including X have been received.

使用されるachnowledgementメカニズムは累積的であり、シーケンス番号Xの確認応答は、Xを含まないすべてのオクテットが受信されたことを示す。

>This mechanism allows for straight-forward duplicate detection in the presence of retransmission.

このメカニズムは再送信存在での直接的な重複検査を可能にする. 

>Numbering of octets within a segment is that the first data octet immediately following the header is the lowest numbered, and the following octets are numbered consecutively.

セグメント内のオクテットのナンバリングは, ヘッダ直後のデータが最も低い値であり, 続くオクテットが連続してナンバリングされる.

>It is essential to remember that the actual sequence number space is finite, though very large.

実際のシーケンス番号空間は有限であるということが重要である. 非常に大きいが.

>This space ranges from 0 to 2**32 - 1. 

この(シーケンス番号)空間は 0 ~ 2^32 である.

>Since the space is finite, all arithmetic dealing with sequence numbers must be performed modulo 2**32.

空間は有限であるため, 全ての算術演算は mod 2^32 で実行される必要がある.

>This unsigned arithmetic preserves the relationship of sequence numbers as they cycle from 2**32 - 1 to 0 again.

この符号なし演算は 2^32-1 ~ 0 のサイクルとしてシーケンス番号の関係を保持する.

>There are some subtleties to computer modulo arithmetic, so great care should be taken in programming the comparison of such values.  

コンピュータmod演算にはいくつかの微妙な点があるので, そのような値の比較をプログラムするときには注意が必要です.

>The symbol "=<" means "less than or equal" (modulo 2**32).

"=<" は (mod 2^32での) "以下" を意味する.

>The typical kinds of sequence number comparisons which the TCP must perform include

TCPが実行しなければならないシーケンス番号比較の典型的な種類は(下に記述)

>(a) Determining that an acknowledgment refers to some sequence number sent but not yet acknowledged.  
>(b) Determining that all sequence numbers occupied by a segment have been acknowledged (e.g., to remove the segment from a retransmission queue).  
>(c) Determining that an incoming segment contains sequence numbers which are expected (i.e., that the segment "overlaps" the receive window).

(a)  
(b)  
(c) 

<!-- [page 24] -->

>In response to sending data the TCP will receive acknowledgments.

(対向の)データの送信に応答して, TCPは肯定応答を受信する. 



<!-- [page 27] 途中から -->

#### The TCP Quiet Time Concept

>This specification provides that hosts which "crash" without retaining any knowledge of the last sequence numbers transmitted on each active (i.e., not closed) connection shall delay emitting any TCP segments for at least the agreed Maximum Segment Lifetime (MSL) in the internet system of which the host is a part.

この仕様は, 各アクティブな(すなわち非クローズド)コネクション上で送信された最後のシーケンス番号の知識を保持せずに "クラッシュ"するホストは, インターネットシステムにおいて少なくとも合意された最大セグメント寿命（MSL） ホストの一部です.  ?

>In the paragraphs below, an explanation for this specification is given.

以下の段落では, この仕様の説明が与えられます. 

>TCP implementors may violate the "quiet time" restriction, but only at the risk of causing some old data to be accepted as new or new data rejected as old duplicated by some receivers in the internet system.

TCP実装者は, "quiet time"の制限に違反する可能性がありますが、インターネットシステムのいくつかの受信者によって重複して古いものとして新しいデータや新しいデータが拒否されるため、古いデータを受け入れる危険があります。
(以前説明した2MSLの話)

>TCPs consume sequence number space each time a segment is formed and entered into the network output queue at a source host.

TCPは, セグメントが形成されソースホスト(送信者)のネットワーク出力キューに入力されるたびにシーケンス番号空間を消費します.

>The duplicate detection and sequencing algorithm in the TCP protocol relies on the unique binding of segment data to sequence space to the extent that sequence numbers will not cycle through all 2**32 values before the segment data bound to those sequence numbers has been delivered and acknowledged by the receiver and all duplicate copies of the segments have "drained" from the internet.

>TCPプロトコルの重複検出およびシーケンシングアルゴリズムは、シーケンス番号がそれらのシーケンス番号にバインドされたデータが配信される前にすべての2^32値を循環しない範囲で、セグメントデータのシーケンススペースへの一意のバインディングに依存します 受領者によって承認され、セグメントの複製コピーはすべてインターネットから「流出」しています。

>Without such an assumption, two distinct TCP segments could conceivably be  assigned the same or overlapping sequence numbers, causing confusion at the receiver as to which data is new and which is old.

このような前提がないと、2つの異なるTCPセグメントに同じまたは重複するシーケンス番号が割り当てられ、新しいデータと古いデータが混在して受信機に混乱を招く可能性があります。

>Remember that each segment is bound to as many consecutive sequence numbers as there are octets of data in the segment.

各セグメントはセグメント内のデータのオクテットと同じ数の連続したシーケンス番号にバインドされていることに注意してください.

>Under normal conditions, TCPs keep track of the next sequence number to emit and the oldest awaiting acknowledgment so as to avoid mistakenly using a sequence number over before its first use has been acknowledged.

通常の状態では, TCPは最初に使用される前にシーケンス番号を間違って使用するのを避けるために、次に発行するシーケンス番号と最も古い受信確認番号を追跡します。

>This alone does not guarantee that old duplicate data is drained from the net, so the sequence space has been made very large to reduce the probability that a wandering duplicate will cause trouble upon arrival.

これだけでは古い重複データがネットから排出されることを保証するものではないため, 非常に大きなシーケンス番号空間がさまよう重複(wandering duplicate)が到着時にトラブルを引き起こす可能性を低減します.

>At 2 megabits/sec. it takes 4.5 hours to use up 2**32 octets of sequence space.

2Mbit/sでは, 2^32のシーケンス番号空間を4.5時間で使い切ります.

>Since the maximum segment lifetime in the net is not likely to exceed a few tens of seconds, this is deemed ample protection for foreseeable nets, even if data rates escalate to l0's of megabits/sec. 

ネットの最大セグメント寿命は数十秒を超えない可能性が高いため, データレートが10メガビット/秒に拡大しても, 予測可能なネットに対する十分な保護と見なされる.

>At 100 megabits/sec, the cycle time is 5.4 minutes which may be a little short, but still within reason.

100Mbits/sでは, サイクル時間は5.4分と短いが, 問題はありません.

>The basic duplicate detection and sequencing algorithm in TCP can be defeated, however, if a source TCP does not have any memory of the sequence numbers it last used on a given connection.

しかし、ソースTCPがシーケンス番号のメモリを持たず、与えられた接続で最後に使用された場合、TCPの基本的な重複検出およびシーケンシングアルゴリズムは無効になります。

>For example, if the TCP were to start all connections with sequence number 0, then upon crashing and restarting, a TCP might re-form an earlier connection (possibly after half-open connection resolution) and emit packets with sequence numbers identical to or overlapping with packets still in the network which were emitted on an earlier incarnation of the same connection.

たとえば、TCPがすべての接続をシーケンス番号0で開始する場合、クラッシュして再起動すると、TCPは以前の接続を再構成し（半開の接続解決後に）、シーケンス番号が同一または重複するパケットを送信する 同じ接続の以前のインカネーションで発行されたネットワーク内のパケットはまだ残っています。

>In the absence of knowledge about the sequence numbers used on a particular connection, the TCP specification recommends that the source delay for MSL seconds before emitting segments on the connection, to allow time for segments from the earlier connection incarnation to drain from the system.

特定の接続で使用されるシーケンス番号についての情報がない場合、TCP仕様では、以前の接続インカネーションからのセグメントの時間をシステムから排除するために、接続でセグメントを送信する前にMSL秒のソース遅延を推奨します。

>Even hosts which can remember the time of day and used it to select initial sequence number values are not immune from this problem (i.e., even if time of day is used to select an initial sequence number for each new connection incarnation).

時刻を記憶してそれを初期シーケンス番号値の選択に使用するホストであっても、この問題は免れない（すなわち、時刻が新しい接続インカネーションごとに初期シーケンス番号を選択しても）。

>Suppose, for example, that a connection is opened starting with sequence number S.  

例えば, シーケンう番号Sでコネクションが開かれたとする.

>Suppose that this connection is not used much and that eventually the initial sequence number function (ISN(t)) takes on a value equal to the sequence number, say S1, of the last segment sent by this TCP on a particular connection.

この接続があまり使用されておらず、最終的に初期シーケンス番号関数(ISN(t))が、特定の接続でこのTCPによって送信された最後のセグメントのシーケンス番号がS1に等しい値をとると仮定します。

>Now suppose, at this instant, the host crashes, recovers, and establishes a new incarnation of the connection. The initial sequence number chosen is S1 = ISN(t) -- last used sequence number on old incarnation of connection!  If the recovery occurs quickly enough, any old duplicates in the net bearing sequence numbers in the neighborhood of S1 may arrive and be treated as new packets by the receiver of the new incarnation of the connection.


今、この瞬間に、ホストがクラッシュし、回復し、新しい接続のインカーネーションを確立すると仮定します。 選択された初期シーケンス番号は、S1 = ISN(t) - コネクションの古いインカネーションで最後に使用されたシーケンス番号です。 回復が速やかに十分に行われた場合、S1の近傍の正味のベアリングシーケンス番号にある古い重複が到着し、接続の新しいインカネーションの受信者によって新しいパケットとして扱われることがあります。

>The problem is that the recovering host may not know for how long it crashed nor does it know whether there are still old duplicates in the system from earlier connection incarnations.

問題は、復旧しているホストがクラッシュした時間を知ることができず、以前の接続インカネーションから古い重複がシステムに残っているかどうかを知ることもできないということです。

>One way to deal with this problem is to deliberately delay emitting segments for one MSL after recovery from a crash- this is the "quite time" specification. 

この問題に対処する1つの方法は、クラッシュからの回復後に1つのMSLのセグメントを意図的に遅らせることです。これは"quite time"の仕様です.

>Hosts which prefer to avoid waiting are willing to risk possible confusion of old and new packets at a given destination may choose not to wait for the "quite time".

待機を避けることを好むホストは, 与えられた宛先での古いパケットと新しいパケットの混乱を招くリスクを選択し, "quite time"を待たないことを選択することがあります。

>Implementors may provide TCP users with the ability to select on a connection by connection basis whether to wait after a crash, or may informally implement the "quite time" for all connections.

実装者は、クラッシュ後に待機するかどうかを接続ごとに選択する機能をTCPユーザーに提供したり、すべての接続に対して非正常的に"quite time"を実装することができます。

>Obviously, even where a user selects to "wait," this is not necessary after the host has been "up" for at least MSL seconds.

明らかに、ユーザが"待機する"ことを選択した場合であっても、ホストが少なくともMSL秒間 "アップ(起動)"した後であれば、これは必要ではない。

>To summarize:

要約:

>every segment emitted occupies one or more sequence numbers in the sequence space, the numbers occupied by a segment are "busy" or "in use" until MSL seconds have passed, upon crashing a block of space-time is occupied by the octets of the last emitted segment, if a new connection is started too soon and uses any of the sequence numbers in the space-time footprint of the last segment of the previous connection incarnation, there is a potential sequence number overlap area which could cause confusion at the receiver.

放出された各セグメントはシーケンス空間内の1つ以上のシーケンス番号を占有し、セグメントが占有する数は、MSL秒が経過するまで「ビジー」または「使用中」である。 新しい接続があまりにも早く開始され、以前の接続インカネーションの最後のセグメントの時空間フットプリント内のシーケンス番号のいずれかを使用する場合、潜在的なシーケンス番号オーバーラップ領域が存在し、受信側で混乱を招く可能性がある 。

### 3.4.  Establishing a connection

>The "three-way handshake" is the procedure used to establish a connection.  This procedure normally is initiated by one TCP and responded to by another TCP.  

"3ウェイハンドシェイク"は、接続を確立するために使用される手順です。 この手順は通常、1つのTCPによって開始され、別のTCPによって応答されます。 

>The procedure also works if two TCP simultaneously initiate the procedure.

この手順は、2つのTCPが同時に手順を開始する場合にも機能します。

>When simultaneous attempt occurs, each TCP receives a "SYN" segment which carries no acknowledgment after it has sent a "SYN".

同時試行が発生すると、各TCPは"SYN"セグメントを受信し, "SYN"セグメントを受信します。

>Of course, the arrival of an old duplicate "SYN" segment can potentially make it appear, to the recipient, that a simultaneous connection initiation is in progress.

もちろん、古い重複した「SYN」セグメントの到着は、潜在的に受信者に、同時の接続開始が進行中であるように見せることができる。

>Proper use of "reset" segments can disambiguate these cases.

"reset"セグメントを適切に使用すると、これらのケースの曖昧さをなくすことができる可能性があります。

>Several examples of connection initiation follow.

接続開始のいくつかの例を下にしめす.

>Although these examples do not show connection synchronization using data-carrying segments, this is perfectly legitimate, so long as the receiving TCP doesn't deliver the data to the user until it is clear the data is valid (i.e., the data must be buffered at the receiver until the connection reaches the ESTABLISHED state).

???

>The three-way handshake reduces the possibility of false connections.

スリーウェイハンドシェイクは誤った接続の可能性を減らす.

>It is the implementation of a trade-off between memory and messages to provide information for this checking.

このチェックのための情報を提供するのは、メモリとメッセージとの間のトレードオフの実装です。

>The simplest three-way handshake is shown in figure 7 below.

最も単純な3ウェイハンドシェイクを下の図7に示す.

>The figures should be interpreted in the following way.

図は次のように解釈する必要があります。

>Each line is numbered for reference purposes.

参照のために各行に番号が付けられています。

>Right arrows (-->) indicate departure of a TCP segment from TCP A to TCP B, or arrival of a segment at B from A.

右矢印(-->)は、TCP AからTCP BへのTCPセグメントの出発(送信)、またはAからのBへのセグメントの到着を示します。

>Left arrows (<--), indicate the reverse.

左は逆.

>Ellipsis (...) indicates a segment which is still in the network (delayed).

省略記号（...）は、まだネットワーク内にある（遅延している）セグメントを示します。

>An "XXX" indicates a segment which is lost or rejected.

"XXX"は、紛失または拒絶されたセグメントを示す。

>Comments appear in parentheses.

コメントはカッコ内に表示されます。

>TCP states represent the state AFTER the departure or arrival of the segment (whose contents are shown in the center of each line).

TCPのステートは、セグメントの出発または到着後の状態を表します（内容は各行の中央に表示されます）。

>Segment contents are shown in abbreviated form, with sequence number, control flags, and ACK field.

セグメントの内容は、シーケンス番号、制御フラグ、およびACKフィールドを含む省略形で示されています。

>Other fields such as window, addresses, lengths, and text have been left out in the interest of clarity.

ウィンドウ、アドレス、長さ、テキストなどの他のフィールドは明快にするために省略されています。

```

	TCP A																												TCP B
  1.  CLOSED																									LISTEN
  2.  SYN-SENT   	--> <SEQ=100><CTL=SYN>   							--> SYN-RECEIVE
  3.  ESTABLISHED <-- <SEQ=300><ACK=101><CTL=SYN,ACK>  	<-- SYN-RECEIVED
  4.  ESTABLISHED --> <SEQ=101><ACK=301><CTL=ACK>       --> ESTABLISHED
  5.  ESTABLISHED --> <SEQ=101><ACK=301><CTL=ACK><DATA> --> ESTABLISHED

```

<div style="text-align: center;">
Basic 3-Way Handshake for Connection Synchronization  Figure 7.
</div>


>In line 2 of figure 7, TCP A begins by sending a SYN segment indicating that it will use sequence numbers starting with sequence number 100.

図7の2行目では、TCP Aは、シーケンス番号100で始まるシーケンス番号を使用することを示すSYNセグメントを送信することから始まります。

>In line 3, TCP B sends a SYN and acknowledges the SYN it received from TCP A.

3行目では、TCP BはSYNを送信し、TCP Aから受信したSYNを確認応答します。

>Note that the acknowledgment field indicates TCP B is now expecting to hear sequence 101, acknowledging the SYN which occupied sequence 100.

肯定応答フィールドは、TCP Bが現在シーケンス101を聞くことを期待しており、シーケンス100を占有していたSYNを確認していることを示していることに留意されたい。

>At line 4, TCP A responds with an empty segment containing an ACK for TCP B's SYN; and in line 5, TCP A sends some data.

4行目で、TCP Aは、TCP BのSYNに対するACKを含む空のセグメントで応答します。 5行目では、TCP Aがデータを送信します。

>Note that the sequence number of the segment in line 5 is the same as in line 4 because the ACK does not occupy sequence number space (if it did, we would wind up ACKing ACK's!).

5行目のセグメントのシーケンス番号は、ACKがシーケンス番号スペースを占有しないため、4行目と同じであることに注意してください（ACKであれば、ACKを返すようになります）。

>Simultaneous initiation is only slightly more complex, as is shown in figure 8.

図8に示すように、同時開始はわずかに複雑です。

>Each TCP cycles from CLOSED to SYN-SENT to SYN-RECEIVED to ESTABLISHED.

各TCPは、CLOSEDからSYN-SENTまで、SYN-RECEIVEDからESTABLISHEDまで循環します。

```

      TCP A                                            TCP B

  1.  CLOSED                                           CLOSED

  2.  SYN-SENT     --> <SEQ=100><CTL=SYN>              ...

  3.  SYN-RECEIVED <-- <SEQ=300><CTL=SYN>              <-- SYN-SENT

  4.               ... <SEQ=100><CTL=SYN>              --> SYN-RECEIVED

  5.  SYN-RECEIVED --> <SEQ=100><ACK=301><CTL=SYN,ACK> ...

  6.  ESTABLISHED  <-- <SEQ=300><ACK=101><CTL=SYN,ACK> <-- SYN-RECEIVED

  7.               ... <SEQ=101><ACK=301><CTL=ACK>     --> ESTABLISHED

```

<div style="text-align: center;">
Simultaneous Connection Synchronization  Figure 8.
</div>

>The principle reason for the three-way handshake is to prevent old duplicate connection initiations from causing confusion.

3ウェイハンドシェイクの主な理由は、古い重複した接続開始が混乱を引き起こさないようにすることです。

>To deal with this, a special control message, reset, has been devised.

これに対処するために、特別な制御メッセージresetが考案されている。

>If the receiving TCP is in a  non-synchronized state (i.e., SYN-SENT, SYN-RECEIVED), it returns to LISTEN on receiving an acceptable reset.

受信側のTCPが非同期状態（すなわち、SYN-SENT、SYN-RECEIVED）である場合、(受信可能な)リセットを受信するとLISTENに戻る。

>If the TCP is in one of the synchronized states (ESTABLISHED, FIN-WAIT-1, FIN-WAIT-2, CLOSE-WAIT, CLOSING, LAST-ACK, TIME-WAIT), it aborts the connection and informs its user.

TCPが同期状態（ESTABLISHED、FIN-WAIT-1、FIN-WAIT-2、CLOSE-WAIT、CLOSING、LAST-ACK、TIME-WAIT）のいずれかにある場合、TCPは接続を打ち切り、そのユーザーに通知します。

>We discuss this latter case under "half-open" connections below.

"ハーフオープン"のケースは後で議論する.

```

      TCP A                                                TCP B

  1.  CLOSED                                               LISTEN

  2.  SYN-SENT    --> <SEQ=100><CTL=SYN>               ...

  3.  (duplicate) ... <SEQ=90><CTL=SYN>               --> SYN-RECEIVED

  4.  SYN-SENT    <-- <SEQ=300><ACK=91><CTL=SYN,ACK>  <-- SYN-RECEIVED

  5.  SYN-SENT    --> <SEQ=91><CTL=RST>               --> LISTEN


  6.              ... <SEQ=100><CTL=SYN>               --> SYN-RECEIVED

  7.  SYN-SENT    <-- <SEQ=400><ACK=101><CTL=SYN,ACK>  <-- SYN-RECEIVED

  8.  ESTABLISHED --> <SEQ=101><ACK=401><CTL=ACK>      --> ESTABLISHED

```

<div style="text-align: center;">
Recovery from Old Duplicate SYN  Figure 9.
</div>

>As a simple example of recovery from old duplicates, consider figure 9.  At line 3, an old duplicate SYN arrives at TCP B.

古い重複からのリカバリーの簡単な例として、図9を検討してください。3行目で、古い重複SYNがTCP Bに到着します。

>TCP B cannot tell that this is an old duplicate, so it responds normally (line 4).

TCP Bはこれが古い重複であることを知ることができないので、通常通り応答します（行4）。

>TCP A detects that the ACK field is incorrect and returns a RST (reset) with its SEQ field selected to make the segment believable.

TCP AはACKフィールドが間違っていることを検出し、そのSEQフィールドを選択してセグメントを信憑性のあるものにするようなRST（リセット）を返す.

>TCP B, on receiving the RST, returns to the LISTEN state.

TCP Bは、RSTを受信すると、LISTEN状態に戻る。

>When the original SYN (pun intended) finally arrives at line 6, the synchronization proceeds normally.

元のSYN（意図されているpun??）が最終的に6行目に到着すると、同期は正常に進みます。

>If the SYN at line 6 had arrived before the RST, a more complex exchange might have occurred with RST's sent in both directions.

6行目のSYNがRSTの前に到着した場合、より複雑な交換が発生している可能性があり、両方向でRSTが送信されます。

#### Half-Open Connections and Other Anomalies (ハーフオープントその他の異常)

>An established connection is said to be  "half-open" if one of the TCPs has closed or aborted the connection at its end without the knowledge of the other, or if the two ends of the connection have become desynchronized owing to a crash that resulted in loss of memory.

establishedな接続は"half-connection"と言われる. TCPの一つが他への通知なくエンドとのコネクションを閉じたり中断してしまった場合.
or 
メモリの損失を招くクラッシュのために接続の2つのエンドが非同期になった場合。

>Such connections will automatically become reset if an attempt is made to send data in either direction.

このような接続は、いずれかの方向にデータを送信しようとすると自動的にリセットされます。

>However, half-open connections are expected to be unusual, and the recovery procedure is mildly involved.

しかし、half-openの接続は珍しいことが予想され、リカバリ手順は軽度に関与しています。

>If at site A the connection no longer exists, then an attempt by the user at site B to send any data on it will result in the site B TCP receiving a reset control message.

サイトAに接続が存在しなくなった場合、サイトBのユーザがデータを送信しようとすると、サイトBのTCPはリセット制御メッセージを受信します。

>Such a message indicates to the site B TCP that something is wrong, and it is expected to abort the connection.

このようなメッセージは、サイトBのTCPに何かが間違っていることを示し、接続を中止することが期待されます。

>Assume that two user processes A and B are communicating with one another when a crash occurs causing loss of memory to A's TCP.

2つのユーザプロセスAとBが互いに通信シアていて, AのTCPでメモリ不足が原因で破棄が発生したとする.

>Depending on the operating system supporting A's TCP, it is likely that some error recovery mechanism exists.

AのTCPをサポートしているオペレーティングシステムによっては、何らかのエラー回復メカニズムが存在する可能性があります。

>When the TCP is up again, A is likely to start again from the beginning or from a recovery point.

TCPが再びアップすると、Aははじめまたはリカバリポイントから再び開始される可能性があります。

>As a result, A will probably try to OPEN the connection again or try to SEND on the connection it believes open.

結果として、Aはおそらく接続を再度開くか、または開いていると思われる接続をSENDしようとします。

>In the latter case, it receives the error message "connection not open" from the local (A's) TCP.

後者の場合、ローカル（A）のTCPからエラーメッセージ "connection not open"を受信します。

>In an attempt to establish the connection, A's TCP will send a segment containing SYN.

接続を確立しようとすると、AのTCPはSYNを含むセグメントを送信します。

>This scenario leads to the example shown in figure 10.

このシナリオは、図10に示す例につながります。

>After TCP A crashes, the user attempts to re-open the connection.

TCP Aがクラッシュした後、ユーザーは接続を再開しようとします。

>TCP B, in the meantime, thinks the connection is open.

TCP Bはその間に, 接続が開いていると考えます。



```
		TCP A 																								TCP B

  1.  (CRASH)                               (send 300,receive 100)

  2.  CLOSED                                           ESTABLISHED

  3.  SYN-SENT --> <SEQ=400><CTL=SYN>              --> (??)

  4.  (!!)     <-- <SEQ=300><ACK=100><CTL=ACK>     <-- ESTABLISHED

  5.  SYN-SENT --> <SEQ=100><CTL=RST>              --> (Abort!!)

  6.  SYN-SENT                                         CLOSED

  7.  SYN-SENT --> <SEQ=400><CTL=SYN>              -->

```

<div style="text-align: center;">
 Half-Open Connection Discovery  Figure 10.
</div>

>When the SYN arrives at line 3, TCP B, being in a synchronized state, and the incoming segment outside the window, responds with an acknowledgment indicating what sequence it next expects to hear (ACK 100).

SYNが同期状態にある3行目、TCP Bに到着し、ウィンドウ外の着信セグメントに到着すると、次にどのシーケンスが聞こえるかを示す肯定応答が返されます（ACK 100）。

>TCP A sees that this segment does not acknowledge anything it sent and, being unsynchronized, sends a reset (RST) because it has detected a half-open connection.  TCP B aborts at line 5.

TCP Aは、このセグメントが何も確認応答しないことを確認し、非同期で、ハーフオープン接続を検出したためリセット（RST）を送信します。

>TCP A will continue to try to establish the connection; the problem is now reduced to the basic 3-way handshake of figure 7.

TCP Aは引き続き接続の確立を試みます。 この問題は現在、図7の基本的な3ウェイハンドシェイクに縮小されています。

>An interesting alternative case occurs when TCP A crashes and TCP B tries to send data on what it thinks is a synchronized connection.

TCP Aがクラッシュし、TCP Bが同期接続と考えるデータを送信しようとすると、面白い代替ケースが発生します。

>This is illustrated in figure 11.



>In this case, the data arriving at TCP A from TCP B (line 2) is unacceptable because no such connection exists, so TCP A sends a RST.

この場合、TCP B（2行目）からTCP Aに到着するデータは、そのような接続が存在しないため受け入れられないため、TCP AはRSTを送信します。

>The RST is acceptable so TCP B processes it and aborts the connection.

RSTは受け入れられ、TCP Bはそれを処理して接続を中止します。


```

 TCP A                                              TCP B

  1.  (CRASH)                                   (send 300,receive 100)

  2.  (??)    <-- <SEQ=300><ACK=100><DATA=10><CTL=ACK> <-- ESTABLISHED

  3.          --> <SEQ=100><CTL=RST>                   --> (ABORT!!)

```

<div style="text-align: center;">
Active Side Causes Half-Open Connection Discovery  Figure 11.
</div>


>In figure 12, we find the two TCPs A and B with passive connections waiting for SYN.

図12では、SYNを待っているパッシブコネクションな2つのTCP AとBが見つかりました。

>An old duplicate arriving at TCP B (line 2) stirs B into action.

TCP B（2行目）に到着した古い複製はBを動作させます。

>A SYN-ACK is returned (line 3) and causes TCP A to generate a RST (the ACK in line 3 is not acceptable).

SYN-ACKが返され（3行目）、TCP AにRSTが生成されます（3行目のACKは受け入れられません）。

>TCP B accepts the reset and returns to its passive LISTEN state.

TCP Bはリセットを受け入れ、受動LISTEN状態に戻ります。

```

      TCP A                                         TCP B

  1.  LISTEN                                        LISTEN

  2.       ... <SEQ=Z><CTL=SYN>                -->  SYN-RECEIVED

  3.  (??) <-- <SEQ=X><ACK=Z+1><CTL=SYN,ACK>   <--  SYN-RECEIVED

  4.       --> <SEQ=Z+1><CTL=RST>              -->  (return to LISTEN!)

  5.  LISTEN                                        LISTEN

```

<div style="text-align: center;">
Old Duplicate SYN Initiates a Reset on two Passive Sockets  Figure 12.
</div>


>A variety of other cases are possible, all of which are accounted for by the following rules for RST generation and processing.

他のさまざまなケースが可能であり、そのすべてがRSTの生成および処理のための以下のルールによって説明される。


**Reset Generation**

>As a general rule, reset (RST) must be sent whenever a segment arrives which apparently is not intended for the current connection.

一般的には、現在の接続を意図していないセグメントが到着するたびに、リセット（RST）を送信する必要があります。

>A reset must not be sent if it is not clear that this is the case.

このようなケースがはっきりしない場合は、リセットを送信してはなりません。


>There are three groups of states:



>1. If the connection does not exist (CLOSED) then a reset is sent in response to any incoming segment except another reset.  In particular, SYNs addressed to a non-existent connection are rejected by this means.

接続が存在しない場合（CLOSED）、別のリセット以外の着信セグメントに応答してリセットが送信されます。 特に、存在しない接続に宛てられたSYNは、この手段によって拒否されます。

>If the incoming segment has an ACK field, the reset takes its sequence number from the ACK field of the segment, otherwise the reset has sequence number zero and the ACK field is set to the sum of the sequence number and segment length of the incoming segment.

着信セグメントがACKフィールドを有する場合、リセットはセグメントのACKフィールドからそのシーケンス番号をとり、そうでなければリセットはシーケンス番号0を有し、ACKフィールドは着信セグメントのシーケンス番号とセグメント長の合計に設定される 。

>The connection remains in the CLOSED state.

接続はCLOSED状態のままです。

>2. If the connection is in any non-synchronized state (LISTEN, SYN-SENT, SYN-RECEIVED), and the incoming segment acknowledges something not yet sent (the segment carries an unacceptable ACK), or if an incoming segment has a security level or compartment which does not exactly match the level and compartment requested for the connection, a reset is sent.

接続が非同期状態（LISTEN、SYN-SENT、SYN-RECEIVED）であり、着信セグメントがまだ送信されていないものを認識した場合（セグメントに許容できないACKが含まれている）、または着信セグメントにセキュリティレベルがある場合、 接続が要求されたレベルとコンパートメントに正確に一致しない場合、リセットが送信されます。

















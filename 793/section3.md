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

>If our SYN has not been acknowledged and the precedence level of the incoming segment is higher than the precedence level requested then either raise the local precedence level (if allowed by the user and the system) or send a reset; or if the precedence level of the incoming segment is lower than the precedence level requested then continue as if the precedence matched exactly (if the remote TCP cannot raise the precedence level to match ours this will be detected in the next segment it sends, and the connection will be terminated then).

SYNが確認されておらず、着信セグメントの優先順位レベルが要求された優先順位レベルよりも高い場合は、ローカル優先順位レベルを上げる（ユーザーとシステムによって許可されている場合）か、リセットを送信します。 あるいは、入力セグメントの優先順位レベルが要求された優先順位レベルよりも低い場合は、優先順位が完全に一致するかのように継続します（リモートTCPが優先順位レベルを上げることができない場合、これは次のセグメントで検出されます）。 その後接続は終了します）。

>If our SYN has been acknowledged (perhaps in this incoming segment) the precedence level of the incoming segment must match the local precedence level exactly, if it does not a reset must be sent.

我々のSYNが（おそらくこの入力セグメントにおいて）確認されたならば、入力セグメントの優先順位レベルはローカル優先順位レベルと正確に一致しなければならず、それがリセットされなければ送信されなければならない。

>If the incoming segment has an ACK field, the reset takes its sequence number from the ACK field of the segment, otherwise the reset has sequence number zero and the ACK field is set to the sum of the sequence number and segment length of the incoming segment.

着信セグメントにACKフィールドがある場合、リセットはそのセグメントのACKフィールドからシーケンス番号を取得します。それ以外の場合、リセットのシーケンス番号は0になり、ACKフィールドは着信セグメントのシーケンス番号とセグメント長の合計に設定されます。 

>The connection remains in the same state.

接続は同じ状態のままです。

>3.  If the connection is in a synchronized state (ESTABLISHED, FIN-WAIT-1, FIN-WAIT-2, CLOSE-WAIT, CLOSING, LAST-ACK, TIME-WAIT), any unacceptable segment (out of window sequence number or unacceptible acknowledgment number) must elicit only an empty acknowledgment segment containing the current send-sequence number and an acknowledgment indicating the next sequence number expected to be received, and the connection remains in the same state.

接続が同期状態（ESTABLISHED、FIN-WAIT-1、FIN-WAIT-2、CLOSE-WAIT、CLOSING、LAST-ACK、TIME-WAIT）の場合、許容できないセグメント（ウィンドウシーケンス番号外または認識不能な確認応答） は、現在の送信シーケンス番号を含む空の確認応答セグメントと、受信されると予想される次のシーケンス番号を示す確認応答のみを引き出す必要があります。接続は同じ状態のままです。

>If an incoming segment has a security level, or compartment, or precedence which does not exactly match the level, and compartment, and precedence requested for the connection,a reset is sent and connection goes to the CLOSED state.

着信セグメントが、セキュリティー・レベル、コンパートメント、または接続に要求されたレベル、コンパートメント、および優先順位と完全には一致しない優先順位を持っている場合、リセットが送信され、接続はCLOSED状態になります。

>The reset takes its sequence number from the ACK field of the incoming segment.

>Reset Processing

>In all states except SYN-SENT, all reset (RST) segments are validated by checking their SEQ-fields.

SYN-SENTを除くすべての状態で、すべてのリセット（RST）セグメントはそれらのSEQフィールドをチェックすることによって検証されます。

>A reset is valid if its sequence number is in the window.

シーケンス番号がウィンドウ内にある場合、リセットは有効です。

>In the SYN-SENT state (a RST received in response to an initial SYN), the RST is acceptable if the ACK field acknowledges the SYN.

SYN-SENT状態（初期SYNに応答して受信されたRST）では、ACKフィールドがSYNを確認応答すれば、RSTは許容可能である。

>The receiver of a RST first validates it, then changes state.

RSTの受信側はまずそれを検証し、次に状態を変更します。

>If the receiver was in the LISTEN state, it ignores it.

受信機がLISTEN状態にあったなら、それはそれを無視します。

>If the receiver was in SYN-RECEIVED state and had previously been in the LISTEN state, then the receiver returns to the LISTEN state, otherwise the receiver aborts the connection and goes to the CLOSED state.

受信機がSYN−RECEIVED状態にあり、以前にLISTEN状態にあったならば、受信機はLISTEN状態に戻り、そうでなければ受信機は接続を中止してCLOSED状態に進む。

>If the receiver was in any other state, it aborts the connection and advises the user and goes to the CLOSED state.

受信機が他の状態にあったならば、それは接続を中止して、ユーザに助言してCLOSED状態に行きます。


### 3.5.Closing a Connection

>CLOSE is an operation meaning "I have no more data to send."  The notion of closing a full-duplex connection is subject to ambiguous interpretation, of course, since it may not be obvious how to treat the receiving side of the connection.  We have chosen to treat CLOSE in a simplex fashion.

CLOSEは「送信するデータがもうない」という意味の操作です。 全二重接続を閉じるという概念は、接続の受信側をどのように処理するかが明確でない場合があるため、もちろんあいまいな解釈に従います。 私たちはCLOSEをシンプレックス様式で扱うことを選んだ。

>We have chosen to treat CLOSE in a simplex fashion.

私たちはCLOSEをシンプレックス様式で扱うことを選んだ。

>The user who CLOSEs may continue to RECEIVE until he is told that the other side has CLOSED also.

CLOSEしたユーザは、相手側もCLOSEDになったと言われるまでRECEIVEを続けます。

>Thus, a program could initiate several SENDs followed by a CLOSE, and then continue to RECEIVE until signaled that a RECEIVE failed because the other side has CLOSED.

したがって、プログラムはいくつかのSENDとそれに続くCLOSEを開始し、反対側がCLOSEDであるためにRECEIVEが失敗したことを知らせるまでRECEIVEを続けることができます。

>We assume that the TCP will signal a user, even if no RECEIVEs are outstanding, that the other side has closed, so the user can terminate his side gracefully.

たとえRECEIVEが未解決でなくても、相手側が閉じていることをTCPがユーザに知らせるので、ユーザは自分の側を適切に終了できると仮定します。

>A TCP will reliably deliver all buffers SENT before the connection was CLOSED so a user who expects no data in return need only wait to hear the connection was CLOSED successfully to know that all his data was received at the destination TCP.

TCPは、接続がクローズされる前にすべてのバッファSENTを確実に配信するので、データが返ってこないと予想されるユーザは、接続が正常にクローズされたことを確認するために待つ必要があります。

>Users must keep reading connections they close for sending until the TCP says no more data.

TCPがそれ以上データを示さなくなるまで、ユーザは送信のために閉じる接続を読み続けなければなりません。

>There are essentially three cases:

>1) The user initiates by telling the TCP to CLOSE the connection

1) ユーザはTCPに接続を閉じるように指示することによって開始します 

>2) The remote TCP initiates by sending a FIN control signal

リモートTCPはFIN制御信号を送信することによって開始します

>3) Both users CLOSE simultaneously

両方のユーザーが同時に閉じる

>Case 1:  Local user initiates the close

>In this case, a FIN segment can be constructed and placed on the outgoing segment queue.

この場合、FINセグメントを作成して発信セグメントキューに入れることができます。

>No further SENDs from the user will be accepted by the TCP, and it enters the FIN-WAIT-1 state.

ユーザからのそれ以上のSENDはTCPによって受け入れられず、FIN-WAIT-1状態に入ります。

>RECEIVEs are allowed in this state.

この状態ではRECEIVEが許可されます。

>All segments preceding and including FIN will be retransmitted until acknowledged.

FINの前後のすべてのセグメントは、確認されるまで再送信されます。

>When the other TCP has both acknowledged the FIN and sent a FIN of its own, the first TCP can ACK this FIN.

他のTCPがFINを承認し、それ自身のFINを送信したとき、最初のTCPはこのFINをACKすることができます。

>Note that a TCP receiving a FIN will ACK but not send its own FIN until its user has CLOSED the connection also.

FINを受信したTCPはACKを送信しますが、そのユーザーが接続を閉じるまで自身はFINを送信しないことに注意.

>Case 2:  TCP receives a FIN from the network

>If an unsolicited FIN arrives from the network, the receiving TCP can ACK it and tell the user that the connection is closing.

未承諾のFINがネットワークから到着した場合、受信側TCPはそれをACKし、接続が閉じていることをユーザーに知らせることができます。

>The user will respond with a CLOSE, upon which the TCP can send a FIN to the other TCP after sending any remaining data.

全ての残っているデータも送った後に他のTCPにFINを送ることでCLOSEに応答する.

>The TCP then waits until its own FIN is acknowledged whereupon it deletes the connection.

TCPはそれからそれ自身のFINが確認されるまで待機し、そこで接続を削除します。

>If an ACK is not forthcoming, after the user timeout the connection is aborted and the user is told.

ACKが送信されない場合は、ユーザーのタイムアウト後に接続が中断され、ユーザーに通知されます。

>Case 3:  both users close simultaneously (同時CLOSE)

>A simultaneous CLOSE by users at both ends of a connection causes FIN segments to be exchanged.

接続の両端でユーザが同時に閉じると、FINセグメントが交換されます。

>When all segments preceding the FINs have been processed and acknowledged, each TCP can ACK the FIN it has received.

FINに先行するすべてのセグメントが処理され確認されると、各TCPは受信したFINにACKを送信できます。

>Both will, upon receiving these ACKs, delete the connection.

両方とも、これらのACKを受信すると、接続を削除します。

```
     TCP A                                                TCP B

  1.  ESTABLISHED                                          ESTABLISHED

  2.  (Close)
      FIN-WAIT-1  --> <SEQ=100><ACK=300><CTL=FIN,ACK>  --> CLOSE-WAIT

  3.  FIN-WAIT-2  <-- <SEQ=300><ACK=101><CTL=ACK>      <-- CLOSE-WAIT

  4.                                                       (Close)
      TIME-WAIT   <-- <SEQ=300><ACK=101><CTL=FIN,ACK>  <-- LAST-ACK

  5.  TIME-WAIT   --> <SEQ=101><ACK=301><CTL=ACK>      --> CLOSED

  6.  (2 MSL)
      CLOSED
```

<div style="text-align: center;">
Figure 13.  Normal Close Sequence 
</div>


```
      TCP A                                                TCP B

  1.  ESTABLISHED                                          ESTABLISHED

  2.  (Close)                                              (Close)
      FIN-WAIT-1  --> <SEQ=100><ACK=300><CTL=FIN,ACK>  ... FIN-WAIT-1
                  <-- <SEQ=300><ACK=100><CTL=FIN,ACK>  <--
                  ... <SEQ=100><ACK=300><CTL=FIN,ACK>  -->

  3.  CLOSING     --> <SEQ=101><ACK=301><CTL=ACK>      ... CLOSING
                  <-- <SEQ=301><ACK=101><CTL=ACK>      <--
                  ... <SEQ=101><ACK=301><CTL=ACK>      -->

  4.  TIME-WAIT                                            TIME-WAIT
      (2 MSL)                                              (2 MSL)
      CLOSED                                               CLOSED
```

<div style="text-align: center;">
Figure 14.  Simultaneous Close Sequence
</div>


### 3.6.  Precedence and Security

>The intent is that connection be allowed only between ports operating with exactly the same security and compartment values and at the higher of the precedence level requested by the two ports.

その目的は、まったく同じセキュリティ値とコンパートメント値で動作しているポート間、および2つのポートによって要求されている優先順位レベルの高いポート間でのみ接続が許可されることです。

>The precedence and security parameters used in TCP are exactly those defined in the Internet Protocol (IP) [2].

TCPで使用される優先順位とセキュリティのパラメータは、まさにインターネットプロトコル（IP）[2]で定義されているものです。

>Throughout this TCP specification the term "security/compartment" is intended to indicate the security parameters used in IP including security, compartment, user group, and handling restriction.

このTCP仕様を通して、「セキュリティ/コンパートメント」という用語は、セキュリティ、コンパートメント、ユーザグループ、および取り扱い制限を含む、IPで使用されるセキュリティパラメータを示すことを目的としています。

>A connection attempt with mismatched security/compartment values or a lower precedence value must be rejected by sending a reset.

セキュリティ/コンパートメントの値が一致しない、または優先順位の値が低い接続の試行は、リセットを送信して拒否する必要があります。

>Rejecting a connection due to too low a precedence only occurs after an acknowledgment of the SYN has been received.

優先順位が低すぎるために接続を拒否するのは、SYNの確認応答を受信した後に限ります。

>Note that TCP modules which operate only at the default value of precedence will still have to check the precedence of incoming segments and possibly raise the precedence level they use on the connection.

優先順位のデフォルト値でのみ動作するTCPモジュールは、着信セグメントの優先順位を確認し、接続で使用する優先順位レベルを上げる必要があることに注意してください。

>The security paramaters may be used even in a non-secure environment (the values would indicate unclassified data), thus hosts in non-secure environments must be prepared to receive the security parameters, though they need not send them.

セキュリティパラメータは、安全でない環境でも使用される可能性があるため（値は未分類のデータを示します）、安全でない環境のホストはセキュリティパラメータを受信しないように準備する必要があります。

### 3.7.  Data Communication

>Once the connection is established data is communicated by the exchange of segments.

接続が確立されると、データはセグメントの交換によって通信されます。

>Because segments may be lost due to errors (checksum test failure), or network congestion, TCP uses retransmission (after a timeout) to ensure delivery of every segment.

セグメントはエラー（チェックサムテストの失敗）またはネットワークの輻輳のために失われる可能性があるため、TCPはすべてのセグメントの配信を保証するために（タイムアウト後の）再送信を使用します。

>Duplicate segments may arrive due to network or TCP retransmission.

ネットワークまたはTCPの再送信により、重複するセグメントが到着する可能性があります。

>As discussed in the section on sequence numbers the TCP performs certain tests on the sequence and acknowledgment numbers in the segments to verify their acceptability.

シーケンス番号のセクションで説明したように、TCPはセグメント内のシーケンス番号と確認応答番号について特定のテストを実行して、それらの受け入れ可能性を検証します。

>The sender of data keeps track of the next sequence number to use in the variable SND.NXT.

データの送信者は、変数SND.NXTで使用する次のシーケンス番号を追跡します。

>The receiver of data keeps track of the next sequence number to expect in the variable RCV.NXT.

データの受信側は、変数RCV.NXTに期待される次のシーケンス番号を追跡します。

>The sender of data keeps track of the oldest unacknowledged sequence number in the variable SND.UNA.

データの送信者は、変数SND.UNA内の最も古い未確認シーケンス番号を追跡します。

>If the data flow is momentarily idle and all data sent has been acknowledged then the three variables will be equal.

データフローが一時的にアイドルで、送信されたすべてのデータが確認応答されている場合、3つの変数は等しくなります。

>When the sender creates a segment and transmits it the sender advances SND.NXT.

送信者がセグメントを作成して送信すると、送信者はSND.NXTを進めます。

>When the receiver accepts a segment it advances RCV.NXT and sends an acknowledgment.

受信側がセグメントを受け入れると、RCV.NXTを進めて確認応答を送信します。

>When the data sender receives an acknowledgment it advances SND.UNA.

データ送信者が確認応答を受信すると、SND.UNAに進みます。

>The extent to which the values of these variables differ is a measure of the delay in the communication.

これらの変数の値がどの程度異なるかは、通信における遅延の尺度です。

>The amount by which the variables are advanced is the length of the data in the segment.

変数を進める量は、セグメント内のデータの長さです。

>Note that once in the ESTABLISHED state all segments must carry current acknowledgment information.

ESTABLISHED状態になると、すべてのセグメントが現在の確認応答情報を伝えなければならないことに注意してください。

>The CLOSE user call implies a push function, as does the FIN control flag in an incoming segment.

CLOSEユーザー呼び出しは、着信セグメントのFIN制御フラグと同様に、プッシュ機能を暗黙指定します。

#### Retransmission Timeout

>Because of the variability of the networks that compose an internetwork system and the wide range of uses of TCP connections the retransmission timeout must be dynamically determined.

インターネットワークシステムを構成するネットワークの多様性とTCP接続の広範囲の使用法のために、再送タイムアウトは動的に決定されなければなりません。

>One procedure for determining a retransmission time out is given here as an illustration.

再送信タイムアウトを決定するための１つの手順が例示としてここに与えられる。

#### An Example Retransmission Timeout Procedure

>Measure the elapsed time between sending a data octet with a particular sequence number and receiving an acknowledgment that covers that sequence number (segments sent do not have to match segments received).

特定のシーケンス番号でデータオクテットを送信してから、そのシーケンス番号をカバーする確認応答を受信するまでの経過時間を測定します（送信されたセグメントは受信したセグメントと一致する必要はありません）。

>This measured elapsed time is the Round Trip Time (RTT).

この測定された経過時間は往復時間（RTT）です。

>Next compute a Smoothed Round Trip Time (SRTT)

次に平滑化往復時間（SRTT）を計算します。

```
as:
        SRTT = ( ALPHA * SRTT ) + ((1-ALPHA) * RTT)
      and based on this, compute the retransmission timeout (RTO)
as:
        RTO = min[UBOUND,max[LBOUND,(BETA*SRTT)]]
```

>where UBOUND is an upper bound on the timeout (e.g., 1 minute), LBOUND is a lower bound on the timeout (e.g., 1 second), ALPHA is a smoothing factor (e.g., .8 to .9), and BETA is a delay variance factor (e.g., 1.3 to 2.0).

ここで、UBOUNDはタイムアウトの上限（たとえば1分）、LBOUNDはタイムアウトの下限（たとえば1秒）、ALPHAは平滑化係数（たとえば.8から.9）、そしてBETAは 遅延分散係数（例：1.3〜2.0）

#### The Communication of Urgent Information

>The objective of the TCP urgent mechanism is to allow the sending user to stimulate the receiving user to accept some urgent data and to permit the receiving TCP to indicate to the receiving user when all the currently known urgent data has been received by the user.

ＴＣＰ緊急メカニズムの目的は、送信側ユーザが受信側ユーザに何らかの緊急データの受け入れを促し、現在知られているすべての緊急データがユーザによって受信されたときに受信側ＴＣＰが受信側ユーザに知らせることを可能にすることである。

>This mechanism permits a point in the data stream to be designated as the end of urgent information.

このメカニズムにより、データストリーム内のポイントを緊急情報の終わりとして指定することができます。

>Whenever this point is in advance of the receive sequence number (RCV.NXT) at the receiving TCP, that TCP must tell the user to go into "urgent mode"; when the receive sequence number catches up to the urgent pointer, the TCP must tell user to go into "normal mode".  If the urgent pointer is updated while the user is in "urgent mode", the update will be invisible to the user.

この時点が受信側TCPの受信シーケンス番号（RCV.NXT）より前である場合は常に、そのTCPはユーザーに「緊急モード」に入るように指示する必要があります。 受信シーケンス番号が緊急ポインタに追いつくと、TCPはユーザに「通常モード」に入るように伝えなければなりません。 ユーザが「緊急モード」にある間に緊急ポインタが更新されると、その更新はユーザには見えなくなる。

>The method employs a urgent field which is carried in all segments transmitted.

この方法は、送信されたすべてのセグメントで運ばれるurgent fieldを使用する。

>The URG control flag indicates that the urgent field is meaningful and must be added to the segment sequence number to yield the urgent pointer.

URG制御フラグは、緊急フィールドが意味を持ち、緊急ポインタを生成するためにセグメントシーケンス番号に追加されなければならないことを示します。

>The absence of this flag indicates that there is no urgent data outstanding.

このフラグがないことは、未処理の緊急データがないことを示します。

>To send an urgent indication the user must also send at least one data octet.

緊急表示を送信するには、ユーザーは少なくとも1つのデータオクテットも送信する必要があります。

>If the sending user also indicates a push, timely delivery of the urgent information to the destination process is enhanced.

送信側ユーザもプッシュを指示した場合、宛先プロセスへの緊急情報のタイムリーな配信が向上します。

#### Managing the Window

>The window sent in each segment indicates the range of sequence numbers the sender of the window (the data receiver) is currently prepared to accept.

各セグメントで送信されるウィンドウは、ウィンドウの送信者（データ受信者）が現在受け入れる準備ができているシーケンス番号の範囲を示します。

>There is an assumption that this is related to the currently available data buffer space available for this connection.

これはこの接続に利用可能な現在利用可能なデータバッファスペースに関連しているという仮定があります。

>Indicating a large window encourages transmissions.

大きなウィンドウを表示すると、送信が促進されます。

>If more data arrives than can be accepted, it will be discarded.

受け入れられるよりも多くのデータが到着すると、それは破棄されます。

>This will result in excessive retransmissions, adding unnecessarily to the load on the network and the TCPs.

これは過度の再送信をもたらし、ネットワークとTCPの負荷を不必要に増やします。

>Indicating a small window may restrict the transmission of data to the point of introducing a round trip delay between each new segment transmitted.

小さいウィンドウを指示することは、送信される各新しいセグメント間に往復遅延を導入する点までデータの送信を制限することがある。

>The mechanisms provided allow a TCP to advertise a large window and to subsequently advertise a much smaller window without having accepted that much data.

提供されているメカニズムは、TCPが大きなウィンドウをアドバタイズし、その後、それほど多くのデータを受け入れずに、はるかに小さなウィンドウをアドバタイズすることを可能にします。

>This, so called "shrinking the window," is strongly discouraged.

これはいわゆる縮小ウィンドと言われています.

>The robustness principle dictates that TCPs will not shrink the window themselves, but will be prepared for such behavior on the part of other TCPs.

ロバスト性の原則は、TCPはウィンドウ自体を縮小するのではなく、他のTCP側のそのような振る舞いに備えて準備することを指示します。

>The sending TCP must be prepared to accept from the user and send at least one octet of new data even if the send window is zero.

送信ウィンドウがゼロであっても、送信側TCPはユーザーから受け入れて新しいデータを少なくとも1オクテット送信する準備をしておく必要があります。

>The sending TCP must regularly retransmit to the receiving TCP even when the window is zero.

ウィンドウがゼロの場合でも、送信側TCPは受信側TCPに定期的に再送信する必要があります。

>Two minutes is recommended for the retransmission interval when the window is zero.

ウィンドウがゼロのときの再送信間隔は2分が推奨されます。

>This retransmission is essential to guarantee that when either TCP has a zero window the re-opening of the window will be reliably reported to the other.

この再送信は、どちらかのTCPがゼロウィンドウを持っているときにウィンドウの再オープンがもう一方に確実に報告されることを保証するために不可欠です。

>When the receiving TCP has a zero window and a segment arrives it must still send an acknowledgment showing its next expected sequence number and current window (zero).

受信側TCPにゼロウィンドウがあり、セグメントが到着したとき、それはまだその次の期待されるシーケンス番号と現在のウィンドウ（ゼロ）を示す確認応答を送信しなければなりません。

>The sending TCP packages the data to be transmitted into segments which fit the current window, and may repackage segments on the retransmission queue.

送信側TCPは、送信するデータを現在のウィンドウに合うセグメントにパッケージ化し、再送信キューにセグメントを再パッケージ化することがあります。

>Such repackaging is not required, but may be helpful.

そのような再パッケージ化は必須ではありませんが、役に立つかもしれません。

>In a connection with a one-way data flow, the window information will be carried in acknowledgment segments that all have the same sequence number so there will be no way to reorder them if they arrive out of order.

一方向のデータフローに関連して、ウィンドウ情報は、すべて同じシーケンス番号を持つ確認応答セグメントで伝達されるため、順序がずれて到着した場合に並べ替えることはできません。

>This is not a serious problem, but it will allow the window information to be on occasion temporarily based on old reports from the data receiver.

これは深刻な問題ではありませんが、データ受信者からの古いレポートに基づいてウィンドウ情報が一時的に表示されることを可能にします。

>A refinement to avoid this problem is to act on the window information from segments that carry the highest acknowledgment number (that is segments with acknowledgment number equal or greater than the highest previously received).

この問題を回避するための改良点は、最大の確認応答番号を搬送するセグメント（すなわち、以前に受信した最大のものと等しいかそれより大きい確認応答番号を有するセグメント）からのウィンドウ情報に作用することである。

>The window management procedure has significant influence on the communication performance.  The following comments are suggestions to implementers.

ウィンドウ管理手順は通信性能に大きな影響を与えます。 以下のコメントは実装者への提案です。

#### Window Management Suggestions

>Allocating a very small window causes data to be transmitted in many small segments when better performance is achieved using fewer large segments.

非常に小さいウィンドウを割り振ると、少数の大きいセグメントを使用してよりよいパフォーマンスが達成されるときに、データが多数の小さいセグメントで送信されることになります。

>One suggestion for avoiding small windows is for the receiver to defer updating a window until the additional allocation is at least X percent of the maximum allocation possible for the connection (where X might be 20 to 40).

小さなウィンドウを回避するための１つの提案は、追加割り当てが接続に可能な最大割り当ての少なくともＸパーセントになるまで受信機がウィンドウの更新を延期することである（Ｘは２０から４０であり得る）。

>Another suggestion is for the sender to avoid sending small segments by waiting until the window is large enough before sending data.  If the the user signals a push function then the data must be sent even if it is a small segment.

別の提案は、データが送信される前にウィンドウが十分に大きくなるまで待つことによって送信者が小さなセグメントを送信することを回避することです。 ユーザがプッシュ機能を信号で伝えると、たとえそれが小さなセグメントであってもデータは送られなければならない。

>Note that the acknowledgments should not be delayed or unnecessary retransmissions will result.

確認応答が遅れるべきではないか、または不必要な再送信が結果として生じることに注意してください。

>One strategy would be to send an acknowledgment when a small segment arrives (with out updating the window information), and then to send another acknowledgment with new window information when the window is larger.

1つの戦略は、（ウィンドウ情報を更新せずに）小さなセグメントが到着したときに確認応答を送信し、次にウィンドウが大きいときに新しいウィンドウ情報を使用して別の確認応答を送信することです。

>The segment sent to probe a zero window may also begin a break up of transmitted data into smaller and smaller segments.

ゼロウィンドウを探索するために送信されたセグメントはまた、送信されたデータをますます小さなセグメントに分割することを開始し得る。

>If a segment containing a single data octet sent to probe a zero window is accepted, it consumes one octet of the window now available.

ゼロウィンドウをプローブするために送信された単一のデータオクテットを含むセグメントが受け入れられると、それは現在利用可能なウィンドウの1オクテットを消費します。

>If the sending TCP simply sends as much as it can whenever the window is non zero, the transmitted data will be broken into alternating big and small segments.

送信側TCPが、ウィンドウがゼロ以外のときにできる限り送信するだけでは、送信データは大小のセグメントに交互に分割されます。

>As time goes on, occasional pauses in the receiver making window allocation available will result in breaking the big segments into a small and not quite so big pair. And after a while the data transmission will be in mostly small segments.  

時間が経つにつれて、ウィンドウ割り当てを利用可能にする受信機の時折の休止は、大きなセグメントをそれほど大きくないペアに分割することになります。 そしてしばらくすると、データ転送はほとんど小さなセグメントになります。

>The suggestion here is that the TCP implementations need to actively attempt to combine small window allocations into larger windows, since the mechanisms for managing the window tend to lead to many small windows in the simplest minded implementations.

ここでの提案は、ウィンドウを管理するためのメカニズムが最も単純な志向の実装では多くの小さなウィンドウにつながる傾向があるので、TCP実装は小さなウィンドウ割り当てをより大きなウィンドウに積極的に組み合わせることを試みる必要があるということです。

### 3.8.  Interfaces

>There are of course two interfaces of concern:  the user/TCP interface and the TCP/lower-level interface.

当然のことながら、ユーザー/ TCPインターフェースとTCP /下位インターフェースの2つのインターフェースがあります。

>We have a fairly elaborate model of the user/TCP interface, but the interface to the lower level protocol module is left unspecified here, since it will be specified in detail by the specification of the lowel level protocol.

私たちはユーザ/ TCPインタフェースのかなり複雑なモデルを持っています、しかし、それが低レベルプロトコルの仕様によって詳細に指定されるので、低レベルプロトコルモジュールへのインタフェースはここで指定されないままにされます。

>For the case that the lower level is IP we note some of the parameter values that TCPs might use.

より低いレベルがIPである場合に関しては私達はTCPが使用するかもしれないパラメータ値のいくつかに注意します。

#### User/TCP Interface

>The following functional description of user commands to the TCP is, at best, fictional, since every operating system will have different facilities.

TCPに対するユーザコマンドの以下の機能的な説明は、せいぜい架空のものです。すべてのオペレーティングシステムが異なる機能を持つためです。

>Consequently, we must warn readers that different TCP implementations may have different user interfaces.

その結果、私たちは読者に異なるTCP実装が異なるユーザーインターフェースを持つかもしれないことを警告しなければなりません。

>However, all TCPs must provide a certain minimum set of services to guarantee that all TCP implementations can support the same protocol hierarchy.

ただし、すべてのTCP実装が同じプロトコル階層をサポートできることを保証するために、すべてのTCPは特定の最小限のサービスセットを提供する必要があります。

>This section specifies the functional interfaces required of all TCP implementations.

このセクションはすべてのTCP実装に必要な機能的なインタフェースを指定します。

#### TCP User Commands

>The following sections functionally characterize a USER/TCP interface.

以下の節では、USER / TCPインタフェースを機能的に特徴付けます。

>The notation used is similar to most procedure or function calls in high level languages, but this usage is not meant to rule out trap type service calls (e.g., SVCs, UUOs, EMTs).

使用される表記法は、高水準言語におけるほとんどの手続きまたは関数呼び出しに似ていますが、この使用法はトラップ型サービス呼び出し（例えば、SVC、UUO、EMT）を除外することを意図していません。

>The user commands described below specify the basic functions the TCP must perform to support interprocess communication.

下記のユーザコマンドは、TCPがプロセス間通信をサポートするために実行しなければならない基本機能を指定します。

>Individual implementations must define their own exact format, and may provide combinations or subsets of the basic functions in single calls.

個々のインプリメンテーションはそれら自身の正確なフォーマットを定義しなければならず、そして単一の呼び出しで基本機能の組み合わせまたはサブセットを提供するかもしれません。

>In particular, some implementations may wish to automatically OPEN a connection on the first SEND or RECEIVE issued by the user for a given connection.

特に、いくつかの実装は、与えられた接続に対してユーザによって発行された最初のSENDまたはRECEIVEで自動的に接続を開くことを望むかもしれません。

>In providing interprocess communication facilities, the TCP must not only accept commands, but must also return information to the processes it serves.

プロセス間通信機能を提供する際、TCPはコマンドを受け付けるだけでなく、サービスを提供しているプロセスに情報を返さなければなりません。

>The latter consists of:

>(a) general information about a connection (e.g., interrupts, remote close, binding of unspecified foreign socket).

接続に関する一般的な情報（例：割り込み、リモートクローズ、未指定の外部ソケットのバインド）。

> (b) replies to specific user commands indicating success or various types of failure.

成功またはさまざまな種類の失敗を示す特定のユーザーコマンドに応答します。

```
      Open

        Format:  OPEN (local port, foreign socket, active/passive
        [, timeout] [, precedence] [, security/compartment] [, options])
        -> local connection name
```

>We assume that the local TCP is aware of the identity of the processes it serves and will check the authority of the process to use the connection specified.

私たちは、地方のTCPがそれが役立つプロセスのアイデンティティを知っていると仮定して、指定された接続を使用するためにプロセスの権威をチェックするでしょう。

>Depending upon the implementation of the TCP, the local network and TCP identifiers the the be for the source address will either be supplied by the TCP or lower level protocol (e.g., IP).

ＴＣＰの実装に応じて、ローカルネットワークおよび送信元アドレスのためのＴＣＰ識別子は、ＴＣＰまたはより低いレベルのプロトコル（例えばＩＰ）によって供給されることになる。

>These considerations are the result of concern about security, to the extent that no TCP be able to masquerade as another one, and so on.

これらの考慮事項は、TCPが他のTCPになりすますことができないなど、セキュリティに関する懸念の結果です。

>Similarly, no process can masquerade as another without the collusion of the TCP.

同様に、TCPの共謀なしには、他のプロセスを装うことはできません。

>If the active/passive flag is set to passive, then this is a call to LISTEN for an incoming connection.

アクティブ/パッシブフラグがパッシブに設定されている場合、これは着信接続に対するLISTENへの呼び出しです。

>A passive open may have either a fully specified foreign socket to wait for a wait particular connection or an unspecified foreign socket to for any call.

パッシブオープンには、特定の接続を待つために完全に指定された外部ソケット、または任意の呼び出しを待機するための未指定の外部ソケットがあります。

>A fully specified passive call can be made active by the subsequent execution of a SEND.

その後のSENDの実行によって、完全指定の受動呼び出しをアクティブにすることができます。

>A transmission control block (TCB) is created and partially filled in with data from the OPEN command parameters.

伝送制御ブロック（TCB）が作成され、OPENコマンド・パラメーターからのデータで部分的に埋められます。

>On an active OPEN command, the TCP will begin the procedure to synchronize (i.e., establish) the connection at once.

アクティブなＯＰＥＮコマンドでは、ＴＣＰは一度に接続を同期させる（すなわち確立する）手順を開始する。

>The timeout, if present, permits the caller to set up a timeout for all data submitted to TCP.

タイムアウトが存在する場合、呼び出し側はTCPに送信されたすべてのデータに対してタイムアウトを設定できます。

>If data is not successfully delivered to the destination within the timeout period, the TCP will abort the connection.

タイムアウト期間内にデータが宛先に正常に配信されなかった場合、TCPは接続を中止します。

>The present global default is five minutes.

現在のグローバルデフォルトは5分です。

>The TCP or some component of the operating system will verify the users authority to open a connection with the specified precedence or security/compartment.

TCPまたはオペレーティングシステムの一部のコンポーネントは、指定された優先順位またはセキュリティ/コンパートメントで接続を開くためのユーザー権限を確認します。

>The absence of precedence or security/compartment specification in the OPEN call indicates the default values must be used.

OPEN呼び出しに優先順位またはセキュリティー/コンパートメントの指定がないことは、デフォルト値を使用する必要があることを示しています。

>TCP will accept incoming requests as matching only if the security/compartment information is exactly the same and only if the precedence is equal to or higher than the precedence requested in the OPEN call.

セキュリティ/コンパートメント情報がまったく同じであり、優先順位がOPEN呼び出しで要求された優先順位以上である場合に限り、TCPは着信要求を一致として受け入れます。

>The precedence for the connection is the higher of the values requested in the OPEN call and received from the incoming request, and fixed at that value for the life of the connection.

接続の優先順位は、OPEN呼び出しで要求され、着信要求から受信された値のうち高い方の値であり、接続の存続期間中その値に固定されています。

>Implementers may want to give the user control of this precedence negotiation.

実装者は、この優先順位の交渉をユーザーに制御させることができます。

>For example, the user might be allowed to specify that the precedence must be exactly matched, or that any attempt to raise the precedence be confirmed by the user.

たとえば、優先順位を完全に一致させる必要があること、または優先順位を上げるための試行をユーザーが確認することをユーザーに指定させることができます。

>A local connection name will be returned to the user by the TCP.

ローカル接続名はTCPによってユーザーに返されます。

>The local connection name can then be used as a short hand term for the connection defined by the <local socket, foreign socket> pair.

ローカル接続名は、<local socket、foreign socket>のペアで定義された接続の簡単な用語として使用できます。

```
      Send

        Format:  SEND (local connection name, buffer address, byte
        count, PUSH flag, URGENT flag [,timeout])
```

>This call causes the data contained in the indicated user buffer to be sent on the indicated connection.

この呼び出しは、指示されたユーザー・バッファーに含まれているデータを指示された接続で送信させます。

>If the connection has not been opened, the SEND is considered an error.

接続が開かれていない場合、SENDはエラーと見なされます。

>Some implementations may allow users to SEND first; in which case, an automatic OPEN would be done.

いくつかの実装はユーザが最初に送ることを可能にするかもしれません。 その場合、自動オープンが行われます。

>If the calling process is not authorized to use this connection, an error is returned.

呼び出しプロセスがこの接続を使用することを許可されていない場合は、エラーが返されます。

>If the PUSH flag is set, the data must be transmitted promptly to the receiver, and the PUSH bit will be set in the last TCP segment created from the buffer.

PUSHフラグが設定されている場合、データは受信側に速やかに送信されなければならず、PUSHビットはバッファから作成された最後のTCPセグメントに設定されます。

>If the PUSH flag is not set, the data may be combined with data from subsequent SENDs for transmission efficiency.

プッシュフラグが設定されていない場合、データは伝送効率のために後続のＳＥＮＤからのデータと組み合わされてもよい。

>If the URGENT flag is set, segments sent to the destination TCP will have the urgent pointer set.

URGENTフラグが設定されている場合、宛先TCPに送信されたセグメントには緊急ポインタが設定されます。

>The receiving TCP will signal the urgent condition to the receiving process if the urgent pointer indicates that data preceding the urgent pointer has not been consumed by the receiving process.

緊急ポインタが、緊急ポインタの前のデータが受信プロセスによって消費されていないことを示している場合、受信TCPは受信プロセスに緊急状態を通知します。

>The purpose of urgent is to stimulate the receiver to process the urgent data and to indicate to the receiver when all the currently known urgent data has been received.

緊急の目的は、緊急データを処理するように受信者を刺激し、現在知られている緊急データがすべて受信されたことを受信者に知らせることです。

>The number of times the sending user's TCP signals urgent will not necessarily be equal to the number of times the receiving user will be notified of the presence of urgent data.

送信側ユーザのＴＣＰが緊急信号を送信する回数は、受信側ユーザが緊急データの存在を通知される回数に必ずしも等しいとは限らない。

> If no foreign socket was specified in the OPEN, but the connection is established (e.g., because a LISTENing connection has become specific due to a foreign segment arriving for the local socket), then the designated buffer is sent to the implied foreign socket. 

外部ソケットがオープンに指定されていないが、接続が確立されている場合（例えば、外部ソケットがローカルソケットに到着したためにLISTENing接続が特定になったため）、指定バッファは暗黙の外部ソケットに送信される。

>Users who make use of OPEN with an unspecified foreign socket can make use of SEND without ever explicitly knowing the foreign socket address.

未指定の外部ソケットでOPENを利用するユーザーは、外部ソケットアドレスを明示的に知らなくてもSENDを利用できます。

>However, if a SEND is attempted before the foreign socket becomes specified, an error will be returned.  Users can use the STATUS call to determine the status of the connection.  In some implementations the TCP may notify the user when an unspecified socket is bound.

ただし、外部ソケットが指定される前にSENDが試行された場合は、エラーが返されます。 ユーザーはSTATUS呼び出しを使用して接続の状況を判別できます。 いくつかの実装形態では、未指定のソケットがバインドされたときにTCPがユーザに通知することがあります。

>If a timeout is specified, the current user timeout for this connection is changed to the new one.

タイムアウトが指定されている場合、この接続に対する現在のユーザタイムアウトは新しいものに変更されます。

>In the simplest implementation, SEND would not return control to the sending process until either the transmission was complete or the timeout had been exceeded.

最も単純な実装では、SENDは送信が完了するかタイムアウトを超えるまで送信プロセスに制御を返しません。

>However, this simple method is both subject to deadlocks (for example, both sides of the connection might try to do SENDs before doing any RECEIVEs) and offers poor performance, so it is not recommended.

ただし、この単純な方法はどちらもデッドロックの影響を受け（たとえば、接続の両側でRECEIVEを実行する前にSENDを実行しようとする可能性がある）、パフォーマンスが低いため、お勧めできません。

>A more sophisticated implementation would return immediately to allow the process to run concurrently with network I/O, and, furthermore, to allow multiple SENDs to be in progress.

より洗練された実装はすぐに戻って、プロセスがネットワークI / Oと同時に実行することを可能にし、さらに複数のSENDが進行中であることを可能にするでしょう。

>Multiple SENDs are served in first come, first served order, so the TCP will queue those it cannot service immediately.

複数のSENDが先着順に処理されるため、TCPはすぐには処理できないものをキューに入れます。

>We have implicitly assumed an asynchronous user interface in which a SEND later elicits some kind of SIGNAL or pseudo-interrupt from the serving TCP.  An alternative is to return a response immediately. 

SENDが後でサービングTCPからある種のシグナルまたは擬似割り込みを引き出す非同期ユーザーインターフェースを暗黙のうちに仮定しました。 別の方法はすぐに応答を返すことです。

>For instance, SENDs might return immediate local acknowledgment, even if the segment sent had not been acknowledged by the distant TCP.

例えば、送信されたセグメントが遠くのTCPによって確認応答されていなくても、SENDは即時のローカル確認応答を返すかもしれません。

>We could optimistically assume eventual success.

我々は楽観的に最終的な成功を引き受けることができた。

>If we are wrong, the connection will close anyway due to the timeout.

間違っていると、タイムアウトにより接続が切断されます。

>In implementations of this kind (synchronous), there will still be some asynchronous signals, but these will deal with the connection itself, and not with specific segments or buffers.

この種の（同期）実装では、まだ非同期信号がいくつかありますが、これらは接続自体を処理し、特定のセグメントやバッファを処理することはありません。

>In order for the process to distinguish among error or success indications for different SENDs, it might be appropriate for the  buffer address to be returned along with the coded response to the SEND request.  TCP-to-user signals are discussed below, indicating the information which should be returned to the calling process. 

プロセスが異なるSENDに対するエラーまたは成功の表示を区別するためには、SEND要求に対するコード化された応答とともにバッファアドレスを返すことが適切な場合があります。 ＴＣＰからユーザへの信号については後述するが、これは呼出しプロセスに返されるべき情報を示している。

```
    Receive

        Format:  RECEIVE (local connection name, buffer address, byte
        count) -> byte count, urgent flag, push flag
```

>This command allocates a receiving buffer associated with the specified connection.  If no OPEN precedes this command or the calling process is not authorized to use this connection, an error is returned.

このコマンドは、指定された接続に関連した受信バッファーを割り振ります。 このコマンドの前にOPENがないか、呼び出し側プロセスがこの接続を使用することを許可されていない場合、エラーが返されます。

>In the simplest implementation, control would not return to the calling program until either the buffer was filled, or some error occurred, but this scheme is highly subject to deadlocks. A more sophisticated implementation would permit several RECEIVEs to be outstanding at once. 

最も単純な実装では、バッファが一杯になるか、何らかのエラーが発生するまで、制御は呼び出し側プログラムに戻りませんが、この方式はデッドロックの影響を強く受けます。 より洗練された実装では、いくつかのRECEIVEを一度に未処理にすることができます。

>These would be filled as segments arrive.  This strategy permits increased throughput at the cost of a more elaborate scheme (possibly asynchronous) to notify the calling program that a PUSH has been seen or a buffer filled.

これらはセグメントが到着するといっぱいになります。 この方法では、PUSHが発生したこと、またはバッファがいっぱいになったことを呼び出し側プログラムに通知するための、より複雑な方法（おそらく非同期）を犠牲にしてスループットを向上させることができます。

>If enough data arrive to fill the buffer before a PUSH is seen, the PUSH flag will not be set in the response to the RECEIVE. The buffer will be filled with as much data as it can hold.  If a PUSH is seen before the buffer is filled the buffer will be returned partially filled and PUSH indicated.

PUSHが発生する前にバッファを満たすのに十分なデータが到着した場合、RECEIVEへの応答でPUSHフラグは設定されません。 バッファーには、保持できる量のデータがいっぱいになります。 バッファがいっぱいになる前にPUSHが見られる場合、バッファは部分的にいっぱいになって返され、PUSHが示されます。

>If there is urgent data the user will have been informed as soon as it arrived via a TCP-to-user signal.  The receiving user should thus be in "urgent mode".  If the URGENT flag is on, additional urgent data remains.

緊急のデータがある場合は、TCP-to-userシグナルを介して到着したらすぐにユーザーに通知されます。 したがって、受信ユーザーは「緊急モード」になっているはずです。 緊急フラグがオンの場合は、追加の緊急データが残ります。

>If the URGENT flag is off, this call to RECEIVE has returned all the urgent data, and the user may now leave "urgent mode".  Note that data following the urgent pointer (non-urgent data) cannot be delivered to the user in the same buffer with preceeding urgent data unless the boundary is clearly marked for the user.

URGENTフラグがオフの場合、RECEIVEへのこの呼び出しはすべての緊急データを返したので、ユーザーは「緊急モード」を終了することができます。 緊急ポインタに続くデータ（緊急でないデータ）は、境界がユーザに対して明確にマークされていない限り、先行する緊急データと同じバッファでユーザに配信することはできません。

>To distinguish among several outstanding RECEIVEs and to take care of the case that a buffer is not completely filled, the return code is accompanied by both a buffer pointer and a byte count indicating the actual length of the data received.

いくつかの未解決のRECEIVEを区別し、バッファーが完全にいっぱいになっていない場合に対処するために、戻りコードにはバッファー・ポインターと受信データの実際の長さを示すバイト数の両方が付随します。

>Alternative implementations of RECEIVE might have the TCP allocate buffer storage, or the TCP might share a ring buffer with the user.

RECEIVEの代替実装では、TCPにバッファストレージを割り当てさせる、またはTCPがリングバッファをユーザと共有することがあります。

```
      Close

        Format:  CLOSE (local connection name)
```

>This command causes the connection specified to be closed.  If the connection is not open or the calling process is not authorized to use this connection, an error is returned.

このコマンドにより、指定された接続が閉じられます。 接続がオープンされていないか、呼び出し側プロセスがこの接続を使用することを許可されていない場合は、エラーが返されます。

>Closing connections is intended to be a graceful operation in the sense that outstanding SENDs will be transmitted (and retransmitted), as flow control permits, until all have been serviced.

接続を閉じることは、すべてが処理されるまで、フロー制御が許す限り、未解決のSENDが送信される（そして再送信される）という意味での優雅な操作であることを意図しています。

>Thus, it should be acceptable to make several SEND calls, followed by a CLOSE, and expect all the data to be sent to the destination.

したがって、複数のSEND呼び出しに続けてCLOSEを呼び出し、すべてのデータが宛先に送信されることを想定してもかまいません。

>It should also be clear that users should continue to RECEIVE on CLOSING connections, since the other side may be trying to transmit the last of its data.

相手側が最後のデータを送信しようとしている可能性があるため、ユーザーはCLOSING接続で受信を継続する必要があることも明らかになります。

>Thus, CLOSE means "I have no more to send" but does not mean "I will not receive any more."  It may happen (if the user level protocol is not well thought out) that the closing side is unable to get rid of all its data before timing out.  In this event, CLOSE turns into ABORT, and the closing TCP gives up.

したがって、CLOSEは「送信するものがこれ以上ない」という意味ですが、「これ以上受信することはない」という意味ではありません。 （ユーザレベルのプロトコルがよく考えられていない場合は）クローズサイドがタイムアウトする前にすべてのデータを削除できない可能性があります。 この場合、CLOSEはABORTに変わり、閉じているTCPはあきらめます。

>The user may CLOSE the connection at any time on his own initiative, or in response to various prompts from the TCP (e.g., remote close executed, transmission timeout exceeded, destination inaccessible).

ユーザは、自分の主導で、またはＴＣＰからのさまざまなプロンプトに応答して（たとえば、リモートクローズが実行された、送信タイムアウトを超えた、宛先にアクセスできない）、いつでも接続を閉じることができる。

>Because closing a connection requires communication with the foreign TCP, connections may remain in the closing state for a short time.  Attempts to reopen the connection before the TCP replies to the CLOSE command will result in error responses.

接続を閉じるには外部のTCPとの通信が必要なので、接続はしばらくの間閉じた状態のままになります。 TCPがCLOSEコマンドに応答する前に接続を再開しようとすると、エラー応答が返されます。

>Close also implies push function.

CLoseはまたプッシュ機能も意味します。

```
      Status

        Format:  STATUS (local connection name) -> status data
```

>This is an implementation dependent user command and could be excluded without adverse effect.  Information returned would typically come from the TCB associated with the connection.

これは実装依存のユーザコマンドであり、悪影響を与えることなく除外できます。 返される情報は通常、接続に関連したTCBから来ます。

>This command returns a data block containing the following information:

このコマンドは以下を含むデータブロックを返します.

```
          local socket,
          foreign socket,
          local connection name,
          receive window,
          send window,
          connection state,
          number of buffers awaiting acknowledgment,
          number of buffers pending receipt,
          urgent state,
          precedence,
          security/compartment,
          and transmission timeout.
```

>Depending on the state of the connection, or on the implementation itself, some of this information may not be available or meaningful.  If the calling process is not authorized to use this connection, an error is returned.  This prevents unauthorized processes from gaining information about a connection.

接続の状態や実装自体によっては、この情報の一部が利用できない、または意味がない場合があります。 呼び出しプロセスがこの接続を使用することを許可されていない場合は、エラーが返されます。 これにより、許可されていないプロセスが接続に関する情報を取得するのを防ぎます。

```
      Abort

        Format:  ABORT (local connection name)
```

>This command causes all pending SENDs and RECEIVES to be aborted, the TCB to be removed, and a special RESET message to be sent to the TCP on the other side of the connection.

このコマンドによって、保留中のすべてのSENDとRECEIVESが中止され、TCBが削除され、接続の反対側のTCPに特別なRESETメッセージが送信されます。

>Depending on the implementation, users may receive abort indications for each outstanding SEND or RECEIVE, or may simply receive an ABORT-acknowledgment.

実装によっては、ユーザーは未処理のSENDまたはRECEIVEごとに中止指示を受け取るか、単にABORT確認を受け取ることがあります。

#### TCP-to-User Messages

>It is assumed that the operating system environment provides a means for the TCP to asynchronously signal the user program.

オペレーティングシステム環境は、ＴＣＰが非同期にユーザプログラムに信号を送るための手段を提供すると仮定される。

>When the TCP does signal a user program, certain information is passed to the user.  Often in the specification the information will be an error message.

TCPがユーザープログラムに信号を送ると、特定の情報がユーザーに渡されます。 多くの場合、この仕様ではエラーメッセージが表示されます。

>Often in the specification the information will be an error message.  In other cases there will be information relating to the completion of processing a SEND or RECEIVE or other user call.

多くの場合、この仕様ではエラーメッセージが表示されます。 他の場合では、SENDまたはRECEIVEまたは他のユーザー呼び出しの処理の完了に関する情報があります。

>The following information is provided:

```
        Local Connection Name                    Always
        Response String                          Always
        Buffer Address                           Send & Receive
        Byte count (counts bytes received)       Receive
        Push flag                                Receive
        Urgent flag                              Receive
```

#### TCP/Lower-Level Interface

>The TCP calls on a lower level protocol module to actually send and receive information over a network.

ＴＣＰは、ネットワークを介して実際に情報を送受信するために低レベルのプロトコルモジュールを呼び出す。

>One case is that of the ARPA internetwork system where the lower level module is the Internet Protocol (IP) [2].

1つのケースは、下位モジュールがインターネットプロトコル（IP）であるARPAインターネットワークシステムのそれです[2]。

>If the lower level protocol is IP it provides arguments for a type of service and for a time to live.  TCP uses the following settings for these parameters:

下位レベルのプロトコルがIPの場合は、サービスの種類と存続期間に関する引数を提供します。 TCPはこれらのパラメータに次の設定を使用します。

```
      Type of Service = Precedence: routine, Delay: normal, Throughput:
      normal, Reliability: normal; or 00000000.

      Time to Live    = one minute, or 00111100.
```

>Note that the assumed maximum segment lifetime is two minutes. Here we explicitly ask that a segment be destroyed if it cannot be delivered by the internet system within one minute.

想定される最大セグメント寿命は2分です。 ここでは、1分以内にインターネットシステムで配信できない場合、そのセグメントを破棄するように明示的に依頼します。

>If the lower level is IP (or other protocol that provides this feature) and source routing is used, the interface must allow the route information to be communicated.

下位レベルがIP（またはこの機能を提供する他のプロトコル）であり、ソースルーティングが使用されている場合、インターフェイスはルート情報の通信を許可する必要があります。

>This is especially important so that the source and destination addresses used in the TCP checksum be the originating source and ultimate destination. It is also important to preserve the return route to answer connection requests.

TCPチェックサムで使用される送信元および宛先アドレスが発信元および最終宛先になるように、これは特に重要です。 接続要求に答えるためにリターンルートを維持することも重要です。

>Any lower level protocol will have to provide the source address, destination address, and protocol fields, and some way to determine the "TCP length", both to provide the functional equivlent service of IP and to be used in the TCP checksum.

下位レベルのプロトコルでは、送信元アドレス、宛先アドレス、およびプロトコルフィールドを提供し、「TCPの長さ」を決定するための何らかの方法でIPと同等の機能を提供し、TCPチェックサムで使用する必要があります。

#### 3.9.  Event Processing

>he processing depicted in this section is an example of one possible implementation.

このセクションに示されている処理は、1つの可能な実装の例です。

>Other implementations may have slightly different processing sequences, but they should differ from those in this section only in detail, not in substance.

他の実装はわずかに異なる処理シーケンスを持っているかもしれませんが、それらは実質的にではなく、詳細においてのみこのセクションのものと異なるはずです。

>The activity of the TCP can be characterized as responding to events. The events that occur can be cast into three categories:  user calls, arriving segments, and timeouts. 

TCPのアクティビティは、イベントに応答していると見なすことができます。 発生するイベントは、ユーザー呼び出し、到着セグメント、およびタイムアウトという3つのカテゴリーに分類できます。

>This section describes the processing the TCP does in response to each of the events.  In many cases the processing required depends on the state of the connection.

このセクションでは、各イベントに応答してTCPが実行する処理について説明します。 多くの場合、必要な処理は接続の状態によって異なります。

>Events that occur:

```
      User Calls

        OPEN
        SEND
        RECEIVE
        CLOSE
        ABORT
        STATUS

      Arriving Segments

        SEGMENT ARRIVES

      Timeouts

        USER TIMEOUT
        RETRANSMISSION TIMEOUT
        TIME-WAIT TIMEOUT
```

>The model of the TCP/user interface is that user commands receive an immediate return and possibly a delayed response via an event or pseudo interrupt.  In the following descriptions, the term "signal" means cause a delayed response.

TCP /ユーザインタフェースのモデルは、ユーザコマンドがイベントまたは疑似割り込みを介して即時リターンと、場合によっては遅延応答を受け取ることです。 以下の説明において、「信号」という用語は遅延応答を引き起こすことを意味する。

>Error responses are given as character strings.  For example, user commands referencing connections that do not exist receive "error: connection not open".

エラー応答は文字列として与えられます。 たとえば、存在しない接続を参照するユーザーコマンドは「エラー：接続が開いていません」を受け取ります。

>Please note in the following that all arithmetic on sequence numbers, acknowledgment numbers, windows, et cetera, is modulo 2**32 the size of the sequence number space.  

以下では、シーケンス番号、確認応答番号、ウィンドウなどに関するすべての算術演算が、シーケンス番号空間のサイズの2を法とすることに注意してください。

>Also note that "=<" means less than or equal to (modulo 2**32).

また、 "= <"は（modulo 2 ** 32）以下を意味します。

>A natural way to think about processing incoming segments is to imagine that they are first tested for proper sequence number (i.e., that their contents lie in the range of the expected "receive window" in the sequence number space) and then that they are generally queued and processed in sequence number order.

入ってくるセグメントを処理することについて考える自然な方法は、それらが最初に適切なシーケンス番号についてテストされていること（すなわち、それらの内容がシーケンス番号空間の予想される「受信ウィンドウ」の範囲内にある）と想像することです。 シーケンス番号順にキューに入れられ、処理されます。

>When a segment overlaps other already received segments we reconstruct the segment to contain just the new data, and adjust the header fields to be consistent.

あるセグメントが他のすでに受信済みのセグメントと重なっている場合は、新しいデータだけを含むようにセグメントを再構築し、ヘッダーフィールドが一致するように調整します。

>Note that if no state change is mentioned the TCP stays in the same state.

状態の変化が言及されていない場合、TCPは同じ状態に留まります。

```
  OPEN Call

    CLOSED STATE (i.e., TCB does not exist)
```

>Create a new transmission control block (TCB) to hold connection state information.  Fill in local socket identifier, foreign socket, precedence, security/compartment, and user timeout information.

接続状態情報を保持するための新しい伝送制御ブロック（TCB）を作成します。 ローカルソケット識別子、外部ソケット、優先順位、セキュリティ/コンパートメント、およびユーザータイムアウト情報を入力します。

>Note that some parts of the foreign socket may be unspecified in a passive OPEN and are to be filled in by the parameters of the incoming SYN segment.

外部ソケットのいくつかの部分は受動的OPENで指定されていないかもしれず、入ってくるSYNセグメントのパラメータで埋められることになっていることに注意してください。

>Verify the security and precedence requested are allowed for this user, if not return "error:  precedence not allowed" or "error:  security/compartment not allowed."  If passive enter the LISTEN state and return.

「error：優先順位が許可されていません」または「error：セキュリティ/コンパートメントが許可されていません」が返されない場合は、要求されたセキュリティおよび優先順位がこのユーザーに許可されていることを確認します。 受動的ならLISTEN状態に入って戻る。

>If active and the foreign socket is unspecified, return "error: foreign socket unspecified"; if active and the foreign socket is specified, issue a SYN segment.

アクティブで外部ソケットが指定されていない場合、 "error：foreign socket unspecified"を返します。 アクティブで外部ソケットが指定されている場合は、SYNセグメントを発行してください。

>An initial send sequence number (ISS) is selected.

初期送信シーケンス番号（ISS）が選択されています。

>A SYN segment of the form <SEQ=ISS><CTL=SYN> is sent.

<SEQ = ISS> <CTL = SYN>の形式のSYNセグメントが送信されます。

>Set SND.UNA to ISS, SND.NXT to ISS+1, enter SYN-SENT state, and return.

SND.UNAをISSに、SND.NXTをISS + 1に設定し、SYN-SENT状態に入り、そして戻ります。

>If the caller does not have access to the local socket specified, return "error:  connection illegal for this process".  If there is no room to create a new connection, return "error:  insufficient resources".

呼び出し側が指定されたローカルソケットにアクセスできない場合は、「エラー：このプロセスには接続が不正です」を返します。 新しい接続を作成する余地がない場合は、「エラー：リソース不足」を返します。

#### LISTEN STATE

>If active and the foreign socket is specified, then change the connection from passive to active, select an ISS. 

アクティブで外部ソケットが指定されている場合は、接続をパッシブからアクティブに変更して、ISSを選択してください。

>Send a SYN segment, set SND.UNA to ISS, SND.NXT to ISS+1.  Enter SYN-SENT state.  Data associated with SEND may be sent with SYN segment or queued for transmission after entering ESTABLISHED state.

SYNセグメントを送信し、SND.UNAをISSに、SND.NXTをISS + 1に設定します。 SYN-SENT状態に入ります。 SENDに関連したデータは、SYNセグメントとともに送信されるか、またはESTABLISHED状態に入った後に送信のためにキューに入れられることがあります。

>The urgent bit if requested in the command must be sent with the data segments sent as a result of this command.  

コマンドで要求された場合、緊急ビットは、このコマンドの結果として送信されたデータセグメントと一緒に送信する必要があります。

>If there is no room to queue the request, respond with "error:  insufficient resources". If Foreign socket was not specified, then return "error:  foreign socket unspecified".

要求をキューに入れる余地がない場合は、「エラー：リソース不足」で応答してください。 外部ソケットが指定されていない場合は、「エラー：外部ソケットが指定されていません」を返します。

```
    SYN-SENT STATE
    SYN-RECEIVED STATE
    ESTABLISHED STATE
    FIN-WAIT-1 STATE
    FIN-WAIT-2 STATE
    CLOSE-WAIT STATE
    CLOSING STATE
    LAST-ACK STATE
    TIME-WAIT STATE
```

>Return "error:  connection already exists".

「エラー：接続はすでに存在しています」を返します。

```
  SEND Call

    CLOSED STATE (i.e., TCB does not exist)
```

>If the user does not have access to such a connection, then return "error:  connection illegal for this process".

ユーザーがそのような接続にアクセスできない場合は、「エラー：このプロセスには接続が不正です」を返します。

>Otherwise, return "error:  connection does not exist".

そうでなければ、 "error：connection does not exist"を返します.

#### LISTEN STATE

>If the foreign socket is specified, then change the connection from passive to active, select an ISS.

外部ソケットが指定されている場合は、接続をパッシブからアクティブに変更し、ISSを選択してください。

>Send a SYN segment, set SND.UNA to ISS, SND.NXT to ISS+1.  Enter SYN-SENT state.

SYNセグメントを送信し、SND.UNAをISSに、SND.NXTをISS + 1に設定します。 SYN-SENT状態に入ります。

>Data associated with SEND may be sent with SYN segment or queued for transmission after entering ESTABLISHED state.

SENDに関連したデータは、SYNセグメントとともに送信されるか、またはESTABLISHED状態に入った後に送信のためにキューに入れられることがあります。

>The urgent bit if requested in the command must be sent with the data segments sent as a result of this command.

コマンドで要求された場合、緊急ビットは、このコマンドの結果として送信されたデータセグメントと一緒に送信する必要があります。

>If there is no room to queue the request, respond with "error:  insufficient resources".  If Foreign socket was not specified, then return "error:  foreign socket unspecified".

要求をキューに入れる余地がない場合は、「エラー：リソース不足」で応答してください。 外部ソケットが指定されていない場合は、「エラー：外部ソケットが指定されていません」を返します。

```
    SYN-SENT STATE
    SYN-RECEIVED STATE
```

>Queue the data for transmission after entering ESTABLISHED state. If no space to queue, respond with "error:  insufficient resources".

ESTABLISHED状態に入った後、送信用データをキューに入れます。 キューに入れるスペースがない場合は、「エラー：リソース不足」で応答してください。

```
    ESTABLISHED STATE
    CLOSE-WAIT STATE
```

>Segmentize the buffer and send it with a piggybacked acknowledgment (acknowledgment value = RCV.NXT).  If there is insufficient space to remember this buffer, simply return "error: insufficient resources".

バッファをセグメント化して、ピギーバックされた確認応答（確認応答値= RCV.NXT）で送信します。 このバッファを記憶するのに十分なスペースがない場合は、単に「エラー：リソース不足」を返してください。

>If the urgent flag is set, then SND.UP <- SND.NXT-1 and set the urgent pointer in the outgoing segments.

緊急フラグが設定されている場合、SND.UP < -  SND.NXT-1となり、発信セグメントに緊急ポインタが設定されます。

```
    FIN-WAIT-1 STATE
    FIN-WAIT-2 STATE
    CLOSING STATE
    LAST-ACK STATE
    TIME-WAIT STATE
```

>Return "error:  connection closing" and do not service request.

"error：connection closing"を返してサービスリクエストをしないでください。

```
  RECEIVE Call

    CLOSED STATE (i.e., TCB does not exist)
```

>If the user does not have access to such a connection, return "error:  connection illegal for this process".

ユーザーがそのような接続にアクセスできない場合は、「エラー：このプロセスには接続が不正です」を返してください。

>Otherwise return "error:  connection does not exist".

そうでなければ "error：connection does not exist"を返します。

```
    LISTEN STATE
    SYN-SENT STATE
    SYN-RECEIVED STATE
```

>Queue for processing after entering ESTABLISHED state.  If there is no room to queue this request, respond with "error: insufficient resources".

ESTABLISHED状態に入った後の処理のためのキュー。 この要求をキューに入れる余地がない場合は、「エラー：リソース不足」で応答してください。

```
    ESTABLISHED STATE
    FIN-WAIT-1 STATE
    FIN-WAIT-2 STATE
```

>If insufficient incoming segments are queued to satisfy the request, queue the request.  If there is no queue space to remember the RECEIVE, respond with "error:  insufficient resources".

要求を満たすのに不十分な着信セグメントがキューに入れられている場合は、要求をキューに入れます。 RECEIVEを記憶するためのキュースペースがない場合は、「エラー：リソース不足」で応答してください。

>Reassemble queued incoming segments into receive buffer and return to user.  Mark "push seen" (PUSH) if this is the case.

キューに入ってきた入力セグメントを受信バッファーに再アセンブルして、ユーザーに戻ります。 この場合は、「プッシュプッシュ」（PUSH）とマークします。

>If RCV.UP is in advance of the data currently being passed to the user notify the user of the presence of urgent data.

RCV.UPが現在ユーザーに渡されているデータより前の場合は、緊急データの存在をユーザーに通知します。

>When the TCP takes responsibility for delivering data to the user that fact must be communicated to the sender via an acknowledgment.  The formation of such an acknowledgment is described below in the discussion of processing an incoming segment.

TCPがユーザーにデータを配信する責任を負うとき、その事実は確認応答を介して送信者に伝えられなければなりません。 そのような確認応答の形成については、着信セグメントの処理の説明で後述します。

```
    CLOSE-WAIT STATE
```

>Since the remote side has already sent FIN, RECEIVEs must be satisfied by text already on hand, but not yet delivered to the user.  

リモート側はすでにFINを送信しているので、RECEIVEはすでに手元にあるテキストによって満たされる必要がありますが、まだユーザーには配信されません。

>If no text is awaiting delivery, the RECEIVE will get a "error:  connection closing" response.  Otherwise, any remaining text can be used to satisfy the RECEIVE.

配信待ちのテキストがない場合、RECEIVEは "error：connection closing"応答を受け取ります。 それ以外の場合は、残りのテキストを使用してRECEIVEを満たすことができます。

```
    CLOSING STATE
    LAST-ACK STATE
    TIME-WAIT STATE
```

>Return "error:  connection closing".

"error：connection closing"を返します。

```
  CLOSE Call

    CLOSED STATE (i.e., TCB does not exist)
```

>If the user does not have access to such a connection, return "error:  connection illegal for this process".

ユーザーがそのような接続にアクセスできない場合は、「エラー：このプロセスには接続が不正です」を返してください。

>Otherwise, return "error:  connection does not exist".

そうでなければ、 "error：connection does not exist"を返す.

```
    LISTEN STATE
```

>Any outstanding RECEIVEs are returned with "error:  closing" responses.  Delete TCB, enter CLOSED state, and return.

未解決のRECEIVEは "error：closing"応答とともに返されます。 TCBを削除してCLOSED状態に入り、そして戻ります。

```
    SYN-SENT STATE
```

>Delete the TCB and return "error:  closing" responses to any queued SENDs, or RECEIVEs.

TCBを削除し、キューに入れられたSENDまたはRECEIVEに対して "error：closing"応答を返します。

```
    ESTABLISHED STATE
```

>Queue this until all preceding SENDs have been segmentized, then form a FIN segment and send it.  In any case, enter FIN-WAIT-1 state.

先行するすべてのSENDが細分化されるまでこれを待ち行列に入れ、次にFINセグメントを形成してそれを送信します。 いずれにせよ、FIN-WAIT-1状態に入ります。

```
    FIN-WAIT-1 STATE
    FIN-WAIT-2 STATE
```

>Strictly speaking, this is an error and should receive a "error: connection closing" response.  An "ok" response would be acceptable, too, as long as a second FIN is not emitted (the first FIN may be retransmitted though).

厳密に言うと、これはエラーであり、「error：connection closing」という応答を受け取るはずです。 2番目のFINが発行されない限り（最初のFINが再送されるかもしれません）、 "ok"応答も許容されるでしょう。

```
    CLOSE-WAIT STATE
```

>Queue this request until all preceding SENDs have been segmentized; then send a FIN segment, enter CLOSING state.

先行するすべてのSENDがセグメント化されるまで、この要求を待ち行列に入れます。 その後FINセグメントを送信し、CLOSING状態に入ります。

```
    CLOSING STATE
    LAST-ACK STATE
    TIME-WAIT STATE
```

>Respond with "error:  connection closing".

error：connection closing"を返す.

```
  ABORT Call

    CLOSED STATE (i.e., TCB does not exist)
```

>If the user should not have access to such a connection, return "error:  connection illegal for this process".

ユーザーがそのような接続にアクセスできないようにするには、「エラー：このプロセスには接続が不正です」を返します。     
		 
>Otherwise return "error:  connection does not exist".

そうでなければ "error：connection does not exist"を返します。

```
    LISTEN STATE
```

>Any outstanding RECEIVEs should be returned with "error: connection reset" responses.  Delete TCB, enter CLOSED state, and return.

未解決のRECEIVEは "error：connection reset"応答とともに返されるべきです。 TCBを削除してCLOSED状態に入り、そして戻ります。

```
    SYN-SENT STATE
```

>All queued SENDs and RECEIVEs should be given "connection reset" notification, delete the TCB, enter CLOSED state, and return.

すべてのキューに入れられたSENDとRECEIVEは "接続リセット"通知を与えられ、TCBを削除し、CLOSED状態に入り、そして戻るべきです。

```
    SYN-RECEIVED STATE
    ESTABLISHED STATE
    FIN-WAIT-1 STATE
    FIN-WAIT-2 STATE
    CLOSE-WAIT STATE
```

>Send a reset segment:

```
        <SEQ=SND.NXT><CTL=RST>
```

>All queued SENDs and RECEIVEs should be given "connection reset" notification; all segments queued for transmission (except for the RST formed above) or retransmission should be flushed, delete the TCB, enter CLOSED state, and return.

すべてのキューに入れられたSENDとRECEIVEは "接続リセット"通知を与えられるべきです。 送信（上記で形成されたRSTを除く）または再送信のためにキューに入れられたすべてのセグメントはフラッシュされ、TCBを削除し、CLOSED状態に入り、そして戻ります。

```
    CLOSING STATE
    LAST-ACK STATE
    TIME-WAIT STATE
```

>Respond with "ok" and delete the TCB, enter CLOSED state, and return.

"ok"で応答してTCBを削除し、CLOSED状態に入り、そして戻ります。

```
  STATUS Call

    CLOSED STATE (i.e., TCB does not exist)
```

>If the user should not have access to such a connection, return "error:  connection illegal for this process".

ユーザーがそのような接続にアクセスできないようにするには、「エラー：このプロセスには接続が不正です」を返します。

>Otherwise return "error:  connection does not exist".

そうでなければ "error：connection does not exist"を返します。

```
    LISTEN STATE
```

>Return "state = LISTEN", and the TCB pointer.

"state = LISTEN"とTCBポインタを返します。

```
    SYN-SENT STATE
```

>Return "state = SYN-SENT", and the TCB pointer.

"state = SYN-SENT"とTCBポインタを返します。

```
    SYN-RECEIVED STATE
```

>Return "state = SYN-RECEIVED", and the TCB pointer.

"state = SYN-RECEIVED"とTCBポインタを返します。

```
    ESTABLISHED STATE
```

>Return "state = ESTABLISHED", and the TCB pointer.

"state = ESTABLISHED"とTCBポインタを返します。

```
    FIN-WAIT-1 STATE
```

>Return "state = FIN-WAIT-1", and the TCB pointer.

"state = FIN-WAIT-1"とTCBポインタを返します。

```
    FIN-WAIT-2 STATE
```

>Return "state = FIN-WAIT-2", and the TCB pointer.

"state = FIN-WAIT-2"とTCBポインタを返します。

```
    CLOSE-WAIT STATE
```

>Return "state = CLOSE-WAIT", and the TCB pointer.

"state = CLOSE-WAIT"とTCBポインタを返します。

```
    CLOSING STATE
```

>Return "state = CLOSING", and the TCB pointer.

"state = CLOSING"とTCBポインタを返します。

```
    LAST-ACK STATE
```

>Return "state = LAST-ACK", and the TCB pointer.

"state = LAST-ACK"とTCBポインタを返します。

```
    TIME-WAIT STATE
```

>Return "state = TIME-WAIT", and the TCB pointer.

"state = TIME-WAIT"とTCBポインタを返します。

####   SEGMENT ARRIVES

##### If the state is CLOSED (i.e., TCB does not exist) then

>all data in the incoming segment is discarded.  An incoming segment containing a RST is discarded.  An incoming segment not containing a RST causes a RST to be sent in response.

着信セグメント内のすべてのデータは破棄されます。 RSTを含む着信セグメントは破棄されます。 RSTを含まない着信セグメントは、応答としてRSTを送信します。

>The acknowledgment and sequence field values are selected to make the reset sequence acceptable to the TCP that sent the offending segment.

確認応答とシーケンスフィールドの値は、問題のセグメントを送信したTCPがリセットシーケンスを受け入れられるように選択されます。

>If the ACK bit is off, sequence number zero is used,

ACKビットがオフの場合、シーケンス番号0は

```
				<SEQ=0><ACK=SEG.SEQ+SEG.LEN><CTL=RST,ACK>
```

>If the ACK bit is on,

ACKがオンの場合, 

```
        <SEQ=SEG.ACK><CTL=RST>
```

>Return.


>If the state is LISTEN then

もしLISTEN状態ならば, 

>first check for an RST

はじめにRSTをチェックし

>An incoming RST should be ignored.  Return.

受信RSTは無視され, 返される.

>second check for an ACK

2番目にACKをチェックし,

>Any acknowledgment is bad if it arrives on a connection still in the LISTEN state.  An acceptable reset segment should be formed for any arriving ACK-bearing segment.  The RST should be formatted as follows:

LISTEN状態のままの接続に到着した場合、確認応答は正しくありません。 受け入れ可能なリセットセグメントは、到着するすべてのACKを含むセグメントに対して形成されるべきです。 RSTは次のようにフォーマットされるべきです：

```
				<SEQ=SEG.ACK><CTL=RST>
```

>Return.


>third check for a SYN

3番目にSYNをチェックし,

>If the SYN bit is set, check the security.  If the security/compartment on the incoming segment does not exactly match the security/compartment in the TCB then send a reset and return.

SYNビットが設定されている場合は、セキュリティを確認してください。 入ってくるセグメントのセキュリティ/コンパートメントがTCBのセキュリティ/コンパートメントと完全に一致しないならば、リセットを送って戻る。

```
          <SEQ=SEG.ACK><CTL=RST>
```

>If the SEG.PRC is greater than the TCB.PRC then if allowed by the user and the system set TCB.PRC<-SEG.PRC, if not allowed send a reset and return.

もしSEG.PRCがTCB.PRCより大きければ、もしユーザとシステムが許可していればTCB.PRC <-SEG.PRCをセットし、許可されていなければリセットを送って戻る。

```
          <SEQ=SEG.ACK><CTL=RST>
```

>If the SEG.PRC is less than the TCB.PRC then continue.

ＳＥＧ．ＰＲＣがＴＣＢ．ＰＲＣより小さければ、次に続ける。

>Set RCV.NXT to SEG.SEQ+1, IRS is set to SEG.SEQ and any other control or text should be queued for processing later.  ISS should be selected and a SYN segment sent of the form:

RCV.NXTをSEG.SEQ + 1に設定し、IRSをSEG.SEQに設定し、その他の制御またはテキストは後で処理するためにキューに入れる必要があります。 ISSを選択し、SYNセグメントを次の形式で送信する必要があります。

```
          <SEQ=ISS><ACK=RCV.NXT><CTL=SYN,ACK>
```

>SND.NXT is set to ISS+1 and SND.UNA to ISS.  The connection state should be changed to SYN-RECEIVED.  Note that any other incoming control or data (combined with SYN) will be processed in the SYN-RECEIVED state, but processing of SYN and ACK should not be repeated.  If the listen was not fully specified (i.e., the foreign socket was not fully specified), then the unspecified fields should be filled in now.

SND.NXTはISS + 1に設定され、SND.UNAはISSに設定されます。 接続状態はSYN-RECEIVEDに変更する必要があります。 他の着信制御またはデータ（SYNと組み合わせて）はSYN-RECEIVED状態で処理されますが、SYNおよびACKの処理は繰り返されるべきではありません。 待機が完全に指定されていなかった（すなわち、外部ソケットが完全に指定されていなかった）場合は、未指定フィールドに今すぐ記入する必要があります。

>fourth other text or control

その他

>Any other control or text-bearing segment (not containing SYN) must have an ACK and thus would be discarded by the ACK processing.  An incoming RST segment could not be valid, since it could not have been sent in response to anything sent by this incarnation of the connection.  So you are unlikely to get here, but if you do, drop the segment, and return.

（SYNを含まない）他のコントロールまたはテキストを含むセグメントはACKを持っている必要があるため、ACK処理によって破棄されます。 それが接続のこの肉体化によって送られた何でもに応じて送られなかったかもしれないので入って来るRSTセグメントは有効であることができませんでした。 ですから、ここに来ることはまずありませんが、行った場合はそのセグメントをドロップしてから戻ってください。

>If the state is SYN-SENT then

SYN-SENT状態の場合,

>first check the ACK bit

はじめにACKをチェックし

>If the ACK bit is set

セットされていた場合

>If SEG.ACK =< ISS, or SEG.ACK > SND.NXT, send a reset (unless the RST bit is set, if so drop the segment and return)

SEG.ACK = <ISS、またはSEG.ACK> SND.NXTの場合、リセットを送信します（RSTビットが設定されている場合を除き、セグメントをドロップして戻ります）。

```
            <SEQ=SEG.ACK><CTL=RST>
```

>and discard the segment.  Return.

セグメントを破棄し 戻る

>If SND.UNA =< SEG.ACK =< SND.NXT then the ACK is acceptable.

SND.UNA =< SEG.ACK =< SND.NXTの場合、ACKは受け入れ可能です。

>second check the RST bit

2番目にRSTbitをチェックして

>If the RST bit is set

セットされている場合

>If the ACK was acceptable then signal the user "error: connection reset", drop the segment, enter CLOSED state, delete TCB, and return.  Otherwise (no ACK) drop the segment and return.

ACKが受け入れられたならば、それからユーザに「エラー：接続リセット」を合図し、セグメントを落とし、CLOSED状態に入り、TCBを削除し、そして戻る。 そうでなければ（ＡＣＫなし）、セグメントを落として戻る。

>third check the security and precedence

3番目にsecurityとprecedenceをチェックし

>If the security/compartment in the segment does not exactly match the security/compartment in the TCB, send a reset

セグメントの証券/コンパートメントがTCBの証券/コンパートメントと完全に一致しない場合は、リセットを送信する.

>If there is an ACK

ACKがあれば

```
            <SEQ=SEG.ACK><CTL=RST>
```

>Otherwise

そうでなければ

```
            <SEQ=0><ACK=SEG.SEQ+SEG.LEN><CTL=RST,ACK>
```

>fourth check the SYN bit

4番目にSYNbitをチェックし

>This step should be reached only if the ACK is ok, or there is no ACK, and it the segment did not contain a RST.

このステップに到達する必要があるのは、ACKが正常であるか、またはACKがなく、セグメントにRSTが含まれていなかった場合です。

>If the SYN bit is on and the security/compartment and precedence are acceptable then, RCV.NXT is set to SEG.SEQ+1, IRS is set to SEG.SEQ.  SND.UNA should be advanced to equal SEG.ACK (if there is an ACK), and any segments on the retransmission queue which are thereby acknowledged should be removed.

SYNビットがオンで、セキュリティー/コンパートメントおよび優先順位が受け入れ可能である場合、RCV.NXTはSEG.SEQ + 1に設定され、IRSはSEG.SEQに設定されます。 （ＡＣＫがある場合）ＳＮＤ．ＵＮＡはＳＥＧ．ＡＣＫに等しくなるように進められるべきであり、それによって確認応答される再送信キュー上の任意のセグメントは除去されるべきである。

>If SND.UNA > ISS (our SYN has been ACKed), change the connection state to ESTABLISHED, form an ACK segment

AND.UAN> ISS（SYNが確認されている）の場合は、接続状態をESTABLISHEDに変更し、ACKセグメントを形成します。

```
          <SEQ=SND.NXT><ACK=RCV.NXT><CTL=ACK>
```

>and send it.  Data or controls which were queued for transmission may be included.  If there are other controls or text in the segment then continue processing at the sixth step below where the URG bit is checked, otherwise return.

そしてそれを送る。 送信のためにキューに入れられたデータまたは制御が含まれてもよい。 セグメント内に他のコントロールまたはテキストがある場合は、URGビットがチェックされる以下の6番目のステップから処理を続行し、それ以外の場合は戻ります。

>Otherwise enter SYN-RECEIVED, form a SYN,ACK segment

そうでなければSYN-RECEIVEDを入力し、SYN、ACKセグメントを形成する

```
          <SEQ=ISS><ACK=RCV.NXT><CTL=SYN,ACK>
```

>and send it.  If there are other controls or text in the segment, queue them for processing after the ESTABLISHED state has been reached, return.

そしてそれを送る。 セグメント内に他のコントロールまたはテキストがある場合は、ESTABLISHED状態に達した後でそれらを処理のためにキューに入れ、戻ります。

>fifth, if neither of the SYN or RST bits is set then drop the segment and return.

5番目に、SYNビットもRSTビットも設定されていない場合は、セグメントをドロップして戻ります。

>Otherwise, first check sequence number

そうでなければ、最初にシーケンス番号をチェックする。

```
      SYN-RECEIVED STATE
      ESTABLISHED STATE
      FIN-WAIT-1 STATE
      FIN-WAIT-2 STATE
      CLOSE-WAIT STATE
      CLOSING STATE
      LAST-ACK STATE
      TIME-WAIT STATE
```

>Segments are processed in sequence.  Initial tests on arrival are used to discard old duplicates, but further processing is done in SEG.SEQ order.  If a segment's contents straddle the boundary between old and new, only the new parts should be processed.

セグメントは順番に処理されます。 到着時の初期テストは古い重複を破棄するために使用されますが、それ以降の処理はSEG.SEQの順序で行われます。 セグメントの内容が新旧の境界をまたいでいる場合は、新しい部分だけを処理する必要があります。

>There are four cases for the acceptability test for an incoming segment:

着信セグメントの受け入れ可能性テストには4つのケースがあります。

```
        Segment Receive  Test
        Length  Window
        ------- -------  -------------------------------------------

           0       0     SEG.SEQ = RCV.NXT

           0      >0     RCV.NXT =< SEG.SEQ < RCV.NXT+RCV.WND

          >0       0     not acceptable

          >0      >0     RCV.NXT =< SEG.SEQ < RCV.NXT+RCV.WND
                      or RCV.NXT =< SEG.SEQ+SEG.LEN-1 < RCV.NXT+RCV.WND
```

>If the RCV.WND is zero, no segments will be acceptable, but special allowance should be made to accept valid ACKs, URGs and RSTs.

RCV.WNDが0の場合、どのセグメントも受け入れられませんが、有効なACK、URG、およびRSTを受け入れるために特別な許可が必要です。

>If an incoming segment is not acceptable, an acknowledgment should be sent in reply (unless the RST bit is set, if so drop the segment and return):

入ってくるセグメントが受け入れられないなら、（RSTビットが設定されていないなら、セグメントを落として戻ってくる）応答で応答を送るべきです

```
          <SEQ=SND.NXT><ACK=RCV.NXT><CTL=ACK>
```

>After sending the acknowledgment, drop the unacceptable segment and return.

確認応答を送信した後、不適切なセグメントを削除して戻ります。

>In the following it is assumed that the segment is the idealized segment that begins at RCV.NXT and does not exceed the window. One could tailor actual segments to fit this assumption by trimming off any portions that lie outside the window (including SYN and FIN), and only processing further if the segment then begins at RCV.NXT.  Segments with higher begining sequence numbers may be held for later processing.

以下では、セグメントはRCV.NXTで始まり、ウィンドウを超えない理想化されたセグメントであると想定されています。 ウィンドウの外側にある部分（SYNとFINを含む）を切り捨てて、セグメントがRCV.NXTから始まる場合にのみさらに処理することによって、この仮定に合うように実際のセグメントを調整することができます。 より高い開始シーケンス番号を持つセグメントは後の処理のために保持されるかもしれません。

>second check the RST bit,

2番目にRTSbitをチェックし

```
      SYN-RECEIVED STATE
```

>If the RST bit is set

RSTbitがセットされていたら

>If this connection was initiated with a passive OPEN (i.e., came from the LISTEN state), then return this connection to LISTEN state and return.  The user need not be informed.  If this connection was initiated with an active OPEN (i.e., came from SYN-SENT state) then the connection was refused, signal the user "connection refused".  In either case, all segments on the retransmission queue should be removed.  And in the active OPEN case, enter the CLOSED state and delete the TCB, and return.

この接続が受動的ＯＰＥＮで開始された（すなわち、ＬＩＳＴＥＮ状態から来た）場合、この接続をＬＩＳＴＥＮ状態に戻して戻る。 ユーザーに通知する必要はありません。 この接続がアクティブＯＰＥＮで開始された（すなわち、ＳＹＮ − ＳＥＮＴ状態から来た）場合、接続は拒否され、ユーザに「接続拒否」と合図する。 どちらの場合でも、再送信キューのすべてのセグメントを削除する必要があります。 アクティブOPENの場合は、CLOSED状態に入り、TCBを削除して戻ります。

```
      ESTABLISHED
      FIN-WAIT-1
      FIN-WAIT-2
      CLOSE-WAIT
```

>If the RST bit is set then, any outstanding RECEIVEs and SEND should receive "reset" responses.  All segment queues should be flushed.  Users should also receive an unsolicited general "connection reset" signal.  Enter the CLOSED state, delete the TCB, and return.

RSTビットが設定されているなら、どんな未解決のRECEIVEsとSENDも "リセット"応答を受けるべきです。 すべてのセグメントキューをフラッシュする必要があります。 ユーザはまた、求められていない一般的な「接続リセット」信号を受け取るべきです。 CLOSED状態に入り、TCBを削除して返す。

```
      CLOSING STATE
      LAST-ACK STATE
      TIME-WAIT
```

>If the RST bit is set then, enter the CLOSED state, delete the TCB, and return.

RSTビットが設定されている場合は、CLOSED状態に入り、TCBを削除して戻ります。

>third check security and precedence

3番目にsecurityとprecedenceをチェックし

```
      SYN-RECEIVED
```

>If the security/compartment and precedence in the segment do not exactly match the security/compartment and precedence in the TCB then send a reset, and return.

セグメント内のセキュリティ/コンパートメントと優先順位がTCBのセキュリティ/コンパートメントと優先順位と完全に一致しない場合は、リセットを送信して戻ります。

```
      ESTABLISHED STATE
```

>If the security/compartment and precedence in the segment do not exactly match the security/compartment and precedence in the TCB then send a reset, any outstanding RECEIVEs and SEND should receive "reset" responses.  All segment queues should be flushed.  Users should also receive an unsolicited general "connection reset" signal.  Enter the CLOSED state, delete the TCB, and return.

セグメント内のセキュリティ/コンパートメントおよび優先順位がTCB内のセキュリティ/コンパートメントおよび優先順位と完全に一致しない場合、リセットを送信すると、未解決のRECEIVEおよびSENDが「リセット」応答を受信するはずです。 すべてのセグメントキューをフラッシュする必要があります。 ユーザはまた、求められていない一般的な「接続リセット」信号を受け取るべきです。 CLOSED状態に入り、TCBを削除してから戻ってください。

>Note this check is placed following the sequence check to prevent a segment from an old connection between these ports with a different security or precedence from causing an abort of the current connection.

セキュリティまたは優先順位が異なるこれらのポート間の古い接続からのセグメントが現在の接続を中断させないようにするために、このチェックはシーケンスチェックの後に配置されます。

>fourth, check the SYN bit,

4番目にSYNbitをチェックし

```
      SYN-RECEIVED
      ESTABLISHED STATE
      FIN-WAIT STATE-1
      FIN-WAIT STATE-2
      CLOSE-WAIT STATE
      CLOSING STATE
      LAST-ACK STATE
      TIME-WAIT STATE
```

>If the SYN is in the window it is an error, send a reset, any outstanding RECEIVEs and SEND should receive "reset" responses, all segment queues should be flushed, the user should also receive an unsolicited general "connection reset" signal, enter the CLOSED state, delete the TCB, and return.

ＳＹＮがウィンドウ内にある場合、それはエラーであり、リセットを送り、未解決のＲＥＣＥＩＶＥおよびＳＥＮＤは「リセット」応答を受け取るべきであり、すべてのセグメント待ち行列はフラッシュされるべきであり、ユーザは求められない一般的な「接続リセット」信号も受け取るべきである。 CLOSED状態、TCBを削除して戻ります。

>If the SYN is not in the window this step would not be reached and an ack would have been sent in the first step (sequence number check).

SYNがウィンドウ内にない場合、このステップには到達せず、最初のステップでackが送信されたはずです（シーケンス番号チェック）。

>fifth check the ACK field,

5番目にSCKをチェックし

>if the ACK bit is off drop the segment and return

ACKビットがオフの場合、そのセグメントをドロップして戻ります。

>if the ACK bit is on

ACKbitがオンなら

```
        SYN-RECEIVED STATE
```

>If SND.UNA =< SEG.ACK =< SND.NXT then enter ESTABLISHED state and continue processing.

SND.UNA =< SEG.ACK =< SND.NXTの場合、ESTABLISHED状態に入り、処理を続行します。

>If the segment acknowledgment is not acceptable, form a reset segment,

セグメントの確認応答が受け入れられない場合は、リセットセグメントを作成してください。

```
              <SEQ=SEG.ACK><CTL=RST>
```

>and send it.

送信する

```
        ESTABLISHED STATE
```

>If SND.UNA < SEG.ACK =< SND.NXT then, set SND.UNA <- SEG.ACK. Any segments on the retransmission queue which are thereby entirely acknowledged are removed.  Users should receive positive acknowledgments for buffers which have been SENT and fully acknowledged (i.e., SEND buffer should be returned with "ok" response).  If the ACK is a duplicate (SEG.ACK < SND.UNA), it can be ignored.  If the ACK acks something not yet sent (SEG.ACK > SND.NXT) then send an ACK, drop the segment, and return.

ＳＮＤ．ＵＮＡ ＜ＳＥＧ．ＡＣＫ ＝ ＜ＳＮＤ．ＮＸＴであれば、ＳＮＤ．ＵＮＡ ＜ -  ＳＥＧ．ＡＣＫを設定する。 それによって完全に確認応答される再送信キュー上のいかなるセグメントも除去される。 ユーザは、送信済みで完全に承認されたバッファについて肯定的な承認を受け取るべきである（すなわち、「送信」バッファは「OK」応答で返されるべきである）。 ACKが重複している場合（SEG.ACK < SND.UNA）、無視することができます。 ACKがまだ送信されていないものを確認した場合（SEG.ACK > SND.N XT）、ACKを送信し、セグメントをドロップして戻ります。

>If SND.UNA < SEG.ACK =< SND.NXT, the send window should be updated.  If (SND.WL1 < SEG.SEQ or (SND.WL1 = SEG.SEQ and SND.WL2 =< SEG.ACK)), set SND.WND <- SEG.WND, set SND.WL1 <- SEG.SEQ, and set SND.WL2 <- SEG.ACK.

SND.UNA < SEG.ACK =< SND.NXTの場合は、送信ウィンドウを更新する必要があります。 （SND.WL1 < SEG.SEQまたは（SND.WL1 = SEG.SEQかつSND.WL2 =< SEG.ACK））の場合、SND.WND <-  SEG.WNDを設定し、SND.WL1 <-  SEG.SEQを設定してください。 SND.WL2 <-  SEG.ACKを設定します。

>Note that SND.WND is an offset from SND.UNA, that SND.WL1 records the sequence number of the last segment used to update SND.WND, and that SND.WL2 records the acknowledgment number of the last segment used to update SND.WND.  The check here prevents using old segments to update the window.

SND.WNDはSND.UNAからのオフセットであり、SND.WL1はSND.WNDの更新に使用された最後のセグメントのシーケンス番号を記録し、SND.WL2はSND.WNDの更新に使用された最後のセグメントの確認応答番号を記録します。  ここのチェックはウィンドウを更新するために古いセグメントを使用することを防ぎます。

```
        FIN-WAIT-1 STATE
```

>In addition to the processing for the ESTABLISHED state, if our FIN is now acknowledged then enter FIN-WAIT-2 and continue processing in that state.

ESTABLISHED状態の処理に加えて、現在のFINが確認された場合は、FIN-WAIT-2を入力してその状態で処理を続行します。

```
        FIN-WAIT-2 STATE
```

>In addition to the processing for the ESTABLISHED state, if the retransmission queue is empty, the user's CLOSE can be acknowledged ("ok") but do not delete the TCB.

ESTABLISHED状態の処理に加えて、再送信キューが空の場合、ユーザーのCLOSEは確認応答されます（「ok」）が、TCBは削除されません。

```
        CLOSE-WAIT STATE
```

>Do the same processing as for the ESTABLISHED state.

ESTABLISHED状態と同じ処理を行います。

```
        CLOSING STATE
```

>In addition to the processing for the ESTABLISHED state, if the ACK acknowledges our FIN then enter the TIME-WAIT state, otherwise ignore the segment.

ＥＳＴＡＢＬＩＳＨＥＤ状態に対する処理に加えて、ＡＣＫが我々のＦＩＮを確認するならばＴＩＭＥ − ＷＡＩＴ状態に入り、そうでなければセグメントを無視する。

```
        LAST-ACK STATE
```

>The only thing that can arrive in this state is an acknowledgment of our FIN.  If our FIN is now acknowledged, delete the TCB, enter the CLOSED state, and return.

この状態にたどり着くことができる唯一のものは私たちのFINの承認です。 FINが承認されたら、TCBを削除してCLOSED状態に入り、そして戻ります。

```
        TIME-WAIT STATE
```

>The only thing that can arrive in this state is a retransmission of the remote FIN.  Acknowledge it, and restart the 2 MSL timeout.

この状態にたどり着くことができる唯一のものは、リモートFINの再送信です。 それを確認して、2 MSLタイムアウトを再開してください。

>sixth, check the URG bit,

6番目にURGbitをチェックして

```
      ESTABLISHED STATE
      FIN-WAIT-1 STATE
      FIN-WAIT-2 STATE
```

>If the URG bit is set, RCV.UP <- max(RCV.UP,SEG.UP), and signal the user that the remote side has urgent data if the urgent pointer (RCV.UP) is in advance of the data consumed.  If the user has already been signaled (or is still in the "urgent mode") for this continuous sequence of urgent data, do not signal the user again.

URGビットが設定されている場合、RCV.UP < -  max（RCV.UP、SEG.UP）であり、緊急ポインタ（RCV.UP）が消費されたデータより早い場合、リモート側に緊急データがあることをユーザーに知らせます。 。 ユーザーがこの緊急データの連続シーケンスについてすでに通知されている（またはまだ「緊急モード」になっている）場合は、再度ユーザーに通知しないでください。

```
      CLOSE-WAIT STATE
      CLOSING STATE
      LAST-ACK STATE
      TIME-WAIT
```

>This should not occur, since a FIN has been received from the remote side.  Ignore the URG.

リモート側からFINが受信されたため、これは発生しません。 URGを無視してください。

>seventh, process the segment text,

7番目に、セグメントテキストを処理します。

```
      ESTABLISHED STATE
      FIN-WAIT-1 STATE
      FIN-WAIT-2 STATE
```

>Once in the ESTABLISHED state, it is possible to deliver segment text to user RECEIVE buffers.  Text from segments can be moved into buffers until either the buffer is full or the segment is empty.  If the segment empties and carries an PUSH flag, then the user is informed, when the buffer is returned, that a PUSH has been received.

ESTABLISHED状態になると、セグメントテキストをユーザーのRECEIVEバッファーに配信することが可能になります。 セグメントからのテキストは、バッファがいっぱいになるかセグメントが空になるまでバッファに移動できます。 セグメントがプッシュフラグを空にして伝送する場合、バッファが返されると、プッシュが受信されたことがユーザに通知されます。

>When the TCP takes responsibility for delivering the data to the user it must also acknowledge the receipt of the data.

TCPがユーザーにデータを配信する責任を負うとき、TCPはデータの受信を確認応答する必要があります。

>Once the TCP takes responsibility for the data it advances RCV.NXT over the data accepted, and adjusts RCV.WND as apporopriate to the current buffer availability.  The total of RCV.NXT and RCV.WND should not be reduced.

TCPがデータに責任を持つと、受け入れたデータよりもRCV.NXTを進め、現在のバッファの可用性に応じてRCV.WNDを調整します。 RCV.NXTとRCV.WNDの合計を減らすべきではありません。

>Please note the window management suggestions in section 3.7.

セクション3.7のウィンドウ管理の提案に注意してください。

>Send an acknowledgment of the form:

フォームの確認を送信します。

```
          <SEQ=SND.NXT><ACK=RCV.NXT><CTL=ACK>
```

>This acknowledgment should be piggybacked on a segment being transmitted if possible without incurring undue delay.

この確認応答は、可能であれば、過度の遅延を招くことなく送信されているセグメントに便乗する必要があります。

```
      CLOSE-WAIT STATE
      CLOSING STATE
      LAST-ACK STATE
      TIME-WAIT STATE
```

>This should not occur, since a FIN has been received from the remote side.  Ignore the segment text.

リモート側からFINが受信されたため、これは発生しません。 セグメントテキストを無視します。

>eighth, check the FIN bit,

8番目にFINbitをチェックし

>Do not process the FIN if the state is CLOSED, LISTEN or SYN-SENT since the SEG.SEQ cannot be validated; drop the segment and return.

SEG.SEQは検証できないため、状態がCLOSED、LISTEN、またはSYN-SENTの場合は、FINを処理しません。 セグメントを削除して戻ります。

>If the FIN bit is set, signal the user "connection closing" and return any pending RECEIVEs with same message, advance RCV.NXT over the FIN, and send an acknowledgment for the FIN.  Note that FIN implies PUSH for any segment text not yet delivered to the user.

FINビットが設定されているなら、ユーザに "接続終了"を知らせて、同じメッセージでどんな未定のRECEIVEも返してください、そして、FINの上にRCV.NXTを進めてください、そして、FINのために承認を送ってください。 FINは、まだユーザーに配信されていないセグメントテキストに対してPUSHを意味することに注意してください。

```
        SYN-RECEIVED STATE
        ESTABLISHED STATE
```

>Enter the CLOSE-WAIT state.

CLOSE-WAIT状態に入る

```
        FIN-WAIT-1 STATE
```

>If our FIN has been ACKed (perhaps in this segment), then enter TIME-WAIT, start the time-wait timer, turn off the other timers; otherwise enter the CLOSING state.

私たちのFINが（おそらくこのセグメントで）ACKされているならば、それからTIME-WAITを入力し、時間待ちタイマーをスタートさせ、他のタイマーをオフにします。 それ以外の場合はCLOSING状態に入ります。

```
        FIN-WAIT-2 STATE
```

>Enter the TIME-WAIT state.  Start the time-wait timer, turn off the other timers.

TIME-WAIT状態に入ります。 時間待ちタイマーを始動し、他のタイマーをオフにします。

```
        CLOSE-WAIT STATE
```

>Remain in the CLOSE-WAIT state.

CLOSE-WAIT状態のままになります。

```
        CLOSING STATE
```

>Remain in the CLOSING state.

CLOSING状態のままになります。

```
        LAST-ACK STATE
```

>Remain in the LAST-ACK state.

LAST-ACK状態のままになります。

```
        TIME-WAIT STATE
```

>Remain in the TIME-WAIT state.  Restart the 2 MSL time-wait timeout.

TIME-WAIT状態のままにします。 2 MSLの待ち時間タイムアウトを再開します。

>and return.

return

```
  USER TIMEOUT
```

>For any state if the user timeout expires, flush all queues, signal the user "error:  connection aborted due to user timeout" in general and for any outstanding calls, delete the TCB, enter the CLOSED state and return.

ユーザタイムアウトが期限切れになった場合はどの状態でも、すべてのキューをフラッシュし、一般に「エラー：ユーザタイムアウトが原因で接続が中断されました」というシグナルを出します。そして未解決の呼び出しに対してはTCBを削除し、CLOSED状態に入って戻ります。

```
  RETRANSMISSION TIMEOUT
```

>For any state if the retransmission timeout expires on a segment in the retransmission queue, send the segment at the front of the retransmission queue again, reinitialize the retransmission timer, and return.

再送信タイムアウトが再送信キュー内のセグメントで期限切れになった場合は、どの状態でも、そのセグメントを再送信キューの先頭に再送信し、再送信タイマーを再初期化してから戻ります。

```
  TIME-WAIT TIMEOUT
```

>If the time-wait timeout expires on a connection delete the TCB, enter the CLOSED state and return.

接続のタイムアウト待ち時間が経過してTCBを削除した場合は、CLOSED状態に入り、戻ります。






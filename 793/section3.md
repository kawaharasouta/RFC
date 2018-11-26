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

略

>Options: variable
Options may occupy space at the end of the TCP header and are a multiple of 8 bits in length. All options are included in the checksum. An option may begin on any octet boundary. 
There are two cases for the format of an option:
Case 1: A single octet of option-kind.
Case 2: An octet of option-kind, an octet of option-length, and the actual option-data octets.

略

>Note that the list of options may be shorter than the data offset field might imply. 

略

>The content of the header beyond the End-of-Option option must be header padding (i.e., zero).

略

>A TCP must implement all options.

略

>Currently defined options include (kind indicated in octal):

略

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

略

**No-Operation**

```

+--------+
|00000001|
+--------+
 Kind=1

```

>This option code may be used between options, for example, to align the beginning of a subsequent option on a word boundary. There is no guarantee that senders will use this option, so receivers must be prepared to process options even if they
do not begin on a word boundary.

略

**Maximum Segment Size**

```

+--------+--------+---------+--------+
|00000010|00000100|   max seg size   |
+--------+--------+---------+--------+
 Kind=2   Length=4

```

>Maximum Segment Size Option Data: 16 bits
If this option is present, then it communicates the maximum receive segment size at the TCP which sends this segment. This field must only be sent in the initial connection request (i.e., in segments with the SYN control bit set). If this option is not used, any segment size is allowed.

略

>Padding: variable
The TCP header padding is used to ensure that the TCP header ends and data begins on a 32 bit boundary. The padding is composed of zeros.

略

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
SEG.UP - segment urgent pointer
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

TCPコネクションはあるステートからイベントに反して他の状態へ進行する.

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







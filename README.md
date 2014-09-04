# Using it

The basic usage just extracts a thread dump from a given log file.
If there are multiple thread dumps in the given file, thread-grep
automatically finds the most recent one.

    $ thread-grep /path/to/log.file
    "clojure-agent-send-off-pool-31" prio=10 tid=0x00007f8f2c06e000 nid=0x233f runnable [0x00007f8f8f6e9000] (STATE: RUNNABLE)
      at java.net.SocketInputStream.socketRead0(Native Method)
      at java.net.SocketInputStream.read(SocketInputStream.java:150)
      at java.net.SocketInputStream.read(SocketInputStream.java:121)
      at java.io.BufferedInputStream.fill(BufferedInputStream.java:235)
      at java.io.BufferedInputStream.read(BufferedInputStream.java:254)
      - locked <0x00000000e810dd40> (a java.io.BufferedInputStream)
      at java.io.FilterInputStream.read(FilterInputStream.java:83)
      at java.io.PushbackInputStream.read(PushbackInputStream.java:139)
      at clojure.tools.nrepl.bencode$read_byte.invokePrim(bencode.clj:84)
      at clojure.tools.nrepl.bencode$read_token.invoke(bencode.clj:236)
      at clojure.tools.nrepl.bencode$read_bencode.invoke(bencode.clj:254)
      at clojure.tools.nrepl.transport$bencode$fn__231$fn__232.invoke(transport.clj:95)
      at clojure.tools.nrepl.transport$bencode$fn__231.invoke(transport.clj:95)
      at clojure.tools.nrepl.transport$fn_transport$fn__193.invoke(transport.clj:42)
      at clojure.core$binding_conveyor_fn$fn__4145.invoke(core.clj:1910)
      at clojure.lang.AFn.call(AFn.java:18)
      at java.util.concurrent.FutureTask$Sync.innerRun(FutureTask.java:334)
      at java.util.concurrent.FutureTask.run(FutureTask.java:166)
      at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1110)
      at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:603)
      at java.lang.Thread.run(Thread.java:722)

      [... lots more stuff ....]
  

Use the `-l` flag to limit the output per thread to a fixed number of lines:

    $ thread-grep /path/to/log.file -l 3
    "clojure-agent-send-off-pool-31" prio=10 tid=0x00007f8f2c06e000 nid=0x233f runnable [0x00007f8f8f6e9000] (STATE: RUNNABLE)
      at java.net.SocketInputStream.socketRead0(Native Method)
      at java.net.SocketInputStream.read(SocketInputStream.java:150)
      at java.net.SocketInputStream.read(SocketInputStream.java:121)

    "clojure-agent-send-off-pool-11" prio=10 tid=0x00007f8f18001800 nid=0x5951 waiting on condition [0x00007f8f95af1000] (STATE: TIMED_WAITING)
      at sun.misc.Unsafe.park(Native Method)
      - parking to wait for  <0x00000000e810dbc0> (a java.util.concurrent.SynchronousQueue$TransferStack)
      at java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:226)

    [... lots more stuff ...]


And the `-m` flag to limit the output to threads whose
name/state/stack match a regular expression:

    $ thread-grep /path/to/log.file -l 3 -m send-off
    "clojure-agent-send-off-pool-31" prio=10 tid=0x00007f8f2c06e000 nid=0x233f runnable [0x00007f8f8f6e9000] (STATE: RUNNABLE)
      at java.net.SocketInputStream.socketRead0(Native Method)
      at java.net.SocketInputStream.read(SocketInputStream.java:150)
      at java.net.SocketInputStream.read(SocketInputStream.java:121)

    "clojure-agent-send-off-pool-11" prio=10 tid=0x00007f8f18001800 nid=0x5951 waiting on condition [0x00007f8f95af1000] (STATE: TIMED_WAITING)
      at sun.misc.Unsafe.park(Native Method)
      - parking to wait for  <0x00000000e810dbc0> (a java.util.concurrent.SynchronousQueue$TransferStack)
      at java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:226)

    "clojure-agent-send-off-pool-3" prio=10 tid=0x00007f8f2c061000 nid=0x7721 runnable [0x00007f8f8faed000] (STATE: RUNNABLE)
      at java.net.PlainSocketImpl.socketAccept(Native Method)
      at java.net.AbstractPlainSocketImpl.accept(AbstractPlainSocketImpl.java:398)
      at java.net.ServerSocket.implAccept(ServerSocket.java:522)

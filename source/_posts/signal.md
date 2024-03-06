---
title: 系统编程——信号处理
data: 2024-03-06
tags: Linux
---

## 关于信号

   信号是事件发生时对进程的通知机制。有时也称之为软件中断。

   一个(具有合适权限的)进程能够向另一进程发送信号。信号的这一用法可作为一种同
   步技术,甚至是进程间通信(IPC)的原始形式。进程也可以向自身发送信号。然而,发往进
   程的诸多信号,通常都是源于内核。引发内核为进程产生信号的各类事件如下。

   - 硬件发生异常,即硬件检测到一个错误条件并通知内核,随即再由内核发送相应信号给相关进程。硬件异常的例子包括执行一条异常的机器语言指令,诸如,被 0 除,或者引用了无法访问的内存区域。

   - 用户键入了能够产生信号的终端特殊字符。其中包括中断字符(通常是 Control-C)、暂停字符(通常是 Control-Z)。

   - 发生了软件事件。例如,针对文件描述符的输出变为有效,调整了终端窗口大小,定时器到期,进程执行的 CPU 时间超限,或者该进程的某个子进程退出。


   针对每个信号,都定义了一个唯一的(小)整数,从 1 开始顺序展开。<font color=Orange><signal.h></font> 以 SIGxxxx 形式的符号名对这些整数做了定义。由于每个信号的实际编号随系统不同而不同,所以在程序中总是使用这些符号名。

   Linux 对标准信号的编号为 1~31。然而,Linux 于 signal(7)手册页中列出的信号名称却超出了 31 个。名称超出的原因有多种。有些名称只是其他名称的同义词,之所以定义是为了与其他 UNIX 实现保持源码兼容。其他名称虽然有定义,但却并未使用。以下列表介绍了各种信号。

   <table>
    <thead>
     <tr>
      <th>名称</th>
      <th>信号值</th>
      <th>描述</th>
      <th><code>SUSv3</code></th>
      <th>默认</th>
     </tr>
    </thead>
    <tbody>
     <tr>
      <th><code>SIGHUP</code></th>
      <th>1</th>
      <th>挂起</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGINT</code></th>
      <th>2</th>
      <th>终端中断</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGQUIT</code></th>
      <th>3</th>
      <th>终端退出</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGILL</code></th>
      <th>4</th>
      <th>非法指令</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGTRAP</code></th>
      <th>5</th>
      <th>跟踪/断点陷阱</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGABRT</code></th>
      <th>6</th>
      <th>中止进程</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGBUS</code></th>
      <th>7（SAMP=10）</th>
      <th>内存访问错误</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGFPE</code></th>
      <th>8</th>
      <th>算术异常</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGKILL</code></th>
      <th>9</th>
      <th>必杀（确保杀死）</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGUSR1</code></th>
      <th>10 (SA=30, MP=16)</th>
      <th>自定义用户信号1</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGSEGV</code></th>
      <th>11</th>
      <th>无效的内存引用</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGUSR2</code></th>
      <th>12</th>
      <th>自定义用户信号2</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGPIPE</code></th>
      <th>13</th>
      <th>管道断开</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGALRM</code></th>
      <th>14</th>
      <th>实时定时器过期</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGTERM</code></th>
      <th>15</th>
      <th>终止进程</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGSTKFLT</code></th>
      <th>16 (SAM=undef, P=36)</th>
      <th>协处理器栈错误</th>
      <th><code> </code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGCHLD</code></th>
      <th>17 (SA=20, MP=18)</th>
      <th>终止或者停止子进程</th>
      <th><code>*</code></th>
      <th><code>ignore</code></th>
     </tr>
     <tr>
      <th><code>SIGCONT</code></th>
      <th>18 (SA=19, M=25, P=26)</th>
      <th>若停止则继续</th>
      <th><code>*</code></th>
      <th><code>cont</code></th>
     </tr>
     <tr>
      <th><code>SIGSTOP</code></th>
      <th>19 (SA=17, M=23, P=24)</th>
      <th>确保停止</th>
      <th><code>*</code></th>
      <th><code>stop</code></th>
     </tr>
     <tr>
      <th><code>SIGTSTP</code></th>
      <th>20 (SA=18, M=24, P=25)</th>
      <th>终端停止</th>
      <th><code>*</code></th>
      <th><code>stop</code></th>
     </tr>
     <tr>
      <th><code>SIGTTIN</code></th>
      <th>21 (M=26, P=27)</th>
      <th>BG 从终端读取</th>
      <th><code>*</code></th>
      <th><code>stop</code></th>
     </tr>
     <tr>
      <th><code>SIGTTOU</code></th>
      <th>22 (M=27, P=28)</th>
      <th>BG 向终端写</th>
      <th><code>*</code></th>
      <th><code>stop</code></th>
     </tr>
     <tr>
      <th><code>SIGURG</code></th>
      <th>23 (SA=16, M=21, P=29)</th>
      <th>套接字上的紧急数据</th>
      <th><code>*</code></th>
      <th><code>ignore</code></th>
     </tr>
     <tr>
      <th><code>SIGXCPU</code></th>
      <th>24 (M=30, P=33)</th>
      <th>突破对 CPU 时间的限制</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGXFSZ</code></th>
      <th>25 (M=31, P=34)</th>
      <th>突破对文件大小的限制</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGVTALRM</code></th>
      <th>26 (M=28, P=20)</th>
      <th>虚拟定时器过期</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGPROF</code></th>
      <th>27 (M=29, P=21)</th>
      <th>性能分析定时器过期</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGWINCH</code></th>
      <th>28 (M=20, P=23)</th>
      <th>终端窗口尺寸发生变化</th>
      <th><code> </code></th>
      <th><code>ignore</code></th>
     </tr>
     <tr>
      <th><code>SIGIO /</code></th>
      <th>29 (SA=23, MP=22)</th>
      <th>I/O 时可能产生</th>
      <th><code>*</code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGPWR</code></th>
      <th>30 (SA=29, MP=19)</th>
      <th>电量行将耗尽</th>
      <th><code> </code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGSYS</code></th>
      <th>31 (SAMP=12)</th>
      <th>无效的系统调用</th>
      <th><code>*</code></th>
      <th><code>core</code></th>
     </tr>
     <tr>
      <th><code>SIGEMT</code></th>
      <th>undef (SAMP=7)</th>
      <th>硬件错误</th>
      <th><code> </code></th>
      <th><code>term</code></th>
     </tr>
     <tr>
      <th><code>SIGPOLL</code></th>
      <th> </th>
      <th> </th>
      <th><code> </code></th>
      <th><code> </code></th>
     </tr>
      <p>*该表格来自于《LINUX/UNIX系统编程》</p>
    </tbody>
    
   </table>

   - 在 Linux 2.2 中,信号 SIGXCPU、SIGXFSZ、SIGSYS 和 SIGBUS 的默认行为是终止进程,但不会产生核心转储文件。自内核 2.4 以后,Linux 实现满足了 SUSv3 的要求,这些信号不但会引发进程终止,也将生成核心转储文件。在其他几个 UNIX 实现中,对信号 SIGXCPU 和 SIGXFSZ 的处理方式与 Linux 2.2 相同。
  
   - 在其他的 UNIX 实现中,对 SIGPWR 信号的默认行为通常是将其忽略。
    
   - 几个 UNIX 实现(特别是 BSD 衍生系统)默认情况下将忽略 SIGIO 信号。
    
   - 虽然 SIGEMT 信号尚未获得任何标准的接纳,但却得到大多数 UNIX 实现的支持。然而,在其他实现中,该信号通常会导致进程终止并产生核心转储文件。
  
   - SUSv1 将 SIGURG 信号的默认行为定义为终止进程,这也是一些较老 UNIX 实现的默认做法。而 SUSv2 则采用了现行规范(将其忽略)。
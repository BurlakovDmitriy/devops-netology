# Домашнее задание к занятию "3.3. Операционные системы, лекция 1"

1. Какой системный вызов делает команда `cd`? В прошлом ДЗ мы выяснили, что `cd` не является самостоятельной программой, это `shell builtin`, поэтому запустить `strace` непосредственно на `cd` не получится. Тем не менее, вы можете запустить `strace` на `/bin/bash -c 'cd /tmp'`. В этом случае вы увидите полный список системных вызовов, которые делает сам `bash` при старте. Вам нужно найти тот единственный, который относится именно к `cd`. Обратите внимание, что `strace` выдаёт результат своей работы в поток stderr, а не в stdout.
    - chdir("/tmp")

3. Попробуйте использовать команду `file` на объекты разных типов на файловой системе. Например:
    ```bash
    vagrant@netology1:~$ file /dev/tty
    /dev/tty: character special (5/0)
    vagrant@netology1:~$ file /dev/sda
    /dev/sda: block special (8/0)
    vagrant@netology1:~$ file /bin/bash
    /bin/bash: ELF 64-bit LSB shared object, x86-64
    ```
    Используя `strace` выясните, где находится база данных `file` на основании которой она делает свои догадки.
   - База данных находится в файле /usr/lib/file/magic.mgc
        
1. Предположим, приложение пишет лог в текстовый файл. Этот файл оказался удален (deleted в lsof), однако возможности сигналом сказать приложению переоткрыть файлы или просто перезапустить приложение – нет. Так как приложение продолжает писать в удаленный файл, место на диске постепенно заканчивается. Основываясь на знаниях о перенаправлении потоков предложите способ обнуления открытого удаленного файла (чтобы освободить место на файловой системе).
     ```
   top | tee -a log.txt
    vagrant@vagrant:~$ ps aux | grep tee
    vagrant     1340  0.0  0.0   5480   676 pts/0    T    18:16   0:00 tee -a log.txt
    rm log.txt
    lsof -p 1340
    tee     1340 vagrant    3w   REG  253,0    72779  786443 /home/vagrant/log.txt (deleted)
    cat /dev/null > /proc/1340/fd/3
    lsof -p 1340
    tee     1340 vagrant    3w   REG  253,0        0  786443 /home/vagrant/log.txt (deleted)


3. Занимают ли зомби-процессы какие-то ресурсы в ОС (CPU, RAM, IO)?
    - Процессы зомби вообще не являются реальными процессами. Это просто записи в таблице процессов ядра. Это единственный ресурс, который они потребляют. Они не потребляют ни CPU, ни RAM. Единственная опасность наличия зомби - это нехватка места в таблице процессов
    При достижении лимита записей все процессы пользователя, от имени которого выполняется создающий зомби родительский процесс, не будут способны создавать новые дочерние процессы. Кроме этого, пользователь, от имени которого выполняется родительский процесс, не сможет зайти на консоль (локальную или удалённую) или выполнить какие-либо команды на уже открытой консоли (потому что для этого командный 
    интерпретатор sh должен создать новый процесс), и для восстановления работоспособности (завершения виновной программы) будет необходимо вмешательство.
    
5. В iovisor BCC есть утилита `opensnoop`:
    ```bash
    root@vagrant:~# dpkg -L bpfcc-tools | grep sbin/opensnoop
    /usr/sbin/opensnoop-bpfcc
    ```
    На какие файлы вы увидели вызовы группы `open` за первую секунду работы утилиты? Воспользуйтесь пакетом `bpfcc-tools` для Ubuntu 20.04. Дополнительные [сведения по установке](https://github.com/iovisor/bcc/blob/master/INSTALL.md).
     
     ```vagrant@vagrant:~sudo opensnoop-bpfcc
    PID    COMM               FD ERR PATH 
   601    irqbalance          6   0 /proc/interrupts
   601    irqbalance          6   0 /proc/stat
   601    irqbalance          6   0 /proc/irq/20/smp_affinity
   601    irqbalance          6   0 /proc/irq/0/smp_affinity
   601    irqbalance          6   0 /proc/irq/1/smp_affinity
   601    irqbalance          6   0 /proc/irq/8/smp_affinity
   601    irqbalance          6   0 /proc/irq/12/smp_affinity
   601    irqbalance          6   0 /proc/irq/14/smp_affinity
   601    irqbalance          6   0 /proc/irq/15/smp_affinity
   787    vminfo              4   0 /var/run/utmp
   596    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
   596    dbus-daemon        18   0 /usr/share/dbus-1/system-services
   596    dbus-daemon        -1   2 /lib/dbus-1/system-services
   596    dbus-daemon        18   0 /var/lib/snapd/dbus-1/system-services/
   787    vminfo              4   0 /var/run/utmp
   596    dbus-daemon        -1   2 /usr/local/share/dbus-1/system-services
   596    dbus-daemon        18   0 /usr/share/dbus-1/system-services
   596    dbus-daemon        -1   2 /lib/dbus-1/system-services
    
1. Какой системный вызов использует `uname -a`? Приведите цитату из man по этому системному вызову, где описывается альтернативное местоположение в `/proc`, где можно узнать версию ядра и релиз ОС.
    -  `uname -a` использует системный вызов sethostname(2)
     ```
   Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version,
       domainname}.
 

3. Чем отличается последовательность команд через `;` и через `&&` в bash? Например:
    ```bash
    root@netology1:~# test -d /tmp/some_dir; echo Hi
    Hi
    root@netology1:~# test -d /tmp/some_dir && echo Hi
    root@netology1:~#
    ```
    Есть ли смысл использовать в bash `&&`, если применить `set -e`?
    - Оператор точка с запятой позволяет запускать несколько команд за один раз, и выполнение команды происходит последовательно. «&&» используется для объединения команд таким образом, что следующая команда запускается тогда и только тогда, когда предыдущая команда завершилась без ошибок (или, точнее, выйдет с кодом возврата 0).  && Удобна для использования в однострочных командах.
    - `set -e`указывает оболочке выйти, если команда дает ненулевой статус выхода. Проще говоря, оболочка завершает работу при сбое команды. Директива `set -e` не работает при работе с конвейерными командами. 
1. Из каких опций состоит режим bash `set -euxo pipefail` и почему его хорошо было бы использовать в сценариях?
    - `set -euxo pipefail` Это приведет к немедленному сбросу (завершению) скрипта bash, если в нём возникнет ошибка. -e Немедленное завершение если команда возвращает значение отличное от 0., -x печатает команда и их аргументы по мере их выполенния, -o позволяет использовать специальные опции, pipefail - заставляет возвращать ноль если всё выполнено успешно, если же нет - возвращаемое значение является статус последней команды, которая завершилась с ненулевым статусом. Дугими словами это статус последней команды для выхода с ненулевым статусом, или ноль, если ни одна команда не завершилась с ненулевым статусом
Использование -euxo pipefail в сценариях позволит не  выходить из сценария, если, допустим команда1 потерпит неудачу. Если команда1 терпит неудачу, я хочу перейти к последующим командам: команда2 и т.д , прежде чем выходить из скрипта с помощью exit code 1. 
Т.е  возможность продолжить исполнение скрипта в случае неудачного исполнения предыдущих команд.
    
3. Используя `-o stat` для `ps`, определите, какой наиболее часто встречающийся статус у процессов в системе. В `man ps` ознакомьтесь (`/PROCESS STATE CODES`) что значат дополнительные к основной заглавной буквы статуса процессов. Его можно не учитывать при расчете (считать S, Ss или Ssl равнозначными).


PROCESS STATE CODES
       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display
       to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               I    Idle kernel thread
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped by job control signal
               t    stopped by debugger during the tracing
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent

       For BSD formats and when the stat keyword is used, additional characters may be displayed:
Дополнительные к основной заглавной буквы статуса процессов значат 

               <    high-priority (not nice to other users)
               N    low-priority (nice to other users)
               L    has pages locked into memory (for real-time and custom IO)
               s    is a session leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
               +    is in the foreground process group

  ```
  root         531  0.0  1.8 281012 18776 ?        SLsl 17:31   0:00 /sbin/multipathd -d -s
  root         543  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop0]
root         548  0.0  0.0      0     0 ?        S    17:31   0:00 [jbd2/sda2-8]
root         549  0.0  0.0      0     0 ?        I<   17:31   0:00 [ext4-rsv-conver]
root         550  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop1]
root         551  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop2]
root         553  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop3]
root         554  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop4]
root         555  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop5]
root         556  0.0  0.0      0     0 ?        S<   17:31   0:00 [loop6]
systemd+     613  0.0  0.7  27828  7848 ?        Ss   17:31   0:00 /lib/systemd/systemd-networkd
systemd+     615  0.0  1.2  24916 12448 ?        Ss   17:31   0:00 /lib/systemd/systemd-resolved
root         630  0.0  0.7 236456  7288 ?        Ssl  17:31   0:00 /usr/lib/accountsservice/accounts-daemon
message+     631  0.0  0.4   8468  4848 ?        Ss   17:31   0:00 /usr/bin/dbus-daemon --system --address=systemd: --no
root         637  0.0  0.3  82724  3952 ?        Ssl  17:31   0:00 /usr/sbin/irqbalance --foreground
root         638  0.0  1.7  29748 17932 ?        Ss   17:31   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-s
syslog       641  0.0  0.5 221128  5080 ?        Ssl  17:31   0:00 /usr/sbin/rsyslogd -n -iNONE
root         644  0.0  2.8 776636 28300 ?        Ssl  17:31   0:01 /usr/lib/snapd/snapd
root         646  0.0  0.8  17896  8064 ?        Ss   17:31   0:00 /lib/systemd/systemd-logind
root         656  0.0  0.2   6800  2964 ?        Ss   17:31   0:00 /usr/sbin/cron -f
daemon       665  0.0  0.2   3776  2144 ?        Ss   17:31   0:00 /usr/sbin/atd -f
root         675  0.0  0.1   5824  1712 tty1     Ss+  17:31   0:00 /sbin/agetty -o -p -- \u --noclear tty1 linux
root         681  0.0  0.7  13084  7372 ?        Ss   17:31   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startu
root         701  0.0  0.6 233596  7000 ?        Ssl  17:31   0:00 /usr/libexec/polkitd --no-debug
root         850  0.0  0.2 293676  2920 ?        Sl   17:31   0:01 /usr/sbin/VBoxService --pidfile /var/run/vboxadd-serv

```

Наиболее часто встречается  - I    Idle kernel thread, ожидание, или неактивный поток ядра
Или -S(Ss, Ssl, Ss+)      interruptible sleep (waiting for an event to complete) прерывистый сон (ожидание завершения события)




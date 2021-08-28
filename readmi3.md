# Домашнее задание к занятию "3.1. Работа в терминале, лекция 1"

1. Установите средство виртуализации [Oracle VirtualBox](https://www.virtualbox.org/).
* установил

1. Установите средство автоматизации [Hashicorp Vagrant](https://www.vagrantup.com/).
* установил
1. В вашем основном окружении подготовьте удобный для дальнейшей работы терминал. Можно предложить:
* установил Windows Terminal в Windows
* почитал - менять не стал.

1. С помощью базового файла конфигурации запустите Ubuntu 20.04 в VirtualBox посредством Vagrant:
* запустил, создал ВМ, подключился по SSH 

1. Ознакомьтесь с графическим интерфейсом VirtualBox, посмотрите как выглядит виртуальная машина, которую создал для вас Vagrant, какие аппаратные ресурсы ей выделены. Какие ресурсы выделены по-умолчанию? 
* выделены по умолчанию 1024 mb RAM, 2 ядра (ЦП), HDD 2 ГБ (расширяющийся до 64), 4 mb видеопамяти


1. Ознакомьтесь с возможностями конфигурации VirtualBox через Vagrantfile: [документация](https://www.vagrantup.com/docs/providers/virtualbox/configuration.html). Как добавить оперативной памяти или ресурсов процессора виртуальной машине?
* Добавление ресурсов оперативной памяти и количества ЦП
```config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end```
Ограничение загрузки ЦП до 50%
```config.vm.provider "virtualbox" do |v|
  v.customize ["modifyvm", "--cpuexecutioncap", "50"]
end```



1. Команда `vagrant ssh` из директории, в которой содержится Vagrantfile, позволит вам оказаться внутри виртуальной машины без каких-либо дополнительных настроек. Попрактикуйтесь в выполнении обсуждаемых команд в терминале Ubuntu.
* подключился

1. Ознакомиться с разделами `man bash`, почитать о настройках самого bash:
    * какой переменной можно задать длину журнала `history`, и на какой строчке manual это описывается?
*   ```862        HISTSIZE
    863               The number of commands to remember in the command history (see HISTORY below).  If the value is 0, com‐
    864               mands  are  not saved in the history list.  Numeric values less than zero result in every command being
    865               saved on the history list (there is no limit).  The shell sets the default value to 500  after  reading
    866               any startup files.```
    
    * что делает директива `ignoreboth` в bash?
* Значение ignorebothявляется сокращением для `ignorespace` и `ignoredups` Список значений, разделенных двоеточиями, управляющий тем, как команды сохраняются в списке истории. Если список значений включает в себя ignorespace, строки, начинающиеся с пробела, не сохраняются в списке истории. Значение ignoredupsприводит к тому, что строки, соответствующие предыдущей записи истории, не будут сохранены.
* ``` 837               of ignoreboth is shorthand for ignorespace and ignoredups.  A value of erasedups  causes  all  previous
    838               lines  matching  the  current  line to be removed from the history list before that line is saved.  Any
    839               value not in the above list is ignored.  If HISTCONTROL is unset, or does not include  a  valid  value,
    840               all  lines  read by the shell parser are saved on the history list, subject to the value of HISTIGNORE.
    841               The second and subsequent lines of a multi-line compound command are not tested, and are added  to  the
    842               history regardless of the value of HISTCONTROL.```

1. В каких сценариях использования применимы скобки `{}` и на какой строчке `man bash` это описано?
1. С учётом ответа на предыдущий вопрос, как создать однократным вызовом `touch` 100000 файлов? Получится ли аналогичным образом создать 300000? Если нет, то почему?
1. В man bash поищите по `/\[\[`. Что делает конструкция `[[ -d /tmp ]]`
1. Основываясь на знаниях о просмотре текущих (например, PATH) и установке новых переменных; командах, которые мы рассматривали, добейтесь в выводе type -a bash в виртуальной машине наличия первым пунктом в списке:

	```bash
	bash is /tmp/new_path_directory/bash
	bash is /usr/local/bin/bash
	bash is /bin/bash
	```

	(прочие строки могут отличаться содержимым и порядком)
    В качестве ответа приведите команды, которые позволили вам добиться указанного вывода или соответствующие скриншоты.

1. Чем отличается планирование команд с помощью `batch` и `at`?

1. Завершите работу виртуальной машины чтобы не расходовать ресурсы компьютера и/или батарею ноутбука.

 
 ---

## Как сдавать задания

Обязательными к выполнению являются задачи без указания звездочки. Их выполнение необходимо для получения зачета и диплома о профессиональной переподготовке.

Задачи со звездочкой (*) являются дополнительными задачами и/или задачами повышенной сложности. Они не являются обязательными к выполнению, но помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в github репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Также вы можете выполнить задание в [Google Docs](https://docs.google.com/document/u/0/?tgif=d) и отправить в личном кабинете на проверку ссылку на ваш документ.
Название файла Google Docs должно содержать номер лекции и фамилию студента. Пример названия: "1.1. Введение в DevOps — Сусанна Алиева".

Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Перед тем как выслать ссылку, убедитесь, что ее содержимое не является приватным (открыто на комментирование всем, у кого есть ссылка), иначе преподаватель не сможет проверить работу. Чтобы это проверить, откройте ссылку в браузере в режиме инкогнито.

[Как предоставить доступ к файлам и папкам на Google Диске](https://support.google.com/docs/answer/2494822?hl=ru&co=GENIE.Platform%3DDesktop)

[Как запустить chrome в режиме инкогнито ](https://support.google.com/chrome/answer/95464?co=GENIE.Platform%3DDesktop&hl=ru)

[Как запустить  Safari в режиме инкогнито ](https://support.apple.com/ru-ru/guide/safari/ibrw1069/mac)

Любые вопросы по решению задач задавайте в чате Slack.

---

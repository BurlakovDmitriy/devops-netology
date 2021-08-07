# Домашнее задание к занятию «2.4. Инструменты Git»


1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.

`aefead2207ef7e2aa5dc81a34aedf0cad4c32545`

3. Какому тегу соответствует коммит `85024d3`?

Коммит `85024d3100126de36331c6982bfaac02cdab9e76`
Таг `"tag: v0.12.23"`

5. Сколько родителей у коммита `b8d720`? Напишите их хеши.

`b8d720` Является мерж комитом и по идее у него два родителя 
Комнадой `git show HEAD^` находясь на этом комите получаем первого 
`56cd7859e05c36c06b56d013b55a252d0bb7e158`
Но вот командой `git show HEAD~2` я получаю уже не второго родителя, а родителя родителя.
судя по графу второй родитель комита `b8d720` является комит из смерженой ветки `9ea88f22fc6269854151c571162c5bcf958bee2b`

6. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами  v0.12.23 и v0.12.24.

```* 33ff1c03b 2020-03-19 | v0.12.24 (HEAD, tag: v0.12.24) [tf-release-bot]
* b14b74c49 2020-03-10 | [Website] vmc provider links [Chris Griggs]
* 3f235065b 2020-03-19 | Update CHANGELOG.md [Alisdair McDiarmid]
* 6ae64e247 2020-03-19 | registry: Fix panic when server is unreachable [Alisdair McDiarmid]
* 5c619ca1b 2020-03-18 | website: Remove links to the getting started guide's old location [Nick Fagerlund]
* 06275647e 2020-03-18 | Update CHANGELOG.md [Alisdair McDiarmid]
* d5f9411f5 2020-03-17 | command: Fix bug when using terraform login on Windows [Alisdair McDiarmid]
* 4b6d06cc5 2020-03-10 | Update CHANGELOG.md [Pam Selle]
* dd01a3507 2020-03-05 | Update CHANGELOG.md [Kristin Laemmert]
* 225466bc3 2020-03-05 | Cleanup after v0.12.23 release [tf-release-bot]
* 85024d310 2020-03-05 | v0.12.23 (tag: v0.12.23) [tf-release-bot]
```


7. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит так `func providerSource(...)` (вместо троеточего перечислены аргументы).

Перепробовал множество способов поиска функций и изменений, но итог по сути один и он не находит конкретно необходимую функцию 

```$ git grep -e func --and -e providerSource
command/init_test.go:func TestInit_providerSource(t *testing.T) {
```
или вот пытался найти комит в котором была создана функция через `blame`, но такой директории и файла нет....
```$ git grep -n providerSource
command/init_test.go:950:func TestInit_providerSource(t *testing.T) {
plugin/discovery/get.go:156:    providerSource := allVersions.ID
plugin/discovery/get.go:207:    downloadURLs, err := i.listProviderDownloadURLs(providerSource, versionMeta.Version)
plugin/discovery/get.go:235:    printedProviderName := fmt.Sprintf("%q (%s)", provider.LegacyString(), providerSource)
Solid@DESKTOP-ARVFF6C MINGW64 ~/github/terraform (main)
$ git blame -L 156,235 get.go
fatal: no such path 'get.go' in HEAD

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/terraform (main)
$ cd plugin/discovery
bash: cd: plugin/discovery: No such file or directory

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/terraform (main)
$ ls -l
total 260
-rw-r--r-- 1 Solid 197121  15020 Aug  8 00:51 BUGPROCESS.md
-rw-r--r-- 1 Solid 197121   1669 Aug  8 00:51 CHANGELOG.md
-rw-r--r-- 1 Solid 197121   1631 Aug  8 00:51 CODEOWNERS
-rw-r--r-- 1 Solid 197121    828 Aug  8 00:51 Dockerfile
-rw-r--r-- 1 Solid 197121  16331 Aug  7 19:04 LICENSE
-rw-r--r-- 1 Solid 197121   1898 Aug  8 00:51 Makefile
-rw-r--r-- 1 Solid 197121   3771 Aug  8 00:51 README.md
-rw-r--r-- 1 Solid 197121   2140 Aug  8 00:51 checkpoint.go
-rw-r--r-- 1 Solid 197121    620 Aug  8 00:51 codecov.yml
-rw-r--r-- 1 Solid 197121  10908 Aug  8 00:51 commands.go
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 docs/
-rw-r--r-- 1 Solid 197121   6005 Aug  8 00:51 go.mod
-rw-r--r-- 1 Solid 197121 109310 Aug  8 00:51 go.sum
-rw-r--r-- 1 Solid 197121   2654 Aug  8 00:51 help.go
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 internal/
-rw-r--r-- 1 Solid 197121  18537 Aug  8 00:51 main.go
-rw-r--r-- 1 Solid 197121   6742 Aug  8 00:51 main_test.go
-rw-r--r-- 1 Solid 197121    985 Aug  8 00:51 plugins.go
-rw-r--r-- 1 Solid 197121  10034 Aug  8 00:51 provider_source.go
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 scripts/
-rw-r--r-- 1 Solid 197121    170 Aug  7 19:04 signal_unix.go
-rw-r--r-- 1 Solid 197121    138 Aug  7 19:04 signal_windows.go
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 tools/
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 version/
-rw-r--r-- 1 Solid 197121    151 Aug  8 00:51 version.go
drwxr-xr-x 1 Solid 197121      0 Aug  8 00:51 website/
```



1. Найдите все коммиты в которых была изменена функция `globalPluginDirs`.
```$ git log -S "globalPluginDirs" --oneline
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package
```

1. Кто автор функции `synchronizedWriters`? 
```$ git grep -n synchronizedWriters
main.go:284:            wrapped := synchronizedWriters(stdout, stderr)
synchronized_writers.go:13:// synchronizedWriters takes a set of writers and returns wrappers that ensure
synchronized_writers.go:15:func synchronizedWriters(targets ...io.Writer) []io.Writer {

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/terraform ((v0.12.24))
$ git blame -L 15,17 synchronized_writers.go
5ac311e2a9 (Martin Atkins 2017-05-03 16:25:41 -0700 15) func synchronizedWriters(targets ...io.Writer) []io.Writer {
```
Martin Atkins

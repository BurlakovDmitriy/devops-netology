# Домашнее задание к занятию «2.4. Инструменты Git»


1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.
`aefead2207ef7e2aa5dc81a34aedf0cad4c32545`

3. Какому тегу соответствует коммит `85024d3`?
Коммит `85024d3100126de36331c6982bfaac02cdab9e76`
Таг `"tag: v0.12.23"`

5. Сколько родителей у коммита `b8d720`? Напишите их хеши.
`b8d720` Является мерж комитом и по идее у него два родителя 
Комнадой `git show HEAD^` находясь на этом комите получаем первого 
56cd7859e05c36c06b56d013b55a252d0bb7e158
Но вот командой `git show HEAD~2` я получею уже не второго родителя а родителя родителя.
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

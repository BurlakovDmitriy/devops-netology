# Домашнее задание к занятию «2.3. Ветвления в Git»
Данное задание получилось первым, что я переделывал дважды.
Так на первый раз совершив несколько ошибок попытался откатить версию путем 
`reset head~`  и попытался заново всё сделать. Приходилось и удалять ветки локально и из репозитория, освоить базовые вещи в VIM, удалять комиты, неоднократно пролистывать историю изменений. В итоге подобрать alias для работы с историей, после все отойти на комит прошлого домашнего задания и все снести. 
Ну, а после снова просмотреть материал по теме и в итоге заново всё сделать.

В итоге мне немного не понятно история изменений которую я вижу что в `GitHub` что `PyCharm`, но надеюсь всё сделано верно
Ниже представлен кусок кода начиная с rebase ветки git-rebase
```Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (main)
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (main)
$ git switch git-rebase
Switched to branch 'git-rebase'
Your branch is up to date with 'origin/git-rebase'.

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase)
$ git rebase -i main
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/Solid/github/devops-netology/.git/rebase-merge/git-rebase-todo to DOS format...
dos2unix: converting file C:/Users/Solid/github/devops-netology/.git/rebase-merge/git-rebase-todo to Unix format...
error: could not apply deebfa9... git-rebase 1
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply deebfa9... git-rebase 1
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 1/2)
$ vim rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 1/2)
$ git add rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 1/2)
$ git rebase --continue
error: could not apply a2fe923... git-rebase 2
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply a2fe923... git-rebase 2
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 2/2)
$ vim rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 2/2)
$ git add rebase.sh

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase|REBASE 2/2)
$ git rebase --continue
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/Solid/github/devops-netology/.git/COMMIT_EDITMSG to DOS format...
dos2unix: converting file C:/Users/Solid/github/devops-netology/.git/COMMIT_EDITMSG to Unix format...
[detached HEAD ecb942e] Merge branch 'git-merge'
 Date: Thu Jul 29 23:21:31 2021 +0300
Successfully rebased and updated refs/heads/git-rebase.

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase)
$ git push -u origin git-rebase
To https://github.com/BurlakovDmitriy/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'https://github.com/BurlakovDmitriy/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase)
$ git push -u origin git-rebase -f
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 425 bytes | 425.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/BurlakovDmitriy/devops-netology.git
 + a2fe923...ecb942e git-rebase -> git-rebase (forced update)
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (git-rebase)
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (main)
$ git merge git-rebase
hint: Waiting for your editor to close the file... unix2dos: converting file C:/Users/Solid/github/devops-netology/.git/MERGE_MSG to DOS format...
dos2unix: converting file C:/Users/Solid/github/devops-netology/.git/MERGE_MSG to Unix format...
Merge made by the 'recursive' strategy.
 branching/rebase.sh | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

Solid@DESKTOP-ARVFF6C MINGW64 ~/github/devops-netology/branching (main)

$ git push -u origin main
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 229 bytes | 229.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/BurlakovDmitriy/devops-netology.git
   3d94d82..c57ccc9  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'
```
А здесь представлен лог. 

```$ git hist
*   c57ccc9 2021-07-29 | Merge branch 'git-rebase' (HEAD -> main, origin/main, origin/HEAD) [BurlakovDmitry]
|\
| *   ecb942e 2021-07-29 | Merge branch 'git-merge' (origin/git-rebase, git-rebase) [BurlakovDmitry]
| |\
* | \   3d94d82 2021-07-29 | Merge branch 'git-merge' [BurlakovDmitry]
|\ \ \
| |/ /
|/| /
| |/
| * b8a7707 2021-07-29 | merge: use shift (origin/git-merge, git-merge) [BurlakovDmitry]
| * 4553792 2021-07-29 | merge: @ instead * [BurlakovDmitry]
* | fc65f5b 2021-07-29 | change rabase [BurlakovDmitry]
|/
* 4a283b1 2021-07-29 | prepare for merge and rebase [BurlakovDmitry]
* 11dbd16 2021-07-18 | Update README.md (tag: v0.1, tag: v0.0) [BurlakovDmitriy]
* f9ff4e7 2021-07-18 | Moved and deleted [BurlakovDmitry]
* edea029 2021-07-18 | readmi changed [BurlakovDmitry]
* f750ad4 2021-07-18 | Prepare to delete and move [BurlakovDmitry]
* f657301 2021-07-18 | Prepare to delete and move [BurlakovDmitry]
* 67ef05f 2021-07-18 | Added gitignore [BurlakovDmitry]
* 7f8bb09 2021-07-18 | First commit [BurlakovDmitry]
* f8a2f24 2021-07-18 | Initial commit [BurlakovDmitriy]
```



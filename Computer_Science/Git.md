---
Duration: Invalid date
Interview graded: true
Last edited time: 2023-07-16T20:26
Needs Rework: false
Status: Not started
Topic:
  - "[SCM](SCM)"
---
# **Системы контроля версий**

## **Подходы**

**Git Flow**

Легко управлять каждой фазой разработки, лучше использовать, когда есть концепция выпуска ПО(нет непрерывного развертывания)

GitFlow у вас есть дополнительная ветвь develop куда сливаются все разрабатываемые в текущий момент ветви. develop необходимо "стабилизировать" перед релизом, что часто приводит либо к переносу релиза, либо "релизу с замечаниями".

Основные ветки:

- master
- develop
- features
- hotfix
- release

[![](https://lh4.googleusercontent.com/wgK3QRrCvTa-qoTMMM-MzlXP7zwkYQqGdAZvlimnV9BLfBkdMpGM54VHy8RdHS_9PSHuzQN7IUJbg7zy8NzrnEiqLqAW0Kp8Vl6KuV9o4wnBM5Nl9vrTKRU4TJWmUz_ETJYnC6NZnnmjCDiOPVc61RNtPx7VzO7b-60Ts2KKVlpArFvJUf1ExHvuY8y_)](https://lh4.googleusercontent.com/wgK3QRrCvTa-qoTMMM-MzlXP7zwkYQqGdAZvlimnV9BLfBkdMpGM54VHy8RdHS_9PSHuzQN7IUJbg7zy8NzrnEiqLqAW0Kp8Vl6KuV9o4wnBM5Nl9vrTKRU4TJWmUz_ETJYnC6NZnnmjCDiOPVc61RNtPx7VzO7b-60Ts2KKVlpArFvJUf1ExHvuY8y_)

**Github Flow**

Удобнее для среды постоянного развертывания, где нет понятия выпуск. Каждый раз при реализации новой функции они выпускают ее в продакшн. Для работы используются следующие концепции:

Все что в мастере - уже рабочее

После мерджа изменений в мастер, оно должно быть задеплоено

[![](https://lh6.googleusercontent.com/ZGzGzsqyj-dHHg4VxOfq8NUNyY2v-0EmeYUlpQnNnWxK37CMYeKQvmK2ZXarIqsiIHt0UeNaHNvk7rCjGS8dokrr2Ej3u5bwh6GkjltKcVapVMoGNr9OkD3o8slv8M-PnIx6ZUA8FHV6PZfqdzOYA8jTKBeYIsFa4rp0cGEBgfeCA4sjO9f0-m_GdrQi)](https://lh6.googleusercontent.com/ZGzGzsqyj-dHHg4VxOfq8NUNyY2v-0EmeYUlpQnNnWxK37CMYeKQvmK2ZXarIqsiIHt0UeNaHNvk7rCjGS8dokrr2Ej3u5bwh6GkjltKcVapVMoGNr9OkD3o8slv8M-PnIx6ZUA8FHV6PZfqdzOYA8jTKBeYIsFa4rp0cGEBgfeCA4sjO9f0-m_GdrQi)

### **Forking Workflow**

Fork the official repository to create a copy of it on the server. This new copy serves as their personal public repository—no other developers are allowed to push to it, but they can pull changes from it (we’ll see why this is important in a moment). When they're ready to publish a local commit, they push the commit to their own public repository—not the official one. Then, they file a pull request with the main repository, which lets the project maintainer know that an update is ready to be integrated. The pull request also serves as a convenient discussion thread if there are issues with the contributed code. The following is a step-by-step example of this workflow.

1. A developer 'forks' an 'official' server-side repository. This creates their own server-side copy.
2. The new server-side copy is cloned to their local system.
3. A Git remote path for the 'official' repository is added to the local clone.
4. A new local feature branch is created.
5. The developer makes changes on the new branch.
6. New commits are created for the changes.
7. The branch gets pushed to the developer's own server-side copy.
8. The developer opens a pull request from the new branch to the 'official' repository.
9. The pull request gets approved for merge and is merged into the original server-side repository

[https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

## **GIT**

**Коммит**

Элемент односвязного списка, состоящий из объектов с измененными файлами и ссылки на предыдущий коммит. При создании коммита, указатель ветки автоматически сдвигается на него.

**Ветка**

Перемещаемый указатель на один из коммитов

**HEAD**

Указатель на текущую ветку

**Index(Staging area)**

Промежуточная область, в которой хранятся изменения файлов на пути от рабочей директории до репозитория. При выполнении коммита, в него попадают только те изменения, которые были в индексе. **git add** - добавляет в индекс.

**Хранение изменений**

При каждом коммите все измененные файлы сохраняются целиком в виде **blob объектов**(заархивированных), которые адресуются с помощью 40 значного SHA1 хэша

[![](https://lh6.googleusercontent.com/_-qr2Cw_QHJQoeF-whPFalbDtUzi8N34ZuBffEusyoLnlRJgwwSjmKWDpD58ncHjUuYPulUdxGUiCzS0cnYKffbvJXKUPAfHMYf5PNaQDSPpfl8A6W2ID4kAYxbOIKYqm0pIv1HIBZVKeknjgHHuem15qiDdSHkDCVSU4iBau0vrey7iLT3PrcfBpCVE)](https://lh6.googleusercontent.com/_-qr2Cw_QHJQoeF-whPFalbDtUzi8N34ZuBffEusyoLnlRJgwwSjmKWDpD58ncHjUuYPulUdxGUiCzS0cnYKffbvJXKUPAfHMYf5PNaQDSPpfl8A6W2ID4kAYxbOIKYqm0pIv1HIBZVKeknjgHHuem15qiDdSHkDCVSU4iBau0vrey7iLT3PrcfBpCVE)

Все эти объекты разбиваются на директории с помощью объектов

**tree**

[![](https://lh3.googleusercontent.com/a1Q8hYShylgIlx4uSfmpSII3v-9_iSl0ena1q8XYDgCmrLTUJjWNHKUtrOHUNhdlma4q6omVUV6X_xXvFzRA7stpq0ZUEo-R98UQwVha-ig7dbEwlAJA7ruqaRuoIemSKla2_5WCVK4VI4jL32vYkAlUEbUNkrl4mfqkjsvaYeq3LuKQvI9j1Pom-z9W)](https://lh3.googleusercontent.com/a1Q8hYShylgIlx4uSfmpSII3v-9_iSl0ena1q8XYDgCmrLTUJjWNHKUtrOHUNhdlma4q6omVUV6X_xXvFzRA7stpq0ZUEo-R98UQwVha-ig7dbEwlAJA7ruqaRuoIemSKla2_5WCVK4VI4jL32vYkAlUEbUNkrl4mfqkjsvaYeq3LuKQvI9j1Pom-z9W)

Содержимое: 100644 blob 3f4e21ab1d7b52e033f1d1c562e5dd51c629cf35    test.cmd

Далее на все это указывает **commit**

[![](https://lh4.googleusercontent.com/YRrEA5LHYY88d0BgB6XDiYTt4eQJd9zHlEpPrDedzvmxKSEwxRX5XoZzCaApliGIHCwhBMwTgMtX9WFZ0T7AGkTcT3l4DiDXKhCNErAsuYVbubWZFF1w59xjz2g51_fYEpn9fbTqRxe9OJRA44sYIUnmH95UAAruG3ZsxXT2vCl0lCmB4uh9S8d_-iMd)](https://lh4.googleusercontent.com/YRrEA5LHYY88d0BgB6XDiYTt4eQJd9zHlEpPrDedzvmxKSEwxRX5XoZzCaApliGIHCwhBMwTgMtX9WFZ0T7AGkTcT3l4DiDXKhCNErAsuYVbubWZFF1w59xjz2g51_fYEpn9fbTqRxe9OJRA44sYIUnmH95UAAruG3ZsxXT2vCl0lCmB4uh9S8d_-iMd)

Содержимое:tree dc786d4599af5a03af1acceebf32bfc206983f49

parent 15e9273774286e8acf47337ab732bdb915fe3596

author ethickfox <ethickfox@gmail.com> 1635023230 +0300

committer ethickfox <ethickfox@gmail.com> 1635023230 +0300

New br commit

Коммиты содержат

- Ссылку на дерево с изменениями
- Предыдущий коммит
- Автор коммита
- Комментарий

Чтобы  все это не занимало очень много места, старые коммиты собираются в один большой архив

[![](https://lh6.googleusercontent.com/e2vkErCDV9KM52ZZk8TVE7-02Cwb6bs6Xs6WEPX5vu8TvT0haYIHT_JTD_KgSiGaQGqfT7ac51XAuPljPHCpIKRwtp50mnyBpdR17yAiVqQgjKRmv6Q0ZHoYIep2Q649fMR4yRkm-0PeuzKxAwvh_5eN9NIWikjLBZ_-cJsr4C3f9GkZULXpRHtvWlk5)](https://lh6.googleusercontent.com/e2vkErCDV9KM52ZZk8TVE7-02Cwb6bs6Xs6WEPX5vu8TvT0haYIHT_JTD_KgSiGaQGqfT7ac51XAuPljPHCpIKRwtp50mnyBpdR17yAiVqQgjKRmv6Q0ZHoYIep2Q649fMR4yRkm-0PeuzKxAwvh_5eN9NIWikjLBZ_-cJsr4C3f9GkZULXpRHtvWlk5)

**git fetch**

Связывается с удаленным репозиторием и получает данные, которые отсутствуют в локальном(Не обновляет, просто подтягивает изменения)

**git pull**

Загружает изменения с сервера и автоматически делает слияние с кодом текущей ветки

**git checkout**

Извлекает содержимое из репозитория и помещает его в рабочее дерево. Можно перемещаться по коммитам, не вносит изменений в историю.

**Объединение коммитов**

С помощью перебазировании в интерактивном режиме. git rebase -i HEAD~5. Далее перед выбранными коммитами ставим squash

**git rebase**

Все изменения из целевой ветки применяются поверх изменений из внедряемой. Номера коммитов целевой ветки изменятся

1)

/o-----o---o--o-----o--------- branch

- -o-o--A--o---o---o---o----o--o-o-o--- master

2)

git checkout branch

git rebase master

/o-----o---o--o-----o------ branch

- -o-o--A--o---o---o---o----o--o-o-o master

Также существует интерактивная версия. Она позволяет выполнить следующие действия с перебазируемыми коммитами:

- p, pick - использовать коммит
- r, reword - использовать + изменить сообщение
- e, edit - использовать + остановить ребейз и исправить коммит
- s, squash - объединить с предыдущим коммитом
- f, fixup - squash, только с обнулением сообщения
- x, exec - выполнить shell команду
- b, break - остановка ребейза
- d, drop - убрать коммит
- l, label <label> = label current HEAD with a name
- t, reset <label> = reset HEAD to a label
- m, merge

**git merge**

Создается новый коммит в целевой ветке, который содержит изменения из внедряемой. A commit, that combines all changes of a different branch into the current.

[![](https://lh6.googleusercontent.com/kPD1erXu_KcJbf0PiVnPVyri2mKRWvqUmdaMwKLutiEAiE89saoe52NwBmsUYcG7tmD99_x_kVs_5CnG1skNmrb1PziAuq3E7_4qyxsFWvxWi60O8RXQqviIaQooMYPpkRFk7vqpXuUqEjPPNYNNMMtdIZTA01e5kaLaFvNXaBjAjclnqb95j43CAtiE)](https://lh6.googleusercontent.com/kPD1erXu_KcJbf0PiVnPVyri2mKRWvqUmdaMwKLutiEAiE89saoe52NwBmsUYcG7tmD99_x_kVs_5CnG1skNmrb1PziAuq3E7_4qyxsFWvxWi60O8RXQqviIaQooMYPpkRFk7vqpXuUqEjPPNYNNMMtdIZTA01e5kaLaFvNXaBjAjclnqb95j43CAtiE)

Также существует fast-forward merge, который не создает новых коммитов, а просто объединяет коммиты в одну ветку, передвигая указатель. Re-comitting all commits of the current branch onto a different base commit.

### **git merge vs rebase**

- merge создаёт коммит слияния (создаёт/загрязняет историю). и можно продолжить работать в двух ветках - и старой и новой. Сохраняет полную историю и хронологический порядок;
- А rebase меняет свою историю.Упрощает сложную историю, можно очистить промежуточные коммиты и сделать их одним.
- rebase более функциональный, но им проще все сломать
- Rebase не изменяет целевую ветку, он «переигрывает» изменения из текущей ветки на вершине целевой ветки, оставаясь в текущей ветке. Для интеграции изменений вам все равно придется сделать merge. Однако поскольку в целевой ветке более не будет изменений, сделанных после точки отбранчевания, мердж будет проведен как fast forward, что означает перенесение маркера вершины целевой ветки на конец текущей ветки без merge-комита.
- merge executes only one new commit. rebase typically executes multiple (number of commits in current branch).
- merge produces a new generated commit (the so called merge-commit). rebase only moves existing commits.
- Use merge whenever you want to add changes of a branched out branch back into the base branch. Typically, you do this by clicking the "Merge" button on Pull/Merge Requests, e.g. on GitHub.
- Use rebase whenever you want to add changes of a base branch back to a branched out branch.Typically, you do this in feature branches whenever there's a change in the main branch.
- If the rebased branch has multiple commits that change the same line and that line was also changed in the base branch, you might need to solve merge conflicts for that same line multiple times, which you never need to do when merging. So, on average, there's more merge conflicts to solve.

|   |   |
|---|---|
|Merge|Rebase|
|Git merge is a command that allows you to merge branches from Git.|Git rebase is a command that allows developers to integrate changes from one branch to another.|
|In Git Merge logs will be showing the complete history of the merging of commits.|Logs are linear in Git rebase as the commits are rebased|
|All the commits on the feature branch will be combined as a single commit in the master branch.|All the commits will be rebased and the same number of commits will be added to the master branch.|
|Git Merge is used when the target branch is shared branch|Git Rebase should be used when the target branch is private branch|

### **Why not use merge to merge changes from the base branch into a feature branch?**

1. The git history will include many **unnecessary merge commits**. If multiple merges were needed in a feature branch, then the feature branch might even hold more merge commits than actual commits!
2. This creates a loop which **destroys the mental model that Git was designed by** which causes troubles in any visualization of the Git history.Imagine there's a river (e.g. the "Nile"). Water is flowing in one direction (direction of time in Git history). Now and then, imagine there's a branch to that river and suppose most of those branches merge back into the river. That's what the flow of a river might look like naturally. It makes sense.But then imagine there's a small branch of that river. Then, for some reason, **the river merges into the branch** and the branch continues from there. The river has now technically disappeared, it's now in the branch. But then, somehow magically, that branch is merged back into the river. Which river you ask? I don't know. The river should actually be in the branch now, but somehow it still continues to exist and I can merge the branch back into the river. So, the river is in the river. Kind of doesn't make sense.This is exactly what happens when you merge the base branch into a feature branch and then when the feature branch is done, you merge that back into the base branch again. The mental model is broken. And because of that, you end up with a branch visualization that's not very helpful.

**git cherry-pick**

Взять отдельный коммит из другой ветки и перенести его на Head текущей

**git reset**

Перемещает выбранный коммит на вершину, стирая изменения

- soft - все изменения остаются подготовленными к коммиту
- mixed -текущие изменения сохранены, но не находятся в индексе, в контроле версий последним отображается выбранный коммит.
- hard - удаляет все, что было сделано после выбранного коммита

**git revert**

Создает новый коммит с отменой изменений.

**Мердж конфликты**

Возникают при наличии изменений в одном месте у обоих сливаемых веток. При таком конфликте гит помечает, откуда какая часть кода была получена. Необходимо вручную внести изменения в файл и сделать коммит решения конфликта
hauteville18620603@c3r1s2 my-workstation % git init
Reinitialized existing Git repository in /Users/hauteville18620603/Desktop/my-workstation/.git/
hauteville18620603@c3r1s2 my-workstation % pwd
/Users/hauteville18620603/Desktop/my-workstation
hauteville18620603@c3r1s2 my-workstation % mkdir 3-Terminal
mkdir: 3-Terminal: File exists
hauteville18620603@c3r1s2 my-workstation % touch Terminal.md

hauteville18620603@c3r1s2 my-workstation % git add .
hauteville18620603@c3r1s2 my-workstation % git commit -m "터
미널 실습 파일 생성"
[main c4621df] 터미널 실습 파일 생성
 Committer: 임유현 <hauteville18620603@c3r1s2.codyssey.kr>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 3-Terminal/Terminal.md
hauteville18620603@c3r1s2 my-workstation % git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 380 bytes | 380.00 KiB/s, done.
Total 4 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/hauteville1862/my-workstation.git
   afc6bf2..c4621df  main -> main
---
layout: post
title: Bilmem Gereken Git Komutları
date:   2018-01-23 20:00:19
excerpt: Git de commit’ler, kaynak dosya içerikleri (blob), dosya/klasör
tags: [git]
comments: true
img: gitLogo.jpg
---

Git de commit’ler, kaynak dosya içerikleri (blob), dosya/klasör yapısı (tree) ve tag'ler Git tarafından birer obje olarak değerlendirir.

**git commit --amend**<br />
Son atılan commit'i değiştirmek için kullanılır.

**git commit --amend --no-edit**<br />
--no-edit commit mesajını değiştirmeden değişikli yapmamızı sağlar.

**git show HEAD**<br />
Son yapılan commit'i ve içeriğini gösterir.

**git-checkout**<br />
Switch branches or restore working tree files

**git checkout -b veya -B new_branch** <br />
Specifying -b causes a new branch to be created. If -B is given, <new_branch> is created if it doesn’t exist; otherwise, it is reset.

**git checkout branch**<br />
Switch branch

**git help komut**<br />
komut: commit, status, clone

**git branch**<br />
İstediğiniz bir anda, elinizdeki kod’dan hızlıca bir ya da N tane kopya çıkartma işlemidir. Yerel operasyondur. Yani yaptığınız her branch, siz paylaşmadıkça sadece sizde bulunur.

**git config --global alias.s status**<br />
alias tanımlamak için kullanılır. git status yerine git s

**git config --global --unset alias.s**<br />
alias'o silmek için kullanılır.

**git config --global rerere.enabled true**<br />
Reuse recorded resolution yani merge esnasında yaşanan conflict’lerin çözümlerini hatırla ve bir daha aynısı olursa otomatik olarak çöz!

**git config --global status.submoduleSummary true**<br />
Submodule’ler le çalıştığınız zaman işinize yarayacak. git status dediğinizde, repo altındaki submodule’lerin de durumunu görmeniz gerekebilir.

**Commit Nedir?**<br />
GIT, kendi içinde özel bir GRAPH yapısı kullanıyor. Bunun adı: Directed Acyclic Graph. GIT komple o anın fotoğrafını çeker. Bu aslında o an’ın snapshot’ı dır ve GIT buna Commit der.

**git log --oneline**<br />
git log larını tek satırda gösterir.

**Commit?**<br />
Commit mesajı 50 karakter uzunluğundadır. Bazı özel durumlar dışında boş commit mesajı olamaz.

**git show commit_id**<br />
Commit'in bilgilerini görmek için kullanırız.

**git help everyday**<br />
Bir geliştiricinin sık sık kullanacağı komutları, nasıl kullanacağı bilgisini ve günlük rutin işlerimize yardımcı olabilecek çalışma yöntemlerini gösteren bir klavuz sunar.

**git HEAD**<br />
HEAD aslında bir kısayol yani alias’dır. Bulunduğunuz branch’deki en yeni / en son commit’i işaret eder.

**git reset HEAD**
Resets the index and working tree. Any changes to tracked files in the working tree since "commit" are discarded.

**git checkout -- file**
Dosyada yapılan değişiklikleri geri alır.

**git add**
Değişiklikleri staging areaya alırız.

Not: Git 3 aşamalı dosya sistemi ile çalışır.Bunlar Staged, Modified ve Commited.

**git add -i**
İnteraktif ekleme modu.

**git status -s**
-s'nin anlamı "short status"

**git status -sb**
-b bulunulan branch'i de göster.
Ekran çıktılarının anlamları;
  = unmodified, boşluk karakteri, değiştirilmemiş
M = modified, değişiklik var
A = added, eklendi yani staged
D = deleted, silindi
R = renamed, dosya adı değişti
C = copied, kopyalandı
U = updated but unmerged, index güncellendi ama merge edilmedi

**git log --oneline --stat**
Yapılan commit'i daha rahat anlamamıza yardımcı olur.

**git log --graph --decorate --oneline**
Merge edilmiş commitleri içindeki commitler ile gösterir.

**git status --untracked-files**
Takibe alınmamış untracked klasörlerinin alt dizinindeki untracked dosyaları ayrıca görmemizi sağlar.

**git mv dosya dosya_yeni_isim**

**git rm file ve git rm -r folder**
Git de file ve folder silmek için.

**git branch**
Repodaki tüm branchleri listeler.

**git checkout brancd_adi**
Branch değiştirmek

**git merge birlestirilecek_brach_adi**

**git branch -d silinecek_branch_adi**

**git checkout -b branch_adi**
Branch oluştur ve checkout yap.

**git checkout --theirs dosya_adi**
merge yaparken hangi branch'in değişikliklerini alacağını karar veriyoruz.

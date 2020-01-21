# https://github.com/Ryoukan/ansible-training  
# ansible-training
### テキスト  
https://github.com/hiro52/linklight/blob/devel/exercises/ansible_rhel/README.ja.md  

### 誤植  
https://github.com/hiro52/linklight/blob/devel/exercises/ansible_rhel/2.2-cred/README.ja.md#%E3%83%9E%E3%82%B7%E3%83%B3%E8%AA%8D%E8%A8%BC%E6%83%85%E5%A0%B1%E3%81%AE%E4%BD%9C%E6%88%90  
マシン認証情報の作成  
誤　パスワード: ansible  
正　パスワード: <POD情報のSSHパスワード>



### POD情報
http://5fe7.rhdemo.io

### REDHATサポートモジュール
https://docs.ansible.com/ansible/latest/modules/core_maintained.html 


### 追加解説  
1.3.1 YAML書き方  
シーケンス（リストや配列のこと）  
```
- A
- B
```
階層構造（インデントで表す　２文字が多い　タブは使えない）
```
- A
- B
  - C
```
マッピング（:の後ろに半角スペース）
```
A: aaa
```
1.4.1 変数ファイル  
ini形式のインベントリファイルに以下のように記載が基本 
今回の演習は以下の変数を外だしした形。
```
[group]
test-server ansible_host=10.1.1.1 #ホスト変数

[group:vars]　#グループ変数
inventory_group=local2　
```
外出しする際の命名規則は、  
group_vars/<グループ名>.yml  
group_vars/<グループ名>/xxx.yml  

host_vars/<ホスト名>.yml  
host_vars/<ホスト名>/xxx.yml    

1.5.2 ハンドラ― 概要  
「notify」を指定したタスクが更新された場合（changedの場合）のみ  
notifyと同じ名前のハンドラタスクを実行する  

1.7 Roles  
- Ansibleが認識できるディレクトリ構造で作成する必要がある  
- ディレクトリ配下のファイルは「main.yml」が固定で読み込まれる
（他の名前は不可 includeなどが必要）  
- ディレクトリ構造の意味
```
roles/
    common/               # ロール名のディレクトリ
        tasks/            #　
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            # コピーモジュールなどで利用するファイル
            bar.txt       #  <-- ターゲットノードに配置するファイル
            foo.sh        #  <-- ターゲットノードに配置するスクリプト
        vars/             #
            main.yml      #  <--　ロール内で利用する変数を定義
        defaults/         #
            main.yml      #  <-- デフォルトの変数、varsよりも優先度は低い
```
- tasksディレクトリについて
「tasks:」ディレクティブは定義しなくてもよい  
```
---
tasks: # 不要
- name: install httpd
  yum:
    name: httpd
    state: latest
```
- Ansible Galaxyについて
ロールが登録されている共有リポジトリがAnsible Galaxy
```
[root@localhost home]# ansible-galaxy common init
- common was created successfully
[root@localhost home]#
[root@localhost home]#
[root@localhost home]# ls
common  roles  wlc  wlc-conf
[root@localhost home]# cd common/
[root@localhost common]# ls
defaults  files  handlers  meta  README.md  tasks  templates  tests  vars
```



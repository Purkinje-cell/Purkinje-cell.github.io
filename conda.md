作者： 王沛宇

## Miniconda3

miniconda3是轻量化的python环境版本管理器，可以非常方便的进行环境安装、版本管理。

### 下载安装

* 通过官网下载
  [下载地址：https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html)

  指令`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

* 安装

  * 运行权限

    `chmod +x Miniconda3-latest-Linux-x86_64.sh`

  * 安装

    `bash Miniconda3-latest-Linux-x86_64.sh`或`./Miniconda3-latest-Linux-x86_64.sh`

  * 接受许可协议

  * 确认安装路径，比如`/home/leb03/miniconda`

  * 确认初始化程序

    > 作用：在你的`~/.bashrc`文件里添加了这样一段代码：
    >
    > ```bash
    > # >>> conda initialize >>>
    > # !! Contents within this block are managed by 'conda init' !!
    > __conda_setup="$('/rd1/home/wangpy/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
    > if [ $? -eq 0 ]; then
    >     eval "$__conda_setup"
    > else
    >     if [ -f "/rd1/home/wangpy/miniconda3/etc/profile.d/conda.sh" ]; then
    >         . "/rd1/home/wangpy/miniconda3/etc/profile.d/conda.sh"
    >     else
    >         export PATH="/rd1/home/wangpy/miniconda3/bin:$PATH"
    >     fi
    > fi
    > unset __conda_setup
    > # <<< conda initialize <<<
    > ```
    >
    > 其作用是将conda宏指令、conda/bin中的模块装入系统环境

* 确认安装

  运行一下重载bash：`source ~/.bashrc`

  确认conda安装：`conda --version`

### 源、包与环境管理

#### 源设置

* 常用源

  `conda config –-add channels conda-forge`

  `conda config –-add channels r`

  `conda config --add channels bioconda`

* 清华软件源加速：

  `conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free`

  `conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge`

  `conda config --set show_channel_urls yes`

* 查看所有源：

  `conda config --show channels`

* 删除源：

  `conda config –remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free`

#### 环境管理

* 系统启动时你已经处于`base`环境中，是conda启动时的默认环境

* 创建环境

  `conda create -n foo python=3.8`，以创建一个名为`foo`并安装了python3.8的环境

* 进入环境

  `conda activate foo`

* 退出环境

  `conda deactivate`

* 删除环境

  `conda remove -n foo –-all`

* 查看所有环境

  `conda env list`或`conda info -e`

  > 其他环境与base环境的关系，base是conda的默认环境，可以做修改；创建环境时，base环境中的包被默认认为是相应的备选版本，但并不会默认继承
  
* 环境添加变量

  `conda-envs config vars set my_var=blablabla`

#### 包管理

* 搜索包

  `conda search pkg_name`

* 包安装

  `conda install pkg_name`或指定版本`conda install pkg_name=version`

  > 可以同时安装多个`conda install pkg_name1 pkg_name2 ...`
  >
  > 指定从某个源安装`conda install -c channel pkg_name`
  >
  > 指定某个网站`conda install -i src pkg_name`

* 包删除

  `conda remove pkg_name`

* 列出环境中所有包

  `conda list`或指定环境`conda list -n foo`

* 安装官网下载包

  首先将安装包文件复制到anaconda安装目录下的pkgs目录，然后运行`conda install --use-local pkg_name`

  同时conda对于已经下载的对应版本包，不会在配置环境时从新下载

  

  1. 关于源的一些事情：
  
     清华源、中科大源
  
     conda源搜包的方法：按照源从高到低的方式，具体到网址会从`网址:源名`搜索
  
  2. 安装hmmer
  
     创建一个新的环境
  
     使用public下面的内容`--use-local`
  
     安装查看版本
  
  3. 使用hmmer
  
     相应数据已经在hmmer
  
     首先hmmbuild构建Profile，即生成一个hmm模型
  
     看一下多序列比对文件
  
     `hmmbuild 15ATSBPD.aln`，发现旧版和新版的没有差别
  
     接下来hmmsearch搜索数据库，查看有哪些同源序列
  
     `hmmsearch MYB.hmm uniprot_sprot.fasta > MYB.search.out`
  
     接下来用phmmer进行balst
  
     `phmmer HBB_HUMAN uniport_sprot.fasta > HBB_HUMAN.phmmer.out`
  
     接下来用jackhmmer进行迭代的蛋白质搜索，类似PSI-BLAST，不同的是建立的是profile而不是打分矩阵
  
     `jackhmmer -E 0.001 –nlali -N 6 HBB_HUMAN uniprot_sprot.fasta > HBB_HUMAN.jackhmmer.out`

4. 用序列搜索数据库

   构建profile数据库文件

   `hmmbuild`可以构建，但是服务器上的`pfam.hmm`已经狗年后了

   构建索引和压缩

   `hmmpress pfam`

   搜索profile数据库

   `hmmscan pfam HBB_HUMAN > HBB_HUMA.scan.out`

   统计信息

   `hmmstat pfam`

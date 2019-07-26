
<h2 id="目录">目录</h2>


<div>
	<ul>
	<li><a href="#简介">简介</a></li>
	<li><a href="#更新">更新</a></li>
	<li><a href="#版本">版本</a></li>
	<li><a href="#集群路径">集群路径</a></li>
	<li><a href="#使用方法">使用方法</a>
		<ul>
			<li><a href="#初始化lims账号信息">初始化lims账号信息</a></li>
			<li><a href="#SOP编号查询">SOP编号查询</a></li>
			<li><a href="#结题报告上传">结题报告上传,同时释放数据</a></li>
			<li><a href="#QC报告上传">QC报告上传</a></li>
			<li><a href="#Mapping">Mapping 报告上传</a></li>
			<li><a href="#重新释放数据">重新释放数据</a></li>
			<li><a href="#查看项目分期状态">查看项目分期状态</a></li>
		</ul> 
	</li>
	<li><a href="#FAQ">FAQ</a>
		<ul>
			<li><a href="#FAQ1">脚本环境</a></li>
			<li><a href="#FAQ2">lims返回值</a></li>
		</ul> 
	</li>
	</ul> 
	
</div>

<h2 id="简介">简介<a href="#目录">[top]</a></h2>
`Lims_report_uploader`是用于将结题报告从天津或南京集群传到lims系统的工具.

v2.0 新增了释放数据功能，可用于在天津集群和南京集群通过命令行的方式释放数据。详细使用方法如下。

<h2 id="更新">更新<a href="#目录">[top]</a></h2>
2019-07-26
> 1. 添加对“checkSize.xls”校验, 释放目录下必须有此文件.如果无此文件，将尝试生成“checkSize.xls”，但必须有释放目录的写权限;
> 2. 在程序路径下添加最新版本的"dirCheckSize2.pl"脚本, 可通过此脚本生成“checkSize.xls”文件;
> 3. 添加SOP校验，上传结题报告时所给SOP必须都是此产品的SOP;
> 4. 添加了释放文件个数校验。小文件太多会严重拖慢云交付数据上传速度，因此，对文件数目做了限制。单批次释放目录下，文件个数不能超过500个;


2019-06-19

> 1. 修复自动获取集群地函数get_JQLAND,增加重试次数,降低集群网络不稳定的影响，建议运行释放数据时添加'--jq_local' 参数，以保证网络不稳定时可以正常运行；
> 2. 描述信息中添加了本文档链接<a href="https://lidanqing123.github.io/lims_report_upload_v2.0/">地址</a>；

2019-06-18
> 1. 对数据释放路径强制限定，必须为目录，否则报错提醒；
> 2. 添加了stage_info命令，可以用于查询项目分期结题状态，释放状态，交付状态和时间，路径等；

2019-06-03
> 1. 初始化时，发件邮箱配置改为可选（考虑到一些集群节点无法连接外网），若不配置则无法从集群发送邮件；
> 2. 根据业务逻辑需要，将数据释放和报告上传都放在‘R’命令下执行，要求上传报告的同时释放数据；
> 3. 增加释放数据时集群地点配置参数`--jq_local`或`-j`，但是，优先会根据集群服务器IP来判断；
> 4. ’D‘命令改为用于重新释放数据

<h2 id="版本">版本<a href="#目录">[top]</a></h2>
<p><a href="https://lidanqing123.github.io/lims_report_upload_v2.0/">v2.0 </a></p>
   
<h2 id="集群路径">集群路径<a href="#目录">[top]</a></h2>

天津集群
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python /TJPROJ1/MICRO/lidanqing/lims/lims_report_upload_v2.0/Lims_report_uploader -h
```
南京集群
```
/NJPROJ2/MICRO/share/software/Anaconda/anaconda3/bin/python  /NJPROJ2/MICRO/PROJ/lidanqing/lims/lims_report_upload_v2.0/Lims_report_uploader -h
```

<h2 id="使用方法">使用方法</h2>

* 首先,需要通过`init`命令初始化配置自己的lims账号和密码,此后,使用不需要再重新配置.
* 使用`R`命令来上传结题报告,同时释放数据;
* 如果不清楚SOP编号,可通过`search`命令查询可选SOP
* 重新释放数据使用’D’命令;
* 项目分期结题，释放，交付状态等信息可以使用’stage_info’命令;

```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py -h
```

```
usage: Lims_report_uploader.py [-h] [-V] {init,search,R,Q,M,D,stage_info} ...

Description:
这个脚本用于将结题报告从集群传到lims系统中.
    1 首先,需要通过’init’命令初始化配置lims账号和密码;
    2 上传结题报告,同时释放数据，使用’R’命令;
    3 如果不清楚SOP编号,可通过’search’命令查询可选SOP
    4 重新释放数据使用’D’命令;
    5 项目分期结题，释放，交付状态等信息可以使用’stage_info’命令;

author: lidanqing@novogene.com
document: https://lidanqing123.github.io/lims_report_upload_v2.0/
version:  2.0

positional arguments:
  {init,search,R,Q,M,D,stage_info}
                        sub-command
    init                开始初始化lims账户信息
    search              查询项目SOP
    R                   上传结题报告,同时释放数据
    Q                   上传QC报告
    M                   上传Mapping报告
    D                   重新释放数据
    stage_info          查询项目信息

optional arguments:
  -h, --help            show this help message and exit
  -V, --version         show program's version number and exit

Example:
            Lims_report_uploader.py init -h
            Lims_report_uploader.py search -h
            Lims_report_uploader.py R -h
            Lims_report_uploader.py Q -h
            Lims_report_uploader.py M -h
            Lims_report_uploader.py D -h
            Lims_report_uploader.py stage_info -h
```

<h3 id="初始化lims账号信息">初始化lims账号信息</h3>
初始化lims账号信息直接使用`init`命令
```
$ /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py init
```
查看帮助信息可加上`-h`参数:
```
$ /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py init -h 
usage: Lims_report_uploader init [-h]

Description:
    ’init’命令用于初始化配置lims账号和密码等信息.命令行中不用给其他任何参数.

optional arguments:
  -h, --help  show this help message and exit

Example:
    Lims_report_uploader init

```

<h3 id="SOP编号查询">SOP编号查询<a href="#目录">[top]</a></h3>

通过项目分期编号,可以使用`search`命令查询可选的SOP

```
$  /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py search -s P101SC18072239-01-F002 
	SOP_code	SOP方法
############################################################
	SOPMC00038	ANI分析（每株）
	SOPMC00039	cgMLST
	SOPMC00040	QC
	SOPMC00041	比较基因组分析（SNP／InDel／SV检测及注释）
	SOPMC00042	比较基因组分析（共线性）
	SOPMC00043	标准分析
	SOPMC00044	群体进化分析（core-pan基因分析）
	SOPMC00045	群体进化分析（基因家族）
	SOPMC00046	群体进化分析（进化选择压力分析）
	SOPMC00047	群体进化分析（群体地理传播途径预测）
	SOPMC00048	群体进化分析（群体进化分歧时间预测）
	SOPMC00049	群体进化分析（群体进化树分析）
	SOPMC00050	转座子分析（基于蛋白+核酸序列）
	SOPMC00051	转座子分析（基于蛋白序列）
	SOPMC00052	转座子分析（基于核酸序列）

```
查看帮助信息可使用`-h`参数:
```
$  /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py search -h
usage: Lims_report_uploader search [-h] -s STAGECODE

Description:
    提供项目分期编号,使用'search'命令,可查询此产品可选SOP.

optional arguments:
  -h, --help            show this help message and exit
  -s STAGECODE, --stage_code STAGECODE
                        分期编号

Example:
    Lims_report_uploader search -s  P101SC18072239-01-F002
    Lims_report_uploader search --stage_code  P101SC18072239-01-F002
```

<h3 id="结题报告上传">结题报告上传,同时释放数据<a href="#目录">[top]</a></h3>
报告上传,同时释放数据是此程序的主要功能. 需要配置的参数相对较多:
查看帮助信息可使用`-h`参数:

查看帮助信息，命令：
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python /TJPROJ1/MICRO/lidanqing/lims/lims_report_upload_v2.0/Lims_report_uploader R -h
```
或
```
/TJPROJ1/MICRO/lidanqing/lims/lims_report_upload_v2.0/Lims_report_uploader R -h
```
输出如下：
```
usage: Lims_report_uploader.py R [-h] -i file -s STAGECODE -p RELEASE_PATH
                                 [-j {nj,tj}] [-l file] -d num --SOP SOP
                                 [-m REMARK] [--comments COMMENTS] [-e EMAIL]

Description:
    'R'是本脚本最核心的功能,用于上传结题报告,同时释放数据.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中结题报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -p RELEASE_PATH, --path RELEASE_PATH
                        集群中释放数据的路径
  -j {nj,tj}, --jq_local {nj,tj}
                        集群所在地, "nj":南京集群, "tj":天津集群, 可选。默认根据集群服务器IP判断集群所在地。
  -l file, --sample_list file
                        样本列表，每行一个样本诺和编号
  -d num, --total_data num
                        总数据量,以G为单位
  --SOP SOP             SOP编号,多个SOP编号的话以英文字符逗号分开,like:"SOPMC00038,SOPMC00039".
                        配置前可以用过'search'命令查询可选SOP.
  -m REMARK, --remark REMARK
                        结题报告备注信息,默认为空
  --comments COMMENTS   释放数据备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py R -i Report-P101SC1807xxxx-01-B1-3.zip -p path2release -j tj -s P101SC1807xxxx-01-F002 -l samplelist.txt -d 2 --SOP SOPMC00039,SOPMC00040 -m "结题报告备注" --comments "释放数据备注" -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py R --input Report-P101SC1807xxxx-01-B1-3.zip --path path2release --jq_local tj  --stage_code P101SC1807xxxx-01-F002 --sample_list samplelist.txt --total_data 2 --SOP SOPMC00039,SOPMC00040 --remark "结题报告备注" --comments "释放数据备注" --email "lidanqing@novogene.com;liuchen@novogene.com"


```

sample_list 文件格式:
```
$ cat samplelist

FKRO170938486-1A
FKRO170938276-1A
FKRO170938277-1A
FKRO170938278-1A
FKRO170938279-1A
FKRO170938280-1A
FKRO170938281-1A
FKRO170938282-1A
```

<h3 id="QC报告上传">QC报告上传<a href="#目录">[top]</a></h3>
查看帮助信息，命令：
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python ./Lims_report_uploader Q -h
```
输出如下：
```
usage: Lims_report_uploader.py Q [-h] -i file -s STAGECODE [-m REMARK]
                                 [-e EMAIL]

Description:
    'Q'的功能是用于上传QC报告.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中QC报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py Q -i QC-P101SC1807xxxx-01-B1-3.zip  -s P101SC1807xxxx-01-F002 -m "QC报告备注"  -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py Q --input QC-P101SC1807xxxx-01-B1-3.zip --stage_code P101SC1807xxxx-01-F002 --remark "QC报告备注"  --email "lidanqing@novogene.com;liuchen@novogene.com"
```

<h3 id="Mapping">Mapping 报告上传<a href="#目录">[top]</a></h3>

查看帮助信息，命令：
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python ./Lims_report_uploader M -h
```
输出如下：
```
usage: Lims_report_uploader.py M [-h] -i file -s STAGECODE [-m REMARK]
                                 [-e EMAIL]

Description:
    'M'的功能是用于上传 mapping 报告.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中mapping报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py M -i Mapping-P101SC1807xxxx-01-B1-3.zip -s P101SC1807xxxx-01-F002 -m "Mapping报告备注"  -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py M --input Mapping-P101SC1807xxxx-01-B1-3.zip --stage_code P101SC1807xxxx-01-F002 --remark "Mapping报告备注" --email "lidanqing@novogene.com;liuchen@novogene.com"
```
<h3 id="重新释放数据">重新释放数据<a href="#目录">[top]</a></h3>
查看帮助信息，命令：
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python ./Lims_report_uploader D -h
```
输出如下：
```
usage: Lims_report_uploader.py D [-h] -p RELEASE_PATH -s STAGECODE
                                 [-j {nj,tj}] [-m REMARK] [-e EMAIL]

Description:
    'D'的功能是用于重新释放数据.

optional arguments:
  -h, --help            show this help message and exit
  -p RELEASE_PATH, --path RELEASE_PATH
                        集群中释放数据的路径
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -j {nj,tj}, --jq_local {nj,tj}
                        集群所在地, "nj":南京集群, "tj":天津集群, 可选。默认根据集群服务器IP判断集群所在地。
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py D -p path2release -s P101SC1807xxxx-01-F002 -m "重新释放数据备注"  -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py D --path path2release --stage_code P101SC1807xxxx-01-F002 --remark "重新释放数据备注" --email "lidanqing@novogene.com;liuchen@novogene.com"
```

<h3 id="查看项目分期状态">查看项目分期状态<a href="#目录">[top]</a></h3>
查看帮助信息，命令：
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python ./Lims_report_uploader stage_info -h
```
输出如下：
```
usage: Lims_report_uploader.py stage_info [-h] -s STAGECODE

Description:
    提供项目分期编号,使用'stage_info'命令,可查询此项目交付信息.

optional arguments:
  -h, --help            show this help message and exit
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号

Example:
    Lims_report_uploader.py stage_info -s  P101SC18072239-01-F002
    Lims_report_uploader.py stage_info --stage_code  P101SC18072239-01-F002
```

<h2 id="FAQ">FAQ<a href="#目录">[top]</a></h2>

<h3 id="FAQ1">脚本环境<a href="#目录">[top]</a></h3>
<h4 id="FAQ1.1">1. UnicodeEncodeError: 'charmap' codec can't encode characters in position 85-100</h4>

![](https://raw.githubusercontent.com/lidanqing123/Lims__report_uploader/master/QQ%E5%9B%BE%E7%89%8720181226200832.png)

假如出现这个报错.可能是linux系统的stdout编码不对.可通过如下命令查看.

![](https://raw.githubusercontent.com/lidanqing123/Lims__report_uploader/master/QQ%E5%9B%BE%E7%89%8720181226200951.png)
   
用过`vi`命令将`~/.bashrc`文件加入下面一行信息.把环境变量LANG改为'zh_CN.UTF-8'
```
export LANG='zh_CN.UTF-8'
```

<h4 id="FAQ1.2">2 因为环境变量问题无法运行,又不想修改`~/.bash_profile`和`~/.bash_profile `里面的环境变量</h4>


可以通过下面的方式解决：
```
export PYTHONPATH=""
export PATH=""
export LD_LIBRARY_PATH=""
/TJPROJ1/MICRO/lidanqing/lims/lims_report_upload_v2.0/Lims_report_uploader -h
```
<h3 id="FAQ2">ims返回值<a href="#目录">[top]</a></h3>

...


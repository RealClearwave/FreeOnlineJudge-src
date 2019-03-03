翻了一下之前的日报，发现还没有人教如何手写OJ。那么~~捡漏走起~~


------------


## 概述
首先，这篇文章介绍如何一步步**手写**OJ。

也许你会问“为什么我们不直接配置HustOJ/UniversalOJ呢？”

下面我们了解一下配置现成OJ的缺点：
- 安装配置复杂（大多数需要安装mysql,而mysql对armhf平台有兼容性问题QAQ）
- 系统占用高 （树莓派和小型vps很难带动，更别提执行效率了）
- 不易上手（看代码试图查错会导致一脸懵逼）

为什么不在 _洛谷_ 之类的OJ建立团队呢？

~~嗯...你不想看管理员脸色活着吧。~~

## 前置知识
- [PHP基础语法](http://www.runoob.com/php/php-tutorial.html)
- [html 基础语法](http://www.runoob.com/html/html-tutorial.html)
- [linux shell基础语法](http://www.runoob.com/linux/linux-shell.html)

## 实现原理
![](https://i.loli.net/2018/12/30/5c283d6850a66.png)

大概就是这样。
## 创建调试平台

本项目在Codeanywhere上进行编写、调试。

[Codeanywhere](codeanywhere.com),一个在线PHP执行平台，速度快，功能强大。

![](https://cdn.luogu.org/upload/pic/47410.png)

在codeanywhere注册一个账号，新建一个Project。

关于Codeanywhere的使用**不再赘述**，身为开发者多年的你一定会认为这个IDE简单有趣。

另外，后期我主要使用[Codenvy](codenvy.io),因为我把CodeAnywhere的账号密码忘掉了。
## 编写过程 && 代码解析
一下部分有完善的注释，直接阅读注释即可。

$\color{red}{\text{Day4及以后的项目每次运行前要先执行/setperm.sh 或 admin/setperm.sh修复权限，切记切记！}}$
$\color{red}{\text{Day4及以后的项目每次运行前要先执行/setperm.sh 或 admin/setperm.sh修复权限，切记切记！}}$
$\color{red}{\text{Day4及以后的项目每次运行前要先执行/setperm.sh 或 admin/setperm.sh修复权限，切记切记！}}$
![](https://i.loli.net/2019/01/31/5c5271864358e.png)
### Day1：简单IDE
Day1[**全部代码下载**](https://share.weiyun.com/5T1hhQD)

我们从评测部分写起。Day1的任务是实现一个简单的ide，类似[洛谷IDE](https://www.luogu.org/app/ide_frame)。

观察实现原理图，我们发现需要创建一下的文件：
- **submit.html** : **submit.php**的前端显示页面
- **submit.php** : 用于创建**judge.cpp**和**data.in**并在评测后返回结果
- **judge.sh** : 编译**judge.cpp**并进行测试，输出**data.out**


------------

首先创建一个文件夹：judge，新建一个脚本：judge.sh,在新弹出的编辑器里写入
```
# judge.sh 类似对拍程序。
rm judge # 保险起见，删除上次的可执行文件。
g++ judge.cpp -O2 -o judge # 编译
./judge < data.in > data.out # 运行
```
**记得**`chmod 755 judge.sh` 


------------

然后是**submit.php**。先看**submit.php**的伪代码：

```
删除上次评测时的临时文件
写入judge.cpp和data.in
调用judge.sh编译
将data.out输出至网页
```

下面上代码。
```
<?php

$code = $_POST["code"];// 获取 代码
$input = $_POST["input"];//获取 输入

unlink ("judge"); //删除 judge，以下类推
unlink ("data.in");
unlink ("data.out");
unlink ("judge.cpp");

$file = fopen("judge.cpp","w+") or exit("Unable to Open CPP."); //将 代码 写入 judge.cpp
fwrite($file,$code);
fclose($file);

$fin = fopen("data.in","w+") or exit("Unable to Open Input"); //将 输入 写入 data.in
fwrite($fin,$input);
fclose($fin);

system("./judge.sh");//执行 judge.sh

$fout = fopen("data.out","r") or exit("Unable to Open File."); // 输出 data.out
while (!feof($fout)){
  echo fgets($fout);
}
fclose($fout);

?>
```


------------

然后**submit.html**就极其简单易懂了。
```
<html>

<head>
  <title>Submit Your Code</title>
</head>

<body>
  <form action="submit.php" method="post">
    <textarea name="code" height="200px" width="80px"></textarea>
    <br>
    <textarea name="input" height="100px" width="40px"></textarea>
    <input type="submit">
  </form>
</body>

</html>
```


------------

### Day2：简单OJ
Day2[**全部代码下载**](https://share.weiyun.com/54JYlhO)

一个OJ只有IDE是不行的，还需要测试数据和题面展示。
测试数据的摆放方式是这样的：

![](https://i.loli.net/2018/12/30/5c283d68251ff.png)

首先我们要修改`/judge/submit.php`,让它支持多组数据的评测。
```
<?php

function judge($code,$path){
  //echo $path;
  unlink ("judge");
  unlink ("data.in");
  unlink ("data.out");
  unlink ("judge.cpp");

  $file = fopen("judge.cpp","w+") or exit("Unable to Open CPP.");
  fwrite($file,$code);
  fclose($file);

  if (!is_file($path)) exit("Bad Problem ID!");

  copy($path,"./data.in");

  system("./judge.sh");

  $fout = fopen("data.out","r") or exit("Unable to Open File.");
  $ret = "";
  while (!feof($fout)){
    $ret = $ret . fgets($fout);
    //echo $ret;
  }
  fclose($fout);
  return $ret;
}

$code = $_POST["code"]; //代码
$pid = $_POST["pid"]; // ProblemID

$curj = 1;//初始化当前数据点编号
$p_fnt = "../problems/" . $pid . "/"; //生成数据目录相对路径

while (is_file($p_fnt . "data" . (string)($curj) . ".in")){//枚举数据点编号
  //echo $p_fnt . "data" . (string)($curj) . ".in";
  judge($code,$p_fnt . "data" . (string)($curj) . ".in");//评测当前测试点
  
  $file1 = md5_file("./data.out"); // 计算提交代码的输出的MD5
  $file2 = md5_file($p_fnt . "data" . (string)($curj) . ".out");//计算数据文件中输出的MD5
  
  if ($file1 == $file2) //比对
     echo "AC<br>";
  else
     echo "WA<br>";
  
  
  $curj = $curj + 1; //下一个测试点
}

?>

```

------------

然后修改`judge/submit.html`(略,详见代码包)

收集任意一题的数据包（此处以`A+B Problem`为例），解压至`problem/1001`。

在`problem/1001`中新建题面文件`cover.html`，写入
```
<title>A+B Problem</title>
<h1>A+B Problem</h1>
<p>
  Please Caculate A+B.
  <br>
  Input:a,b:Integer
  <br>
  Output:a+b
</p>

```

------------

写完了...不，没有。还有`show.php`呢。

show.php的访问格式为`show.php?pid=[你要访问的ProblemID]`

下面上代码：
```
<?php

$probid = $_GET["pid"]; //ProblemID
$cover = "./problems/" . $probid . "/cover.html"; //题面相对目录

if (!is_file($cover)) exit("Bad Problem ID!");//确认题面存在

$file = fopen($cover,"r") or exit("Bad Cover!");

echo "<h3>Problem id:" . (string)$probid . "</h3>";//输出PID

while (!feof($file)){
  echo fgets($file);//逐行输出题面
}

fclose($file);

echo "<a href=\"judge/submit.html\">Submit</a>"; //submit链接

?>
```


------------

到Day2为止，一个基本的OJ就建成了。它拥有**题面展示**、**评测**功能。


------------
### Day3：小优化 && TLE的判别
Day3[**全部代码下载**](https://share.weiyun.com/53vaJHt)

首先优化了一下结果显示页，添上了测试点编号和颜色，详见代码包。

一个网站没有`index.php`怎么行呢？
我们简单的写一下`index.php`，让它可以显示所有题目。

```
<?php
// /index.php
exec("ls ./problems",$prbs); // 获取所有文件
$cur = [];
foreach ($prbs as $cur)
  echo "<a href=\"show.php?pid=" . $cur . "\">" . $cur . "</a>"; //逐个输出
?>
```

按理说这样这一节就写完了，然后我意识到，这样的话就没法判别TLE了。无法判别TLE会使试图卡暴力的毒瘤出题人不开心，所以我们要加入TLE的判别。

如何实现TLE的判别呢？我们选择使用linux的命令“timeout”
`Usage:timeout time command [return_val]`

好了，在了解语法后，我们创建一个`/judge/judge.php`，然后删除`/judge/judge.sh`，因为它已经光荣退役了。

下面上`/judge/judge.php`的代码：
```
<?php
function Do_judge($time_limit){ //进行评测
  unlink ("judge");
  unlink ("data.out");
  system("g++ judge.cpp -O2 -o judge"); //编译
  //echo "timeout " . (string)($time_limit) . " ./judge < data.in > data.out";
  $return_var = 0;
  system("timeout " . (string)($time_limit) . " ./judge < data.in > data.out",$return_var); //测试
  if ($return_var == 124) return false; else return true;
}
?>
```

然后修改`/judge/submit.php`

```
<?php
require ("judge.php"); //这里！

function judge($code,$path,$t_l){
  (略)
  return Do_judge($t_l); //这里！ true:没TLE false:TLE

  //$fout = fopen("data.out","r") or exit("Unable to Open File.");
  //$ret = "";
  //while (!feof($fout)){
  //  $ret = $ret . fgets($fout);
    //echo $ret;
  //}
  //fclose($fout);
  //return $ret;
}
(略)

while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
  //echo $p_fnt . "data" . (string)($curj) . ".in";
  echo "<h3>Test Case #" . (string)($curj) . ":</h3>";
  if (!judge($code,$p_fnt . "data" . (string)($curj) . ".in",1/*时限一秒*/))//这里！
    echo "<h3 style=\"background-color:#4169E1\">TLE</h3>";
  else{
  (略)
  }
(略)
}

?>
```

------------
### Day4：题目的属性`attribute.json` && CE的判定
Day4[**全部代码下载**](https://share.weiyun.com/5unEN5n)

AC...WA...TLE(RE)...还差CE。如何判定CE呢？看看编译后有没有输出文件即可。

核心代码如下。
```
  （略）
  if (!file_exists("judge")){ ///没有输出judge
    return -1;
  }
  （略）
```

我们修改一下`submit.php`中显示答案的部分，让它兼容现有的`judge.php`。

```
<?php
（略）

function judge($code,$path,$t_l){
（略）
}
（略）
while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
  echo "<h3>Test Case #" . (string)($curj) . ":</h3>";
  $j_stat = judge($code,$p_fnt . "data" . (string)($curj) . ".in",(int)(get_prb_time_limit($pid)));
  if ($j_stat == 0){
    echo "<h3 style=\"background-color:#4169E1\">TLE</h3>";
  }elseif ($j_stat == -1){
    echo "<h3 style=\"background-color:#FF4500\">CE</h3>";
  }elseif ($j_stat == 1){
	（略）
  }
（略）
}

?>

```
测试一下...可以判别CE了。


------------

我们的代码里写着这样一行:
`if (!judge($code,$p_fnt . "data" . (string)($curj) . ".in",1/*时限一秒*/))`

可是，每题的时限怎么可能都是一秒呢？我们要想办法解决这个问题。

PHP有自带的保存数据的方式`json`,它简单有趣，可以通过`json_encode`函数解析成数组。
我们在`/library/prbinfo.php`中写入
```
<?php
//be called like 'require ("./library/prbinfo.php");'
function get_prb_info($pid){ //获取题目信息
    $file = fopen("../problems/" . (string)($pid) . "/attrib.json","r");
    $attr = fgets($file);
    return json_decode($attr,true);
}

function get_prb_name($pid){//获取题目名称
    $pinfo = get_prb_info($pid);
    return $pinfo["name"];
}

function get_prb_time_limit($pid){//获取题目时限
    $pinfo = get_prb_info($pid);
    return $pinfo["time_limit"];
}
?>
```
然后把那一行改成`$j_stat = judge($code,$p_fnt . "data" . (string)($curj) . ".in",(int)(get_prb_time_limit($pid)));`

别的文件也要进行适配修改，此处不再赘述。

我们还加入了`setperm.sh`和`unperm.sh`便于修复权限。此处不再赘述。

至此，我们已经凑齐了四种编译状态。

------------
### Day5：评测队列 && 小优化
Day5[**全部代码下载**](https://share.weiyun.com/5MRghBY)

想象一下两个用户在你的OJ同时提交了代码，突然...
![500](https://i.loli.net/2019/01/31/5c5251181170f.png)

为什么呢？新来的`judge.cpp`把旧的覆盖了！

所以我们要实现**评测队列**。评测队列就相当于操作系统中的多任务，把每个评测都放到独立的**容器**中评测，互不干扰。

正常评测队列原理如下。
```
开一个Docker
把代码放进去跑
关闭该Docker
```
实现评测队列，其实就是把原来的`judge.php`,`submit.php`,`submit.html`三个核心评测文件单独复制到一个特殊的文件夹内进行评测。文件夹较docker轻量化，占用资源更少。~~其实只是docker咕了~~

我们把`judge`文件夹重命名为`judge-module`，作为评测机的模板。
将`submit.php`中的评测函数搬到`judge.php`中。

`submit.php`
```cpp
<?php
require ("./judge.php"); //链接judge.php
$code = $_POST["code"];//代码
$pid = $_POST["pid"];//problem id

unlink("./submit.html");//删除submit.html
Result($code,$pid);//评测
unlink("./submit.php");//删除submit.php
header('location:' . dirname($_SERVER["PHP_SELF"]) . "/result.html");//跳转至result.html
?>
```
是不是精简多了？但是`judge.php`又变长了QAQ...

`judge.php`
```cpp
<?php
require ("../../library/prbinfo.php"); //获取时限
function Do_judge($time_limit){
  (略)
}

function judge($code,$path,$t_l){
  (略)
}

function Result($code,$pid){
	$fsub=fopen("result.html","w+") or exit("(略)"); //将网页输出至result.html
    (略)
    while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
        fwrite($fsub,"<h3>Test Case #" . (string)($curj) . ":</h3>");
        (略)
        if ($j_stat == 0){
            fwrite($fsub,"<h3 style=\"background-color:#4169E1\">TLE</h3>");
        }elseif ($j_stat == -1){
            fwrite($fsub,"<h3 style=\"background-color:#FF4500\">CE</h3>");
        }elseif ($j_stat == 1){
            (略)
            if ($file1 == $file2)
                fwrite($fsub,"<h3 style=\"background-color:#00FF00\">AC</h3>");
            else
                fwrite($fsub,"<h3 style=\"background-color:#FF4500\">WA</h3>");
        }
        $curj = $curj + 1;
    }
    
    fclose($fsub);
    unlink ("judge"); //删除judge
    unlink ("data.in"); //删除data.in
    unlink ("data.out"); //删除data.out
    unlink ("judge.php"); //删除judge.php
}

```

在`/library`目录下创建`filesys.php`作为文件系统库：
```cpp
<?php
function copydir($source, $dest) //复制目录
{
    if (!file_exists($dest)) mkdir($dest);
    $handle = opendir($source);
    while (($item = readdir($handle)) !== false) {
        if ($item == '.' || $item == '..') continue;
        $_source = $source . '/' . $item;
        $_dest = $dest . '/' . $item;
        if (is_file($_source)) copy($_source, $_dest);
        if (is_dir($_source)) copydir($_source, $_dest);
    }
    closedir($handle);
}

function rmdirs($path) //删除目录
{
    $handle = opendir($path);
    while (($item = readdir($handle)) !== false) {
        if ($item == '.' || $item == '..') continue;
        $_path = $path . '/' . $item;
        if (is_file($_path)) unlink($_path);
        if (is_dir($_path)) rmdirs($_path);
    }
    closedir($handle);
    return rmdir($path);
}

?>

```
在根目录下创建`submit.php`。
```cpp
<?php
require ("./library/filesys.php"); //链接库
$j_path = "submissions/judge" . md5(uniqid(microtime(true),true)); //新容器的目录
copydir("./judge-module",$j_path);把容器模型复制一份过去
header('location:' . $j_path . "/submit.html");跳转至新容器下的submit.html
```

这样,我们的容器部分就完成了。容器使用过后，只留下结果`result.html`和源代码，可以当评测机录用。
![](https://i.loli.net/2019/01/31/5c525b70bcdf8.png)

最后，我们进行一些整理。我们将`setperm.sh`和`unperm.sh`移至`/admin`下。至于里面的`dashboard.html/.php`，这是两个彻底的失败。在之后的版本会被删除的。

至此，我们已经有多个用户同时评测的能力了。

------------
### Day6：简单优化
Day6[**全部代码下载**](https://share.weiyun.com/598ROsu)

Day6最适合咕咕咕了！

删除一些注释，给界面画画表格然后收工!

### Day7：用户支持
Day7[**全部代码下载**](https://share.weiyun.com/5ybDQJF)

没有用户支持的话，这个OJ还只能算一个半成品。我们在第一周就要给它加上用户支持，这样就可以用剩下的时间来优化它了。

我们把用户的数据存到`user/database/{uid}.json`,举个例子:
```
{"name":"gugu","passwd":"123456","record":"[\"1001\"]","intr":"abc"}
```

不难看出，用户数据是由以下部分构成的`json`：

| 名称 | 意义 |
| :----------: | :----------: |
| **name** | **用户名** |
| **passwd** | **密码** |
| **record** | **提交记录** |
| **intr** | **个性签名** |

在PHP中处理`json`很简单，就像在Day4说的那样。
我们创建`library/libuser.php`
```cpp
<?php
define('WWWROOT',str_replace('\\','/',realpath(dirname(__FILE__).'/'))."/");
function get_user_info($uid){ //获取用户信息，返回信息数组
    //echo BASE_PATH . "problems/" . (string)($pid) . "/attrib.json";
    $file = fopen(WWWROOT . "../user/database/" . (string)($uid) . ".json","r");
    $attr = fgets($file);
    fclose($file);
    return json_decode($attr,true);
}

function set_user_info($uid,$pinfo){ //写入用户信息，写入信息数组
    //echo BASE_PATH . "problems/" . (string)($pid) . "/attrib.json";
    $file = fopen(WWWROOT . "../user/database/" . (string)($uid) . ".json","w+");
    fwrite($file,json_encode($pinfo));
    fclose($file);
}

function get_user_name($uid){ //获取用户名称
    $pinfo = get_user_info($uid);
    //echo $pinfo["name"];
    return $pinfo["name"];
}

function get_user_passwd($uid){ //获取用户密码
    $pinfo = get_user_info($uid);
    return $pinfo["passwd"];
}

function get_user_intr($uid){ //获取用户签名
    $pinfo = get_user_info($uid);
    return $pinfo["intr"];
}

function get_user_record($uid){ //获取用户记录
    $pinfo = get_user_info($uid);
    return json_decode($pinfo["record"],true);
}

function set_user_name($uid,$name){ //写入用户名称
    $pinfo = get_user_info($uid);
    $pinfo["name"] = $name;
    set_user_info($uid,$pinfo);
}

function set_user_passwd($uid,$passwd){ //写入用户密码
    $pinfo = get_user_info($uid);
    $pinfo["passwd"] = $passwd;
    set_user_info($uid,$pinfo);
}

function set_user_intr($uid,$intr){ //写入用户签名
    $pinfo = get_user_info($uid);
    $pinfo["intr"] = $intr;
    set_user_info($uid,$pinfo);
}

function set_user_record($uid,$record){ //写入用户记录
    $pinfo = get_user_info($uid);
    $arr = json_decode($pinfo["record"],true);
    array_push($arr,$record);
    $arr = array_unique($arr);
    $pinfo["record"] = json_encode($arr);
    set_user_info($uid,$pinfo);
}

function auth_user($uid,$passwd){ //验证用户信息是否正确
    if ($uid == 0) return false;
    if (get_user_passwd($uid) == $passwd)
        return true;
    else
        return false;
}

function create_user($name,$passwd){ //创建用户
    $cuid = 1; //当前用户id
    while (file_exists(WWWROOT . "../user/database/" . (string)($cuid) . ".json"))
        $cuid = $cuid + 1;
        
    $file = fopen(WWWROOT . "../user/database/" . (string)($cuid) . ".json","w+");
    $pinfo = array("name"=>$name,"passwd"=>$passwd,"record"=>"{}","intr"=>"This user is very lazy so that he didn't ...");
    fwrite($file,json_encode($pinfo));
    fclose($file);
    return $cuid;
}

function cookie_uid(){ //从cookie读取用户id
    if (!isset($_COOKIE["uid"])) return -1;
    else return $_COOKIE["uid"];
}

function cookie_passwd(){ //从cookie读取密码
    if (!isset($_COOKIE["uid"])) return -1;
    else return $_COOKIE["passwd"];
}

function auth_cookie(){ //验证cookie中的用户信息
    //echo cookie_uid() . '  ' . cookie_passwd();
    return auth_user(cookie_uid(),cookie_passwd());
    //echo $blk;
}

```
`user/show.php`:使用`user/show.php?uid={uid}`调用时，显示该id用户信息，否则显示当前用户信息。
```cpp
<?php
require ("../library/libuser.php");

echo "<html><head><title>User Infomation</title></head>";
if (isset($_GET["uid"])){ //有uid参数
    $uid = $_GET["uid"];
    echo "<h3>Hello,". get_user_name($uid)  ."</h3>";
    echo "<h3>Introduction:</h3><p>";
    echo get_user_intr($uid);
    
    echo "<br><h3>Submitting Record</h3>";
    foreach (get_user_record($uid) as $cur){
        echo $cur . "     ";
    }
}else if (auth_cookie()){  //已登录
    echo "<h3>Hello,". get_user_name(cookie_uid())  ."</h3>";
    echo "<h3>Introduction:</h3><p>";
    echo get_user_intr(cookie_uid());
    
    echo "<br><h3>Submitting Record</h3>";
    foreach (get_user_record(cookie_uid()) as $cur){
        echo $cur . "     ";
    }
}else{
    header('location:' . "login.html"); //跳转至登陆界面
}
```

还要添加`login.php/html`,`register.php/html`,修改`submit.html`，请自行阅读代码。

至此，oj已基本支持用户，但是还有一些小问题。比如...
![](https://i.loli.net/2019/02/12/5c62ae652994f.png)

额。。。Day8再修吧。

### Day8：用户密码加密 && 评测状态查询
Day8[**全部代码下载**](https://share.weiyun.com/5xpyC91)

首先我们解决Day7留下的Bug。在不改动apache配置的情况下，我们没有办法阻止查看`{uid}.json`,但是如果将密码加密，就算密文被看到也无法得出密码了。

PHP自带加密函数`crypt()`,我们采用`crypt($passwd,$passwd)`进行加密。

直接将`libuser.php`中所有的 `&ndsp;passwd` 全部改为 `crypt($passwd,$passwd)`即可。篇幅有限，请直接看代码。

![](https://i.loli.net/2019/02/14/5c65040f42265.png)
POJ有个著名的功能，叫做**“Online Status”**。作为一个现代化的OJ，我们也要实现这个功能。

思路：
```
在评测时创建一个辅助文件，它可能是"Unused","unaccepted","accepted"
若该文件是”unused“，表明未进行评测或评测错误。
若该文件是”unaccepted“，表明评测未通过。
若该文件是”accepted“，表明评测通过。

在status.php被调用时，扫描/submissions下的每一个评测记录，根据以上法则判断评测结果输出。
```

下面上`status.php`代码。
```
<?php
echo "<body><table class=\"table\"><caption>Status</caption>";
echo "<thead><tr><th>Run ID</th><th>Status</th><th>Detail</th></tr></thead><tbody>";
exec("ls -tr ./submissions",$stl);
//"<tr class=\"warning\"><td>All</td><td>CE</td></tr>"
foreach ($stl as $cur){
    //echo $cur . "<br>";
    if (file_exists("./submissions/" . $cur . "/unused"))
        echo "<tr class=\"warning\"><td>" . $cur . "</td><td>Submitting/Judgement Failed</td>" . "<td><a href=\"submissions/" . $cur . "/result.html\">More</a></td></tr>";
    elseif (file_exists("./submissions/" . $cur . "/accepted"))
        echo "<tr class=\"success\"><td>" . $cur . "</td><td>Accepted</td>" . "<td><a href=\"submissions/" . $cur . "/result.html\">More</a></td></tr>";
    elseif (file_exists("./submissions/" . $cur . "/unaccepted"))
        echo "<tr class=\"danger\"><td>" . $cur . "</td><td>Unaccepted</td>" . "<td><a href=\"submissions/" . $cur . "/result.html\">More</a></td></tr>";
    else
        echo "<tr class=\"active\"><td>" . $cur . "</td><td>Judging</td>" . "<td>Detail(unavailable)</td></tr>";
}

echo "</tbody></table></body></html>";

```

其他文件还需要修改，篇幅原因省略。


------------
到Day8为止，一个OJ的核心就完成了。仅仅`14.87KB`的代码，甚至比Win7自带的计算器(`758KB`)还要小！在完成核心之后，我们只需进行一些优化即可。

附：FreeOJ核心下载：[**这里**](https://share.weiyun.com/5xpyC91)


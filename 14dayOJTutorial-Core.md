����һ��֮ǰ���ձ������ֻ�û���˽������дOJ����ô~~��©����~~


------------


## ����
���ȣ���ƪ���½������һ����**��д**OJ��

Ҳ������ʡ�Ϊʲô���ǲ�ֱ������HustOJ/UniversalOJ�أ���

���������˽�һ�������ֳ�OJ��ȱ�㣺
- ��װ���ø��ӣ��������Ҫ��װmysql,��mysql��armhfƽ̨�м���������QAQ��
- ϵͳռ�ø� ����ݮ�ɺ�С��vps���Ѵ�����������ִ��Ч���ˣ�
- �������֣���������ͼ���ᵼ��һ���±ƣ�

Ϊʲô���� _���_ ֮���OJ�����Ŷ��أ�

~~��...�㲻�뿴����Ա��ɫ���Űɡ�~~

## ǰ��֪ʶ
- [PHP�����﷨](http://www.runoob.com/php/php-tutorial.html)
- [html �����﷨](http://www.runoob.com/html/html-tutorial.html)
- [linux shell�����﷨](http://www.runoob.com/linux/linux-shell.html)

## ʵ��ԭ��
![](https://i.loli.net/2018/12/30/5c283d6850a66.png)

��ž���������
## ��������ƽ̨

����Ŀ��Codeanywhere�Ͻ��б�д�����ԡ�

[Codeanywhere](codeanywhere.com),һ������PHPִ��ƽ̨���ٶȿ죬����ǿ��

![](https://cdn.luogu.org/upload/pic/47410.png)

��codeanywhereע��һ���˺ţ��½�һ��Project��

����Codeanywhere��ʹ��**����׸��**����Ϊ�����߶������һ������Ϊ���IDE����Ȥ��

���⣬��������Ҫʹ��[Codenvy](codenvy.io),��Ϊ�Ұ�CodeAnywhere���˺����������ˡ�
## ��д���� && �������
һ�²��������Ƶ�ע�ͣ�ֱ���Ķ�ע�ͼ��ɡ�

$\color{red}{\text{Day4���Ժ����Ŀÿ������ǰҪ��ִ��/setperm.sh �� admin/setperm.sh�޸�Ȩ�ޣ��м��мǣ�}}$
$\color{red}{\text{Day4���Ժ����Ŀÿ������ǰҪ��ִ��/setperm.sh �� admin/setperm.sh�޸�Ȩ�ޣ��м��мǣ�}}$
$\color{red}{\text{Day4���Ժ����Ŀÿ������ǰҪ��ִ��/setperm.sh �� admin/setperm.sh�޸�Ȩ�ޣ��м��мǣ�}}$
![](https://i.loli.net/2019/01/31/5c5271864358e.png)
### Day1����IDE
Day1[**ȫ����������**](https://share.weiyun.com/5T1hhQD)

���Ǵ����ⲿ��д��Day1��������ʵ��һ���򵥵�ide������[���IDE](https://www.luogu.org/app/ide_frame)��

�۲�ʵ��ԭ��ͼ�����Ƿ�����Ҫ����һ�µ��ļ���
- **submit.html** : **submit.php**��ǰ����ʾҳ��
- **submit.php** : ���ڴ���**judge.cpp**��**data.in**��������󷵻ؽ��
- **judge.sh** : ����**judge.cpp**�����в��ԣ����**data.out**


------------

���ȴ���һ���ļ��У�judge���½�һ���ű���judge.sh,���µ����ı༭����д��
```
# judge.sh ���ƶ��ĳ���
rm judge # ���������ɾ���ϴεĿ�ִ���ļ���
g++ judge.cpp -O2 -o judge # ����
./judge < data.in > data.out # ����
```
**�ǵ�**`chmod 755 judge.sh` 


------------

Ȼ����**submit.php**���ȿ�**submit.php**��α���룺

```
ɾ���ϴ�����ʱ����ʱ�ļ�
д��judge.cpp��data.in
����judge.sh����
��data.out�������ҳ
```

�����ϴ��롣
```
<?php

$code = $_POST["code"];// ��ȡ ����
$input = $_POST["input"];//��ȡ ����

unlink ("judge"); //ɾ�� judge����������
unlink ("data.in");
unlink ("data.out");
unlink ("judge.cpp");

$file = fopen("judge.cpp","w+") or exit("Unable to Open CPP."); //�� ���� д�� judge.cpp
fwrite($file,$code);
fclose($file);

$fin = fopen("data.in","w+") or exit("Unable to Open Input"); //�� ���� д�� data.in
fwrite($fin,$input);
fclose($fin);

system("./judge.sh");//ִ�� judge.sh

$fout = fopen("data.out","r") or exit("Unable to Open File."); // ��� data.out
while (!feof($fout)){
  echo fgets($fout);
}
fclose($fout);

?>
```


------------

Ȼ��**submit.html**�ͼ�����׶��ˡ�
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

### Day2����OJ
Day2[**ȫ����������**](https://share.weiyun.com/54JYlhO)

һ��OJֻ��IDE�ǲ��еģ�����Ҫ�������ݺ�����չʾ��
�������ݵİڷŷ�ʽ�������ģ�

![](https://i.loli.net/2018/12/30/5c283d68251ff.png)

��������Ҫ�޸�`/judge/submit.php`,����֧�ֶ������ݵ����⡣
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

$code = $_POST["code"]; //����
$pid = $_POST["pid"]; // ProblemID

$curj = 1;//��ʼ����ǰ���ݵ���
$p_fnt = "../problems/" . $pid . "/"; //��������Ŀ¼���·��

while (is_file($p_fnt . "data" . (string)($curj) . ".in")){//ö�����ݵ���
  //echo $p_fnt . "data" . (string)($curj) . ".in";
  judge($code,$p_fnt . "data" . (string)($curj) . ".in");//���⵱ǰ���Ե�
  
  $file1 = md5_file("./data.out"); // �����ύ����������MD5
  $file2 = md5_file($p_fnt . "data" . (string)($curj) . ".out");//���������ļ��������MD5
  
  if ($file1 == $file2) //�ȶ�
     echo "AC<br>";
  else
     echo "WA<br>";
  
  
  $curj = $curj + 1; //��һ�����Ե�
}

?>

```

------------

Ȼ���޸�`judge/submit.html`(��,��������)

�ռ�����һ������ݰ����˴���`A+B Problem`Ϊ��������ѹ��`problem/1001`��

��`problem/1001`���½������ļ�`cover.html`��д��
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

д����...����û�С�����`show.php`�ء�

show.php�ķ��ʸ�ʽΪ`show.php?pid=[��Ҫ���ʵ�ProblemID]`

�����ϴ��룺
```
<?php

$probid = $_GET["pid"]; //ProblemID
$cover = "./problems/" . $probid . "/cover.html"; //�������Ŀ¼

if (!is_file($cover)) exit("Bad Problem ID!");//ȷ���������

$file = fopen($cover,"r") or exit("Bad Cover!");

echo "<h3>Problem id:" . (string)$probid . "</h3>";//���PID

while (!feof($file)){
  echo fgets($file);//�����������
}

fclose($file);

echo "<a href=\"judge/submit.html\">Submit</a>"; //submit����

?>
```


------------

��Day2Ϊֹ��һ��������OJ�ͽ����ˡ���ӵ��**����չʾ**��**����**���ܡ�


------------
### Day3��С�Ż� && TLE���б�
Day3[**ȫ����������**](https://share.weiyun.com/53vaJHt)

�����Ż���һ�½����ʾҳ�������˲��Ե��ź���ɫ������������

һ����վû��`index.php`��ô���أ�
���Ǽ򵥵�дһ��`index.php`������������ʾ������Ŀ��

```
<?php
// /index.php
exec("ls ./problems",$prbs); // ��ȡ�����ļ�
$cur = [];
foreach ($prbs as $cur)
  echo "<a href=\"show.php?pid=" . $cur . "\">" . $cur . "</a>"; //������
?>
```

����˵������һ�ھ�д���ˣ�Ȼ������ʶ���������Ļ���û���б�TLE�ˡ��޷��б�TLE��ʹ��ͼ�������Ķ��������˲����ģ���������Ҫ����TLE���б�

���ʵ��TLE���б��أ�����ѡ��ʹ��linux�����timeout��
`Usage:timeout time command [return_val]`

���ˣ����˽��﷨�����Ǵ���һ��`/judge/judge.php`��Ȼ��ɾ��`/judge/judge.sh`����Ϊ���Ѿ����������ˡ�

������`/judge/judge.php`�Ĵ��룺
```
<?php
function Do_judge($time_limit){ //��������
  unlink ("judge");
  unlink ("data.out");
  system("g++ judge.cpp -O2 -o judge"); //����
  //echo "timeout " . (string)($time_limit) . " ./judge < data.in > data.out";
  $return_var = 0;
  system("timeout " . (string)($time_limit) . " ./judge < data.in > data.out",$return_var); //����
  if ($return_var == 124) return false; else return true;
}
?>
```

Ȼ���޸�`/judge/submit.php`

```
<?php
require ("judge.php"); //���

function judge($code,$path,$t_l){
  (��)
  return Do_judge($t_l); //��� true:ûTLE false:TLE

  //$fout = fopen("data.out","r") or exit("Unable to Open File.");
  //$ret = "";
  //while (!feof($fout)){
  //  $ret = $ret . fgets($fout);
    //echo $ret;
  //}
  //fclose($fout);
  //return $ret;
}
(��)

while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
  //echo $p_fnt . "data" . (string)($curj) . ".in";
  echo "<h3>Test Case #" . (string)($curj) . ":</h3>";
  if (!judge($code,$p_fnt . "data" . (string)($curj) . ".in",1/*ʱ��һ��*/))//���
    echo "<h3 style=\"background-color:#4169E1\">TLE</h3>";
  else{
  (��)
  }
(��)
}

?>
```

------------
### Day4����Ŀ������`attribute.json` && CE���ж�
Day4[**ȫ����������**](https://share.weiyun.com/5unEN5n)

AC...WA...TLE(RE)...����CE������ж�CE�أ������������û������ļ����ɡ�

���Ĵ������¡�
```
  ���ԣ�
  if (!file_exists("judge")){ ///û�����judge
    return -1;
  }
  ���ԣ�
```

�����޸�һ��`submit.php`����ʾ�𰸵Ĳ��֣������������е�`judge.php`��

```
<?php
���ԣ�

function judge($code,$path,$t_l){
���ԣ�
}
���ԣ�
while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
  echo "<h3>Test Case #" . (string)($curj) . ":</h3>";
  $j_stat = judge($code,$p_fnt . "data" . (string)($curj) . ".in",(int)(get_prb_time_limit($pid)));
  if ($j_stat == 0){
    echo "<h3 style=\"background-color:#4169E1\">TLE</h3>";
  }elseif ($j_stat == -1){
    echo "<h3 style=\"background-color:#FF4500\">CE</h3>";
  }elseif ($j_stat == 1){
	���ԣ�
  }
���ԣ�
}

?>

```
����һ��...�����б�CE�ˡ�


------------

���ǵĴ�����д������һ��:
`if (!judge($code,$p_fnt . "data" . (string)($curj) . ".in",1/*ʱ��һ��*/))`

���ǣ�ÿ���ʱ����ô���ܶ���һ���أ�����Ҫ��취���������⡣

PHP���Դ��ı������ݵķ�ʽ`json`,������Ȥ������ͨ��`json_encode`�������������顣
������`/library/prbinfo.php`��д��
```
<?php
//be called like 'require ("./library/prbinfo.php");'
function get_prb_info($pid){ //��ȡ��Ŀ��Ϣ
    $file = fopen("../problems/" . (string)($pid) . "/attrib.json","r");
    $attr = fgets($file);
    return json_decode($attr,true);
}

function get_prb_name($pid){//��ȡ��Ŀ����
    $pinfo = get_prb_info($pid);
    return $pinfo["name"];
}

function get_prb_time_limit($pid){//��ȡ��Ŀʱ��
    $pinfo = get_prb_info($pid);
    return $pinfo["time_limit"];
}
?>
```
Ȼ�����һ�иĳ�`$j_stat = judge($code,$p_fnt . "data" . (string)($curj) . ".in",(int)(get_prb_time_limit($pid)));`

����ļ�ҲҪ���������޸ģ��˴�����׸����

���ǻ�������`setperm.sh`��`unperm.sh`�����޸�Ȩ�ޡ��˴�����׸����

���ˣ������Ѿ����������ֱ���״̬��

------------
### Day5��������� && С�Ż�
Day5[**ȫ����������**](https://share.weiyun.com/5MRghBY)

����һ�������û������OJͬʱ�ύ�˴��룬ͻȻ...
![500](https://i.loli.net/2019/01/31/5c5251181170f.png)

Ϊʲô�أ�������`judge.cpp`�Ѿɵĸ����ˣ�

��������Ҫʵ��**�������**��������о��൱�ڲ���ϵͳ�еĶ����񣬰�ÿ�����ⶼ�ŵ�������**����**�����⣬�������š�

�����������ԭ�����¡�
```
��һ��Docker
�Ѵ���Ž�ȥ��
�رո�Docker
```
ʵ��������У���ʵ���ǰ�ԭ����`judge.php`,`submit.php`,`submit.html`�������������ļ��������Ƶ�һ��������ļ����ڽ������⡣�ļ��н�docker��������ռ����Դ���١�~~��ʵֻ��docker����~~

���ǰ�`judge`�ļ���������Ϊ`judge-module`����Ϊ�������ģ�塣
��`submit.php`�е����⺯���ᵽ`judge.php`�С�

`submit.php`
```cpp
<?php
require ("./judge.php"); //����judge.php
$code = $_POST["code"];//����
$pid = $_POST["pid"];//problem id

unlink("./submit.html");//ɾ��submit.html
Result($code,$pid);//����
unlink("./submit.php");//ɾ��submit.php
header('location:' . dirname($_SERVER["PHP_SELF"]) . "/result.html");//��ת��result.html
?>
```
�ǲ��Ǿ�����ˣ�����`judge.php`�ֱ䳤��QAQ...

`judge.php`
```cpp
<?php
require ("../../library/prbinfo.php"); //��ȡʱ��
function Do_judge($time_limit){
  (��)
}

function judge($code,$path,$t_l){
  (��)
}

function Result($code,$pid){
	$fsub=fopen("result.html","w+") or exit("(��)"); //����ҳ�����result.html
    (��)
    while (is_file($p_fnt . "data" . (string)($curj) . ".in")){
        fwrite($fsub,"<h3>Test Case #" . (string)($curj) . ":</h3>");
        (��)
        if ($j_stat == 0){
            fwrite($fsub,"<h3 style=\"background-color:#4169E1\">TLE</h3>");
        }elseif ($j_stat == -1){
            fwrite($fsub,"<h3 style=\"background-color:#FF4500\">CE</h3>");
        }elseif ($j_stat == 1){
            (��)
            if ($file1 == $file2)
                fwrite($fsub,"<h3 style=\"background-color:#00FF00\">AC</h3>");
            else
                fwrite($fsub,"<h3 style=\"background-color:#FF4500\">WA</h3>");
        }
        $curj = $curj + 1;
    }
    
    fclose($fsub);
    unlink ("judge"); //ɾ��judge
    unlink ("data.in"); //ɾ��data.in
    unlink ("data.out"); //ɾ��data.out
    unlink ("judge.php"); //ɾ��judge.php
}

```

��`/library`Ŀ¼�´���`filesys.php`��Ϊ�ļ�ϵͳ�⣺
```cpp
<?php
function copydir($source, $dest) //����Ŀ¼
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

function rmdirs($path) //ɾ��Ŀ¼
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
�ڸ�Ŀ¼�´���`submit.php`��
```cpp
<?php
require ("./library/filesys.php"); //���ӿ�
$j_path = "submissions/judge" . md5(uniqid(microtime(true),true)); //��������Ŀ¼
copydir("./judge-module",$j_path);������ģ�͸���һ�ݹ�ȥ
header('location:' . $j_path . "/submit.html");��ת���������µ�submit.html
```

����,���ǵ��������־�����ˡ�����ʹ�ù���ֻ���½��`result.html`��Դ���룬���Ե������¼�á�
![](https://i.loli.net/2019/01/31/5c525b70bcdf8.png)

������ǽ���һЩ�������ǽ�`setperm.sh`��`unperm.sh`����`/admin`�¡����������`dashboard.html/.php`�������������׵�ʧ�ܡ���֮��İ汾�ᱻɾ���ġ�

���ˣ������Ѿ��ж���û�ͬʱ����������ˡ�

------------
### Day6�����Ż�
Day6[**ȫ����������**](https://share.weiyun.com/598ROsu)

Day6���ʺϹ������ˣ�

ɾ��һЩע�ͣ������滭�����Ȼ���չ�!

### Day7���û�֧��
Day7[**ȫ����������**](https://share.weiyun.com/5ybDQJF)

û���û�֧�ֵĻ������OJ��ֻ����һ�����Ʒ�������ڵ�һ�ܾ�Ҫ���������û�֧�֣������Ϳ�����ʣ�µ�ʱ�����Ż����ˡ�

���ǰ��û������ݴ浽`user/database/{uid}.json`,�ٸ�����:
```
{"name":"gugu","passwd":"123456","record":"[\"1001\"]","intr":"abc"}
```

���ѿ������û������������²��ֹ��ɵ�`json`��

| ���� | ���� |
| :----------: | :----------: |
| **name** | **�û���** |
| **passwd** | **����** |
| **record** | **�ύ��¼** |
| **intr** | **����ǩ��** |

��PHP�д���`json`�ܼ򵥣�������Day4˵��������
���Ǵ���`library/libuser.php`
```cpp
<?php
define('WWWROOT',str_replace('\\','/',realpath(dirname(__FILE__).'/'))."/");
function get_user_info($uid){ //��ȡ�û���Ϣ��������Ϣ����
    //echo BASE_PATH . "problems/" . (string)($pid) . "/attrib.json";
    $file = fopen(WWWROOT . "../user/database/" . (string)($uid) . ".json","r");
    $attr = fgets($file);
    fclose($file);
    return json_decode($attr,true);
}

function set_user_info($uid,$pinfo){ //д���û���Ϣ��д����Ϣ����
    //echo BASE_PATH . "problems/" . (string)($pid) . "/attrib.json";
    $file = fopen(WWWROOT . "../user/database/" . (string)($uid) . ".json","w+");
    fwrite($file,json_encode($pinfo));
    fclose($file);
}

function get_user_name($uid){ //��ȡ�û�����
    $pinfo = get_user_info($uid);
    //echo $pinfo["name"];
    return $pinfo["name"];
}

function get_user_passwd($uid){ //��ȡ�û�����
    $pinfo = get_user_info($uid);
    return $pinfo["passwd"];
}

function get_user_intr($uid){ //��ȡ�û�ǩ��
    $pinfo = get_user_info($uid);
    return $pinfo["intr"];
}

function get_user_record($uid){ //��ȡ�û���¼
    $pinfo = get_user_info($uid);
    return json_decode($pinfo["record"],true);
}

function set_user_name($uid,$name){ //д���û�����
    $pinfo = get_user_info($uid);
    $pinfo["name"] = $name;
    set_user_info($uid,$pinfo);
}

function set_user_passwd($uid,$passwd){ //д���û�����
    $pinfo = get_user_info($uid);
    $pinfo["passwd"] = $passwd;
    set_user_info($uid,$pinfo);
}

function set_user_intr($uid,$intr){ //д���û�ǩ��
    $pinfo = get_user_info($uid);
    $pinfo["intr"] = $intr;
    set_user_info($uid,$pinfo);
}

function set_user_record($uid,$record){ //д���û���¼
    $pinfo = get_user_info($uid);
    $arr = json_decode($pinfo["record"],true);
    array_push($arr,$record);
    $arr = array_unique($arr);
    $pinfo["record"] = json_encode($arr);
    set_user_info($uid,$pinfo);
}

function auth_user($uid,$passwd){ //��֤�û���Ϣ�Ƿ���ȷ
    if ($uid == 0) return false;
    if (get_user_passwd($uid) == $passwd)
        return true;
    else
        return false;
}

function create_user($name,$passwd){ //�����û�
    $cuid = 1; //��ǰ�û�id
    while (file_exists(WWWROOT . "../user/database/" . (string)($cuid) . ".json"))
        $cuid = $cuid + 1;
        
    $file = fopen(WWWROOT . "../user/database/" . (string)($cuid) . ".json","w+");
    $pinfo = array("name"=>$name,"passwd"=>$passwd,"record"=>"{}","intr"=>"This user is very lazy so that he didn't ...");
    fwrite($file,json_encode($pinfo));
    fclose($file);
    return $cuid;
}

function cookie_uid(){ //��cookie��ȡ�û�id
    if (!isset($_COOKIE["uid"])) return -1;
    else return $_COOKIE["uid"];
}

function cookie_passwd(){ //��cookie��ȡ����
    if (!isset($_COOKIE["uid"])) return -1;
    else return $_COOKIE["passwd"];
}

function auth_cookie(){ //��֤cookie�е��û���Ϣ
    //echo cookie_uid() . '  ' . cookie_passwd();
    return auth_user(cookie_uid(),cookie_passwd());
    //echo $blk;
}

```
`user/show.php`:ʹ��`user/show.php?uid={uid}`����ʱ����ʾ��id�û���Ϣ��������ʾ��ǰ�û���Ϣ��
```cpp
<?php
require ("../library/libuser.php");

echo "<html><head><title>User Infomation</title></head>";
if (isset($_GET["uid"])){ //��uid����
    $uid = $_GET["uid"];
    echo "<h3>Hello,". get_user_name($uid)  ."</h3>";
    echo "<h3>Introduction:</h3><p>";
    echo get_user_intr($uid);
    
    echo "<br><h3>Submitting Record</h3>";
    foreach (get_user_record($uid) as $cur){
        echo $cur . "     ";
    }
}else if (auth_cookie()){  //�ѵ�¼
    echo "<h3>Hello,". get_user_name(cookie_uid())  ."</h3>";
    echo "<h3>Introduction:</h3><p>";
    echo get_user_intr(cookie_uid());
    
    echo "<br><h3>Submitting Record</h3>";
    foreach (get_user_record(cookie_uid()) as $cur){
        echo $cur . "     ";
    }
}else{
    header('location:' . "login.html"); //��ת����½����
}
```

��Ҫ���`login.php/html`,`register.php/html`,�޸�`submit.html`���������Ķ����롣

���ˣ�oj�ѻ���֧���û������ǻ���һЩС���⡣����...
![](https://i.loli.net/2019/02/12/5c62ae652994f.png)

�����Day8���ްɡ�

### Day8���û�������� && ����״̬��ѯ
Day8[**ȫ����������**](https://share.weiyun.com/5xpyC91)

�������ǽ��Day7���µ�Bug���ڲ��Ķ�apache���õ�����£�����û�а취��ֹ�鿴`{uid}.json`,���������������ܣ��������ı�����Ҳ�޷��ó������ˡ�

PHP�Դ����ܺ���`crypt()`,���ǲ���`crypt($passwd,$passwd)`���м��ܡ�

ֱ�ӽ�`libuser.php`�����е� `&ndsp;passwd` ȫ����Ϊ `crypt($passwd,$passwd)`���ɡ�ƪ�����ޣ���ֱ�ӿ����롣

![](https://i.loli.net/2019/02/14/5c65040f42265.png)
POJ�и������Ĺ��ܣ�����**��Online Status��**����Ϊһ���ִ�����OJ������ҲҪʵ��������ܡ�

˼·��
```
������ʱ����һ�������ļ�����������"Unused","unaccepted","accepted"
�����ļ��ǡ�unused��������δ����������������
�����ļ��ǡ�unaccepted������������δͨ����
�����ļ��ǡ�accepted������������ͨ����

��status.php������ʱ��ɨ��/submissions�µ�ÿһ�������¼���������Ϸ����ж������������
```

������`status.php`���롣
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

�����ļ�����Ҫ�޸ģ�ƪ��ԭ��ʡ�ԡ�


------------
��Day8Ϊֹ��һ��OJ�ĺ��ľ�����ˡ�����`14.87KB`�Ĵ��룬������Win7�Դ��ļ�����(`758KB`)��ҪС������ɺ���֮������ֻ�����һЩ�Ż����ɡ�

����FreeOJ�������أ�[**����**](https://share.weiyun.com/5xpyC91)


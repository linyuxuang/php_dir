# php_dir
php目录的操作(dir)

 目录的操作： 创建目录（有），删除目录，复制目录， 统计目录大小， 遍历 （自己定义函数）
 
  
     一、遍历目录：
 
      	   opendir()    打开目录
   	      readdir()    返回目录中下一个文件的文件名。
    	     closedir()     关闭目录
   	      rewinddir()   指定的目录流重置到目录的开头
 
 
          
      计算目录的长度           
          
          
          
          
                     is_dir — 判断给定文件名是否是一个目录
                      filectime — 取得文件的修改时间
                      filesize   获取文件的长度

          
                        $dirname="phpMyAdmin";

                        $dir=opendir($dirname);

                        while($fileName=readdir($dir)){
                          $file=$dirname.'/'.$fileName;
                          if($fileName!="." && $fileName!=".."){

                            if(is_dir($file)){
                                  //把所有的目录变成红色
                        echo "<font color='red'>".$fileName."-
                             --".date("Y-m-d H:i:s", filectime($file))."-
                             -".filetype($file)."---".toSize(dirsize($file))."---</font><br>";

                            }else{
                            //把所有的文件变成 绿色	
                              echo "<font color='green'>".$fileName."--
                              -".date("Y-m-d H:i:s", filectime($file))."--
                              --".filetype($file)."---
                              ---".toSize(filesize($file))."--</font><br>";
                            }
                          }

                        }

                          closedir($dir);   关闭目录

                       //  换算长度单位
                      function toSize($size){
                        $dw="Bytes";
                        if($size > pow(2, 30)){
                          $size=round($size/pow(2, 30), 2);
                          $dw="GB";
                        }else if($size > pow(2, 20)){
                          $size=round($size/pow(2, 20), 2);
                          $dw="MB";
                        }else if($size > pow(2, 10)){
                          $size=round($size/pow(2, 10), 2);
                          $dw="KB";
                        }else{ 
                          $dw="bytes";
                        }
                        return $size.$dw;
                      }
                      
                      
                  //   换算目录的长度
                    function dirsize($dirname) {
                      $dirsize=0;

                      $dir=opendir($dirname);

                      while($filename=readdir($dir)){
                        $file=$dirname."/".$filename;
                        if($filename!="." && $filename!=".."){
                          if(is_dir($file)){
                          //  如果是目录就用递归函数（把目录传递到上面函数，
                            说白了这里把拿到的目录有当做上面函数的参数使用（有一层层往下执行））	
                            $dirsize+=dirsize($file); //递归完成	
                          }else{
                            $dirsize+=filesize($file);
                          }
                        }
                      }
                 closedir($dir); 关闭目录

              return $dirsize;

            }

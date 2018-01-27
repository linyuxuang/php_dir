# php_dir
php目录的操作(dir)

 目录的操作： 创建目录（有），删除目录，复制目录， 统计目录大小， 遍历 （自己定义函数）
 
  
     一、遍历目录：
 
      	   opendir()    打开目录
   	      readdir()    返回目录中下一个文件的文件名。
    	     closedir()     关闭目录
   	      rewinddir()   指定的目录流重置到目录的开头
 
 
 
 	    移动或重命名函数

	 
                   		rename('c:/bbbccc', 'phpMyAdmin');  //和文件操作一样
 
 
          
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
            
            
       
       
  拷贝目录
  
  
              
              
                          $dirname="phpMy";    
                         copydir($dirname,"linyuxuan");

                       function copydir($dirsrc,$dirto){
                        // 若是文件就不返回	
                         //	is_file — 判断给定文件名是否为一个正常的文件
                        if(is_file($dirto)){
                         echo "文件不是目录不能创建"."<br>";
                         return;
                        }
                        //检查目录不存在就新建一个目录
                         //	file_exists — 检查文件或目录是否存在
                        if(!file_exists($dirto)){
                         mkdir($dirto);
                         echo "目录创建成功"."<br>";
                        }
                        //打开这个目录
                        //opendir — 打开目录
                        $dir=opendir($dirsrc);

                        //  打开这个目录每一个下一个文件名
                         //readdir() 函数返回目录中下一个文件的文件名
                        while($filename=readdir($dir)){

                         if($filename!="."&& $filename!=".."){
                         //拼接目录层级关系
                         // 要被复制的目录
                         $file1=$dirsrc."/".$filename;	

                            //新复制的目录
                         $file2=$dirto."/".$filename;	

                          //判断是否 是一个目录		
                          //is_dir — 判断给定文件名是否是一个目录
                          if(is_dir($file1)){
                          copydir($file1,$file2);   //递归处理	
                           }else{
                           copy($file1,$file2);	
                           }		
                         }
                       }
                      closedir($dir);   //closedir — 关闭目录
                    }
       



 删除目录
 
 
 
 
                       $dirname="linyuxuan";    
       
                       deldir($dirname);  
                       function deldir($dirname){

                           // 若目录或文件存在
                           //file_exists — 检查文件或目录是否存在
                           if(file_exists($dirname)){
                               $dir=opendir($dirname);

                               while($filename=readdir($dir)){
                                   if($filename!="." && $filename!=".."){
                                       $file1=$dirname."/".$filename;

                                        //如果是一个目录就删除
                                        //is_dir — 判断给定文件名是否是一个目录
                                       if(is_dir($file1)){
                                          deldir($file1);  // 使用递归删除子目录
                                          echo "删除目录成功<br>";
                                       }else{
                                        //unlink — 删除文件
                                        unlink($file1);
                                        echo "删除文件成功<br>";      	   	   	   	   	
                                       }
                                   }
                               }
                             closedir($dir);
                            echo '删除目录<b>'.$dirname.'</b>成功<br>';
                            //rmdir — 删除目录
                            rmdir($dirname);
                           }

                       } 

 
 
 
 
 


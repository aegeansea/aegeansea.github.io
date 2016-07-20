---
title: tp3.2.3整合PHPexcel实现数据导入导出
date: 2016-07-20 13:39:17
tags: [tp3.2.3, PHPexcel]
---

### 一. 引入
PHPExcel文件夹和PHPExcel.class.php放入thinkphp的Library/Org/Util/目录下。
<!-- more -->
### 二. 导出
```php
    /**
    *   方法名:    getExcel
    *   作用  :将数据导出excel表格
    *   @date   2015/03/26
    *   @author dingling
    *   @param1 文件名
    *   @param2 字段名
    *   @param3 数据
    *   @return excel文件
    */
    function getExcel($fileName, $headArr, $data){
        //对数据进行检验
        if(empty($data) || !is_array($data)){
            die("数据必须为数组");
        }
        //检查文件名
        if(empty($fileName)){
            exit;
        }
        //组装文件名
        $date = date("Y_m_d_H_i_s",time());
        $fileName .= "_{$date}.xls";

        error_reporting(E_ALL);
        ini_set('display_errors', TRUE);
        ini_set('display_startup_errors', TRUE);
        date_default_timezone_set('PRC');

        if (PHP_SAPI == 'cli') {
          die('只能通过浏览器运行'); 
        }

        //导入PHPExcel类库，因为PHPExcel没有用命名空间，只能inport导入
        import("Org.Util.PHPExcel");
        import("Org.Util.PHPExcel.Writer.Excel5");
        import("Org.Util.PHPExcel.IOFactory.php");

        //创建PHPExcel对象，注意，不能少了\
        $objPHPExcel = new \PHPExcel();
        $objProps = $objPHPExcel->getProperties();

        //设置表头
        $key = ord("A");//A--65
        $key2 = ord("@");//@--64
        foreach($headArr as $v){
            if($key>ord("Z")){
                $key2 += 1;
                $key = ord("A");
                $colum = chr($key2).chr($key);//超过26个字母时才会启用  dingling 20150626
            }else{
                if($key2>=ord("A")){
                    $colum = chr($key2).chr($key);
                }else{
                    $colum = chr($key);
                }
            }
            $objPHPExcel->setActiveSheetIndex(0) ->setCellValue($colum.'1', $v);
            $key += 1;
        }

        $column = 2;
        $objActSheet = $objPHPExcel->getActiveSheet();

        foreach($data as $key => $rows){ //行写入
            $span = ord("A");
            $span2 = ord("@");
            foreach($headArr as $k=>$v){
                if($span>ord("Z")){
                    $span2 += 1;
                    $span = ord("A");
                    $j = chr($span2).chr($span);//超过26个字母时才会启用  dingling 20150626
                }else{
                    if($span2>=ord("A")){
                        $j = chr($span2).chr($span);
                    }else{
                        $j = chr($span);
                    }
                }
                //$j = chr($span);
                $objActSheet->setCellValue($j.$column, strip_tags($rows[$k]));
                $span++;
            }
            $column++;
        }

        $fileName = iconv("utf-8", "gb2312", $fileName);

        // $objPHPExcel->getActiveSheet()->setTitle('CNLINK');// 重命名表
        $objPHPExcel->setActiveSheetIndex(0);// 设置活动单指数到第一个表,所以Excel打开这是第一个表


        // Redirect output to a client’s web browser (Excel5)
        header('Content-Type: application/vnd.ms-excel');
        header("Content-Disposition: attachment;filename=\"$fileName\"");
        header('Cache-Control: max-age=0');

        // If you're serving to IE 9, then the following may be needed
        header('Cache-Control: max-age=1');
        // If you're serving to IE over SSL, then the following may be needed
        header ('Expires: Mon, 26 Jul 1997 05:00:00 GMT'); // Date in the past
        header ('Last-Modified: '.gmdate('D, d M Y H:i:s').' GMT'); // always modified
        header ('Cache-Control: cache, must-revalidate'); // HTTP/1.1
        header ('Pragma: public'); // HTTP/1.0

        $objWriter = \PHPExcel_IOFactory::createWriter($objPHPExcel, 'Excel5');
        // $objWriter->save('1.xls');
        // echo str_replace('.php', '.xls', __FILE__);
        $objWriter->save('php://output'); //文件通过浏览器下载

        exit;
    }
```

### 三. 导入
```php
    //导入数据方法
    function import($filename, $exts='xls') {
        //导入PHPExcel类库，因为PHPExcel没有用命名空间，只能inport导入
        import("Org.Util.PHPExcel");
        //创建PHPExcel对象，注意，不能少了\
        $PHPExcel=new \PHPExcel();
        //如果excel文件后缀名为.xls，导入这个类
        if($exts == 'xls'){
            import("Org.Util.PHPExcel.Reader.Excel5");
            $PHPReader=new \PHPExcel_Reader_Excel5();
        }else if($exts == 'xlsx'){
            import("Org.Util.PHPExcel.Reader.Excel2007");
            $PHPReader=new \PHPExcel_Reader_Excel2007();
        }


        //载入文件
        $PHPExcel=$PHPReader->load($filename);
        //获取表中的第一个工作表，如果要获取第二个，把0改为1，依次类推
        $currentSheet=$PHPExcel->getSheet(0);
        //获取总列数
        $allColumn=$currentSheet->getHighestColumn();
        //获取总行数
        $allRow=$currentSheet->getHighestRow();
        //循环获取表中的数据，$currentRow表示当前行，从哪行开始读取数据，索引值从0开始
        for($currentRow=1;$currentRow<=$allRow;$currentRow++){
            //从哪列开始，A表示第一列
            for($currentColumn='A';$currentColumn<=$allColumn;$currentColumn++){
                //数据坐标
                $address=$currentColumn.$currentRow;
                //读取到的数据，保存到数组$arr中
                $cell =$currentSheet->getCell($address)->getValue();
                //$cell = $data[$currentRow][$currentColumn];
                if($cell instanceof PHPExcel_RichText){
                    $cell  = $cell->__toString();
                }
                print_r($cell);
            }
        }
    }
```

### 四. 问题
在excel中如果在一个默认的格中输入或复制超长数字字符串，它会显示为科学计算法，例如身份证号码，解决方法是把表格设置文本格式或在输入前加一个单引号。

使用PHPExcel来生成excel，也会遇到同样的问题，解决方法有三种：
1、设置单元格为文本
```php
$objPHPExcel = new PHPExcel();
$objPHPExcel->setActiveSheetIndex(0);
$objPHPExcel->getActiveSheet()->setTitle('Simple');
//设置A3单元格为文本
$objPHPExcel->getActiveSheet()->getStyle('A3')->getNumberFormat()
    ->setFormatCode(PHPExcel_Style_NumberFormat::FORMAT_TEXT);
//也可以设置整行或整列的style
/*
//E 列为文本
$objPHPExcel->getActiveSheet()->getStyle('E')->getNumberFormat()
    ->setFormatCode(PHPExcel_Style_NumberFormat::FORMAT_TEXT);
//第三行为文本
$objPHPExcel->getActiveSheet()->getStyle('3')->getNumberFormat()
    ->setFormatCode(PHPExcel_Style_NumberFormat::FORMAT_TEXT);
*/
```
更多的格式可以在PHPExcel/Style/NumberFormat.php中找到。注意：上述的设置对长数字字符串还是以文本方式来显示科学计数法的结果，原因可能php在处理大数字时采用的科学计数法
2、在设置值的时候显示的指定数据类型
```php
$objPHPExcel = new PHPExcel();
$objPHPExcel->setActiveSheetIndex(0);
$objPHPExcel->getActiveSheet()->setTitle('Simple');

$objPHPExcel->getActiveSheet()->setCellValueExplicit('D1',
                                 123456789033, 
                                 PHPExcel_Cell_DataType::TYPE_STRING);
```
3、在数字字符串前加一个空格使之成为字符串
```php
$objPHPExcel = new PHPExcel();
$objPHPExcel->setActiveSheetIndex(0);
$objPHPExcel->getActiveSheet()->setTitle('Simple');

$objPHPExcel->getActiveSheet()->setCellValue('D1', ' ' . 123456789033);
```
推荐使用第二、三种，第一种没有根本解决问题。

### 参考
[THINKPHP3.2.3整合PHPexcel实现数据的导入导出（含数据库）](http://www.gouguoyin.cn/php/4.html)
[PHPExcel 长数字串显示为科学计数的解决方法](http://www.gouguoyin.cn/php/83.html)
[百度云参考代码](http://pan.baidu.com/s/1pLs31wv "THINKPHP3.2.3整合PHPexcel实现数据的导入导出.7z") qqia

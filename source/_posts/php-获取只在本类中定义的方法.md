---
title: php 获取只在本类中定义的方法
date: 2016-05-06 06:37:01
tags:
    - php
    - 调试
---
### 注意
实例化不要在父类的初始化构造方法中进行，会导致死循环。
### 测试代码
``` php
    /**
     * 获取类中非继承方法和重写方法
     * 只获取在本类中声明的方法，包含重写的父类方法，其他继承自父类但未重写的，不获取
     * 例
     * class A{
     *      public function a1(){}
     *      public function a2(){}
     * }
     * class B extends A{
     *      public function b1(){}
     *      public function a1(){}
     * }
     * getMethods('B')返回方法名b1和a1，a2虽然被B继承了，但未重写，故不返回
     * @param string $classname 类名
     * @param string $access public or protected  or private or final 方法的访问权限
     * @return array(methodname=>access)  or array(methodname) 返回数组，如果第二个参数有效，
         * 则返回以方法名为key，访问权限为value的数组
     * @see  使用了命名空间，故在new 时类前加反斜线；如果此此函数不是作为类中方法使用，可能由于权限问题，
         *   只能获得public方法
     */
    public function getMethods($classname,$access=null){
        $class = new \ReflectionClass($classname);
        $methods = $class->getMethods();
        $returnArr = array();
        foreach($methods as $value){
            if($value->class == $classname){
                if($access != null){
                    $methodAccess = new \ReflectionMethod($classname,$value->name);
                    switch($access){
                        case 'public':
                            if($methodAccess->isPublic())$returnArr[$value->name] = 'public';
                            break;
                        case 'protected':
                            if($methodAccess->isProtected())$returnArr[$value->name] = 'protected';
                            break;
                        case 'private':
                            if($methodAccess->isPrivate())$returnArr[$value->name] = 'private';
                            break;
                        case 'final':
                            if($methodAccess->isFinal())$returnArr[$value->name] = 'final';
                            break;
                    }
                }else{
                    array_push($returnArr,$value->name);
                }
                 
            }
        }
        return $returnArr;
    }
```
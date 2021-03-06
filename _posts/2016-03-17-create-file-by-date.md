---
layout: post
title:  批处理以当前时间为文件名创建指定格式文件
date:   2016-03-17 09:00:13
categories: Tools
---
批处理以当前时间为文件名创建指定格式文件

## Dos查看日期加时间的方法

``` 
echo %date% %time%
```

output:

2016/02/25 周四 22:49:36.08

格式化一下：

``` 
echo %date:~0,4%%date:~5,2%%date:~8,2%%time:~0,2%%time:~3,2%%time:~6,2%
```

output:

20160225225012

## 新建文件：

新建任意文件名.任意文件类型的空文件(精确到秒)：

``` 
echo a 2>FileName.type
```

新建以当前时间为文件名的空txt文件：

``` 
echo a 2>%date:~0,4%%date:~5,2%%date:~8,2%%time:~0,2%%time:~3,2%%time:~6,2%.txt
```

以"_"间隔：

``` 
echo a 2>%date:~0,4%_%date:~5,2%_%date:~8,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.txt
```

## 把小时数小于10的单个数字转换为两个数字

``` 
set    CurrentDate=%date:~0,10%
set    CurrentYear=%date:~0,4%
set    CurrentMonth=%date:~5,2%
set    CurrentDay=%date:~8,2%
set    CurrentTime=%time%
set    CurrentHour=%CurrentTime:~0,2%
if    /i %CurrentHour% LSS 10 (set CurrentHour=0%CurrentTime:~1,1%)
set    CurrentMinute=%time:~3,2%
set    CurrentSecond=%CurrentTime:~6,2%
set    CurrentDateTime=%CurrentYear%_%CurrentMonth%_%CurrentDay%_%CurrentHour%_%CurrentMinute%_%CurrentSecond%
echo    %CurrentDateTime%
echo a 2>%CurrentDateTime%.txt
```

## 使用PowerShell

获取时间：

``` 
Get-Date
```

output：

2016年2月25日 23:31:57

输出时间：

``` 
 Write-Host "$(Get-Date), hello!"
```

output:

02/25/2016 23:34:06, hello!

格式化输出时间：

``` 
yyyy    年
M    月
d    日
h    小时（12小时制）
H    小时（24小时制）
m    分钟
s    秒
```

``` 
 Get-Date -Format 'yyyy_MM_dd_HH_mm_ss'
```

output:

``` 
2016_02_25_23_37_56
```

新建文件：

``` 
 New-Item FileName  -ItemType f
```

-ItemType ： f(file)表示文件，d(directory)表示目录

-ItemType可以简写为：Type

新建以时间命名的文件：

``` 
New-Item -ItemType file "$((Get-Date).ToString('yyyy_MM_dd_HH_mm')).txt"
```

指定目录名+文件名，在任意目录创建文件：

``` 
New-Item -ItemType file "./$((Get-Date).ToString('yyyy_MM_dd_HH_mm')).txt"
```

新建以时间命名的目录：

``` 
New-Item -ItemType Directory -Path ".\$((Get-Date).ToShortDateString())"
```

目录结构：./yyyy/MM/dd/

或者：

``` 
New-Item -ItemType Directory -Path ".\$((Get-Date).ToString('yyyy-MM-dd'))"
```

此时的目录结构：./yyyy-MM-dd/

END


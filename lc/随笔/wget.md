wget -page-requisites-recursive http://www.17sucai.com/preview/949344/2018-03-19/LoginHTML/demo.html
 wget -m -e robots=off -k -E "http://www.17sucai.com/preview/949344/2018-03-19/LoginHTML/demo.html"
 wget -r -np -k -p http://doc.scrapy.org/en/latest/index.html
 
 -c：断点续传 
-r：递归下载 
-np：递归下载时不搜索上层目录 
-nd：递归下载时不创建一层一层的目录,把所有文件下载当前文件夹中 
-p：下载网页所需要的所有文件(图片,样式,js文件等) 
-H：当递归时是转到外部主机下载图片或链接 
-k：将绝对链接转换为相对链接,这样就可以在本地脱机浏览网页了


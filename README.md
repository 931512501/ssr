cd shadowsocksr           //解压后进入目录

sh setup_cymysql2.sh     //安装

sh initcfg.sh
python server.py        //运行

数据库配置文件：usermysql.json
后端配置文件：user-config.json


WEB端配置

PHP7.1
安装扩展，选择安装 fileinfo 
禁用函数，将 proc_open / proc_get_status 两个函数删除
完成以上操作，记得 php服务重启 一下，才能生效
回到 SSH ，进入 SSRPanel 的目录
php composer.phar update

php composer.phar install

php artisan key:generate
为 storage目录设置权限：

chown -R www:www storage/

chmod -R 777 storage/
运行目录为 /public 
设置伪静态，粘贴以下代码并保存。

location / {

try_files $uri $uri/ /index.php$is_args$args;

}


SSRPanel 定时任务
在 /home 下创建一个 www目录，并将其权限设置为 777  www 
设置定时任务在 SSH 下执行命令：
sudo -i
cd ~
crontab -e -u www
粘贴并保存：* * * * * php /www/wwwroot/你的域名/ssrpanel/artisan schedule:run >> /dev/null 2>&1（此处的路径为本机 SSRPanel 所在目录，必须保证正确。）
查看运行状态

使用该命令查看运行日志：

tail -f /var/log/cron

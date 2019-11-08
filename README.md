
一个简单的基于docker的laravel开发环境，内置nginx、mysql5.7、php7.2

## 启动
```shell script
docker-compose up -d
```

## 停止
```shell script
docker-compose down
```

## 进入容器执行php命令
```shell script
docker-compose exec workspace zsh
```

## 重建镜像
```shell script
docker-compose build
```

## 新增nginx虚拟站点
新增站点配置文件
```shell script
cp services/nginx/default.conf services/nginx/sites/new_site.conf # new_site.conf根据你新增的网站命名
```
根据需要修改新增配置文件的内容
```shell script
docker-compose restart # 重启所有容器
```
__* 以上命令均在项目根目录执行__

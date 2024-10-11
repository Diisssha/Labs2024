
установка сканирующей штуки 
```
wget -O docker-scout.tar https://github.com/docker/scout-cli/releases/download/v1.4.1/docker-scout_1.4.1_linux_amd64.tar.gz
```
разархивируем 
```
tar -xvf docker-scout.tar
```
создаем коталог для плагинов 
```
mkdir -p ~/.docker/cli-plugins
mv docker-scout ~/.docker/cli-plugins/
```
2. Использование основных команд Docker Scout
```
docker scout quickview
```

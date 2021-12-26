FROM debian
MAINTAINER polina



#    Найти где находится директория sources.list.d (К)
#    Зайти на сайт https://www.debian.org/mirror/list и выбрать любое понравившееся зеркало. (Х)
#    Записать зеркало в файл с названием <REPO_NAME>.list, где repo_name заменить на имя репозитория. Зеркало должно поддерживать скачивание main и non free пакетов. (Х)
#    Выполнить обновление apt-кеша (Д)
#    Обновить все пакеты в контейнере (Д)
RUN cd /etc/apt/sources.list.d/ && echo  "deb http://ftp.us.debian.org/debian/ stretch main contrib non-free \ndeb-src http://ftp.us.debian.org/debian/ stretch main contrib non-free" >> ftp.us.debian.org.list && apt-get update && apt-get upgrade
#    Установить веб сервер nginx (Д)
#    Очистить скачанный apt-cache (Д)
RUN apt-get install -y nginx && apt-get clean && apt-get autoclean
#    Удалить содержимое директории /var/www/ (Д)
RUN rm -r /var/www/ && mkdir /var/www/ && \
#    Создать в директории /var/www/ директорию с именем вашего сайта и папку с картинками (company.com/img) (Д)
	cd /var/www/ && mkdir JustSayHomomorphism && cd ./JustSayHomomorphism && mkdir img
#    Поместить из папки с докер файлом в директорию /var/www/company.com файл index.html. Файл нужно создать рядом с Dockerfile и поместить туда какую-нибудь строку приветствия (Х,Д)
COPY index.html /var/www/JustSayHomomorphism/
#    Поместить из папки с докер файлом в директорию /var/www/company.com/img файл img.jpg (поместить туда какую-нибудь картинку). (Х,Д)
COPY img /var/www/JustSayHomomorphism/img
#    Задать рекурсивно на папку /var/www/company.com права "владельцу - читать, писать, исполнять; группе - читать, исполнять, остальным - только читать" (Д)
RUN chmod -R 754 /var/www/JustSayHomomorphism 
#    С помощью команды useradd создать пользователя <ваше имя> (Д)
RUN useradd polina && \
#   С помощью команды groupadd создать группу <ваша фамилия> (Д)
	groupadd solomentseva && \
#   С помощью команды usermod поместить пользователя <ваше имя> в группу <ваша фамилия> (Д)
	usermod -a -G solomentseva polina && \
#    Рекурсивно присвоить созданных пользователя и группу на папку /var/www/company.com (Д)
	chown -R polina:solomentseva /var/www/JustSayHomomorphism
#    Воспользоваться конструкцией (sed -i 's/ЧТО ЗАМЕНИТЬ/НА ЧТО ЗАМЕНИТЬ/g' имя_файла) и заменить в файле /etc/nginx/sites-enabled/default следующую подстроку (/var/www/html) так, чтобы она соответствовала (/var/www/company.com) (Д)
RUN sed -i 's|/var/www/html|/var/www/JustSayHomomorphism|g' /etc/nginx/sites-enabled/default && \
#    С помощью команды grep найти в каком файле задается пользователь (user), от которого запускается nginx (К)
	nginxConf=$(grep -iRl "user www-data" /etc/) && \
#    С помощью команды sed проделать операцию замены пользователя в файле найденном в пункте 17 на вашего пользователя (Д)
	sed -i 's|user www-data|user polina|g' $nginxConf


CMD ["nginx", "-g", "daemon off;"]

#    Соберите ваш контейнер: docker build -t test .
#    Запустить собранный контейнер и проверить, что он запустился от вашего пользователя. Для этого необходимо выполнить Docker exec -ti <container_id> bash в соседеней консоли, предварительно узнав идентификатор запущенного контейнера. Назовите все идентификаторы процессов для родительского и порожденного процессов. (Выполнять после успешной сборки) (Х)
 #   Запустить ваш контейнер, но на этот раз передав аргументами нужные порты: docker run -ti -p 80:80 test
 #   Откройте в браузере(или curl) 127.0.0.1:80. Вы должны получить вашу приветственную страницу. Сделайте скриншот страницы в браузере на фоне команды запуска контейнера. #Скриншот положить в репозиторий (Х)
 #   Выполните правильную постановку тега и загрузите ваш контейнер в docker hub. (Х)

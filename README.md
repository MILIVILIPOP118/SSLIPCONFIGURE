1. Создаем директорию для хранения файлов.

  mkdir ssl
  mkdir ssl/private

2. Переходи в директорию

  cd ssl # Eсли при входе вы не изменяли директорию, то путь будет выглядить "/home/<имя_пользователя>/ssl/" или "/root/ssl/".

3. Генерируем самоподписанный корневой SSL сертификат, используя утилиту OpenSSL:

  openssl req -x509 -days 365 -newkey rsa:2048 -nodes -keyout /private/key.pem -out cert.pem

4. Выпускаем сертификат, при запросах CN/Common Name/host и подобных, пишем IP CentOS:

  openssl genrsa -out domain.key 2048 
  openssl req -new -key domain.key -out domain.csr 
  openssl x509 -req -in domain.csr -CA cert.pem -CAkey /private/key.pem -CAcreateserial -out domain.crt -days 365

5. Изменяем настройки SSL в конфигурационный файл:
  5.1 Изменяем имя хоста:
    server {
    ...
    listen 443 ssl; 
    server_name <IP_CentOS>;
    ...

  5.2 Изменяем путь до файлов сертификатов:
    ssl_certificate /<путь_из_пункта_2>/domain.crt; 
    ssl_certificate_key /<путь_из_пункта_2>/domain.key;

Нужно установить Linux Oracle на VirtualBox

◦ Образ Linux

◦ Выделить 4 ЦП

◦ Выделить 4096+ МБ

◦ При установке ОС выбрать английский язык

Установка docker с использованием grafana:

① `sudo yum install wget`

▹ УтилитА wget

![image](https://github.com/user-attachments/assets/4a10e5dd-6f83-4270-8c8b-6b8c3c7751ce)

② `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

▹ Файл репозитория

![image](https://github.com/user-attachments/assets/62a98b7f-769f-4a1d-9dd1-ba8b2e5f9c5b)

③ `sudo yum install docker-ce docker-ce-cli containerd.io`

▹ Устанавливаем docker

![image](https://github.com/user-attachments/assets/eb9e7dfa-2bc2-4cea-84b3-c93d00db7db2)

④ `sudo systemctl enable docker --now`

▹ Запускаем его и разрешаем автозапуск

⑤ `sudo yum install curl`

▹ Убедимся в наличие пакета curl

⑥ `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

▹ Объявление переменной COMVER

![image](https://github.com/user-attachments/assets/a71b5f57-d895-4e74-9c01-aa11dacc0bd2)

⑦ `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`                        

▹ Скачиваем скрипт docker-compose последней версии и помещаем его в каталог /usr/bin

![image](https://github.com/user-attachments/assets/ff9aa1f5-5964-4027-909c-c49adb8aab4d)

⑧ `sudo chmod +x /usr/bin/docker-compose`

▹ Предоставление прав на выполнение файла docker-compose.

⑨ `docker-compose --version`

▹ Проверка установленной версии Docker Compose.

![image](https://github.com/user-attachments/assets/658f7f17-eff6-4c1f-a8db-b5d9d73470d1)

▹ Можно скачать git прямо из командной строки прописав Y

⑩ `git clone https://github.com/skl256/grafana_stack_for_docker.git`

▹ Выдаст ошибку и предложит скачать git, согласиться и продолжить

![image](https://github.com/user-attachments/assets/ad0d94ee-41f7-490b-8560-9ffbaaa16d5b)

①① `cd grafana_stack_for_docker`
    
▹ Переходим в папку

①② `sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

▹ Команда создаёт полный путь /mnt/common_volume/swarm/grafana/config

①③ `sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`

▹ Команда создаёт структуру каталогов для Grafana и связанных с ней компонентов

①④ `sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

▹ Все файлы и каталоги будут переданы в собственность текущему пользователю и его группе

①⑤ ` touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

▹ Файл grafana.ini уже существует, команда обновит временные метки. Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути

①⑥ `cp config/* /mnt/common_volume/swarm/grafana/config/`

▹ Команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

①⑦ `mv grafana.yaml docker-compose.yaml `

▹ Команда переименовывает файл grafana.yaml в docker-compose.yaml

①⑧ `sudo docker compose up -d`

▹ Команда создает и запускает контейнеры в фоновом режиме с правами суперпользователя.

![image](https://github.com/user-attachments/assets/b5a2d557-f83d-48c6-946b-8cb0330de817)

![image](https://github.com/user-attachments/assets/07b2fed6-6853-46e3-aaa2-13374f9507ef)

①⑨ `sudo vi docker-compose.yaml`

▹ Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет редактировать его содержимое.

![image](https://github.com/user-attachments/assets/bbeb8593-9dd2-44a0-9169-e7907f190f06)

![image](https://github.com/user-attachments/assets/ea435537-9cf0-4a7b-a258-f97692d51086)

②⓪ `sudo vi prometheus.yaml`

▹ Команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

▹ /mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100,

![image](https://github.com/user-attachments/assets/9779ee9d-8d83-4159-8835-1436fdecb767)

![image](https://github.com/user-attachments/assets/c160b968-d1b4-4fd2-9d60-889ff1e3b877)

GRAFANA

Переходим на сайт `localhost:3000`
  * User & Password GRAFANA: `admin`
  * Код графаны: `3000`
  * Код прометеуса: `http://prometheus:9090`

В меню выбираем вкладку Dashboards и создаем Dashboard
  * Жмем кнопку +Add visualization, а после "Configure a new data source"
  * Выбираем Prometheus
  * Connection
  * `http://prometheus:9090`

Authentication
  * Basic authentication
  * User: `admin`
  * Password: `admin`
  * Нажимаем на Save & test и должно показывать зелёную галочку

В меню выбираем вкладку Dashboards и создаем Dashboard
  * Жмем кнопку "Import dashboard"
  * Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load
  * Select Prometheus ждем кнопку "Import"
      
 ![image](https://github.com/user-attachments/assets/e8fff791-49f6-48bb-b6a3-0e0272cbaec1)

VictoriaMetrics

▹ Изменим docker-compose.yaml

① `cd grafana_stack_for_docker`

▹ Команда cd grafana_stack_for_docker изменяет текущий рабочий каталог на каталог grafana_stack_for_docker.

② `sudo vi docker-compose.yaml`

▹ Команда sudo открывает файл docker-compose.yaml в редакторе vi с правами суперпользователя.

![image](https://github.com/user-attachments/assets/ba87158f-2433-4c94-8353-75046110acc5)

▹ В текстовом редакторе после prometheus вставляем

![image](https://github.com/user-attachments/assets/1d92f15d-9018-4e0e-b760-7e24f1ba40ae)

▹ Вместо http//:prometheus:9090 пишем http:victoriametrics:9090 и заменяем имя из "Prometheus-2" в "Vika"

▹ Нажимаем на dashboards add visualition выбираем "Vika"

▹ Снизу меняем на "code"

▹ Переходим в терминал и пишем

③ `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus`

▹ Команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер, который слушает на порту 8428.

④ `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

▹ Команда делает запрос к API для получения данных по метрике OILCOINT_metric1

▹ Команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

▹ Команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.

![image](https://github.com/user-attachments/assets/9daacb6b-5767-4f2e-aa12-3b97cbff4770)

▹ Значение 0 меняем на любое другое

▹ Копируем переменную OILCOINT_metric1 и вставляем в query

▹ Нажимаем run

![image](https://github.com/user-attachments/assets/5010441a-6d6c-4780-a198-2237c2ada3ea)

![image](https://github.com/user-attachments/assets/85f5e435-3f7f-495e-840d-341fe72e54a1)

▹ Копируем OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/90e2d750-6d97-4ab1-9180-198e945948b7)

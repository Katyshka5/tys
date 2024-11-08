Перед началом установки, нужно установить Linux Oracle на VirtualBox, для этого нужно:

◦ Иметь образ Linux

◦ Выделить 2+ ядер

◦ Выделать 4096+ МБ оперативы

◦ При установке ОС выбрать английский язык

Переходим к установке docker с использованием grafana, вводим следующий набор команд:

① `sudo yum install wget`

▹ Устанавливаем утилиту wget

СКРИНШОТ

② `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

▹ Скачиваем файл репозитория

СКРИНШОТ

③ `sudo yum install docker-ce docker-ce-cli containerd.io`

▹ Устанавливаем docker

СКРИНШОТ

④ `sudo systemctl enable docker --now`

▹ Запускаем его и разрешаем автозапуск

⑤ `sudo yum install curl`

▹ Для этого сначала убедимся в наличие пакета curl

⑥ `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

▹ Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней
версии Docker Compose

СКРИНШОТ

⑦ `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`                        

▹ Скачиваем скрипт docker-compose последней версии и помещаем его в каталог /usr/bin

СКРИНШОТ

⑧ `sudo chmod +x /usr/bin/docker-compose`

▹ Предоставление прав на выполнение файла docker-compose.

⑨ `docker-compose --version`

▹ Проверка установленной версии Docker Compose.

СКРИНШОТ

▹ Можно скачать git прямо из командной строки прописав Y

⑩ `git clone https://github.com/skl256/grafana_stack_for_docker.git`

▹ Выдаст ошибку и предложит скачать git, согласиться и продолжить

СКРИНШОТ

①① `cd grafana_stack_for_docker`
    
▹ Переходим в папку

①② `sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

▹ Команда создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги, если они ещё не существуют.

①③ `sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`

▹ Команда создаёт структуру каталогов для Grafana и связанных с ней компонентов, если они ещё не существуют.

①④ `sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

▹ Все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе

①⑤ ` touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

▹ Файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

①⑥ `cp config/* /mnt/common_volume/swarm/grafana/config/`

▹ Команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

①⑦ `mv grafana.yaml docker-compose.yaml `

▹ Команда переименовывает файл grafana.yaml в docker-compose.yaml. Ничего не покажет, но можно проверить при помощи команды ls

①⑧ `sudo docker compose up -d`

▹ Команда создает и запускает контейнеры в фоновом режиме, используя конфигурацию из файла docker-compose.yml, с правами суперпользователя.

СКРИНШОТ

СКРИНШОТ

①⑨ `sudo vi docker-compose.yaml`

▹ Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет нам редактировать его содержимое.

▹ Нас перекинет в текстовый редактор

▹ Что-бы что-то изменить в тесковом редакторе нажмите insert на клавиатуре

▹ Что бы сохранить что-то в этом документе нажимаем Esc пишем :wq! В этом текставом редакторе мы должны поставить node-exporter после services

СКРИНШОТ  

СКРИНШОТ

②⓪ `sudo vi prometheus.yaml`

▹ Команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

▹ /mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100,

СКРИНШОТ

СКРИНШОТ

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
      
 СКРИНШОТ

VictoriaMetrics


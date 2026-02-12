Используемые команды : 
  1)  Создание сервисного аккаунта c ролью Storage.Admin:
      cloudshell:~ (it-server-344307)$ gcloud iam service-accounts create sa-tmsdz33 --display-name "TMS service account" 
      cloudshell:~ (it-server-344307)$ gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID   --member serviceAccount:sa-tmsdz33@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com --role="roles/storage.admin"
      cloudshell:~ (it-server-344307)$ PROJECT_ID="$DEVSHELL_PROJECT_ID" 
      cloudshell:~ (it-server-344307)$ SA_NAME="sa-tmsdz33"
      cloudshell:~ (it-server-344307)$ gcloud config set project it-server-344307                                                                                       // указываем GCP- проект
      cloudshell:~ (it-server-344307)$ gcloud iam service-accounts keys create ~/keys/${SA_NAME}.json  --iam-account=${SA_NAME}@${PROJECT_ID}.iam.gserviceaccount.com
      P.S.#1 или (без переменных) :    gcloud iam service-accounts keys create key.json   --iam-account=gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com
   
   1.3) Проверяем созданные IAM-учетки в нашем проекте :     cloudshell:~ (it-server-344307)$      gcloud iam service-accounts list
   1.4) копируем ключ (json-файл) на удаленный хост    :     cloudshell:~/keys (it-server-344307)$ cat /home/user/keys/sa-tmsdz33.json
   1.5) авторизоваться на ubuntu22 по key.json-файлу   :     ubuntu22#                        gcloud auth activate-service-account gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com   --key-file=key.json
     2) Создаем S3 GCP Bucket   (cli-вариант)          :     cloudshell:~ (it-server-344307)$ gcloud storage buckets create gs://tmsdz33     --location=EUROPE-WEST2    --default-storage-class=STANDARD  
   2.2) Предоставляем доступ к бакету “ tmsdz33” по email :  cloudshell:~ (it-server-344307)$ gcloud storage buckets add-iam-policy-binding gs://tmsdz33   --member="user:egor.belousov@altezza.org"   --role="roles/storage.objectAdmin" 
   2.3) Проверяем  атрибуты нашего бакета  “tmsdz33” :       cloudshell:~ (it-server-344307)$ gcloud storage buckets describe gs://tmsdz3 
   P.S.#2 Проверяем  атрибуты нашего бакета  “tmsdz33” в json-формате :  cloudshell:~ (it-server-344307)$  gcloud storage buckets describe gs://tmsdz33 --format="json(name,location,storageClass)"   
  
   3) Подключаемся к google cloud s3 bucket с  ubuntu22 :
      ubuntu22# snap install google-cloud-cli --classic
      ubuntu22# gcloud auth login                                                                // выдет   https-строку-запрос-на-токен   идем в браузер и авторизация через свою email-учетку.
   P.S.#3  Можно и без браузера  через  “gcloud auth login --no-browser “ но до конца не разобрался как.
      ubuntu22# gcloud config set project it-server-344307                                       // где  it-server-344307 -  ID проекта)
      ubuntu22# gcloud storage buckets create gs://tmsdz331 --location="europe-west2"
      ubuntu22# gcloud storage cp * gs://tmsdz33
      ubuntu22# gcloud storage cp * gs://tmsdz331                                                //Копируем файлы во 2й бакет  “tmsdz331”

   3.6)  Открываем доступ из Интернета  к конкретному файлу в S3 bucket :  
      cloudshell:~ (it-server-344307)$ $  gsutil acl ch -u AllUsers:R gs://tmsdz33/1.txt 

   3.8) Удаляем из бакета "tmsdz33 " файл 3.txt :
       ubuntu22 #  gcloud auth activate-service-account gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com   --key-file=key.json 
       ubuntu22 #  gcloud storage rm gs://tmsdz33/3.txt

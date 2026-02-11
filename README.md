  Домашняя работа 33 TMS по теме Google Cloud S3 Bucket + IAM.
 

1) Создаем S3 GCP Bucket   (cli-вариант)


     $ gcloud storage buckets create gs://tmsdz33     --location=EUROPE-WEST2     --default-storage-class=STANDARD                                                                               

 




Создаем bucket   (gui-вариант) :






























2) Проверяем  атрибуты нашего бакета  :


            $  gcloud storage buckets describe gs://tmsdz3




               $  gcloud storage buckets describe gs://tmsdz33 --format="json(name,location,storageClass)"





















Свойства бакета через GUI








3) Подключаемся к google cloud s3 bucket с  ubuntu :


# snap install google-cloud-cli --classic

# gcloud auth login                                                                // выдет https-строку-запрос-на-токен   (gcloud auth login –no-launch-browser)




# gcloud config set project it-server-344307                     // где  it-server-344307 -  ID проекта).




#gcloud storage buckets create gs://tmsdz331 --location="europe-west2"

Creating gs://tmsdz331/…












# gcloud storage cp * gs://tmsdz33                                     // копируем файлы в бакет






Проверяем в GUI:











Копируем файлы во 2й бакет

      #gcloud storage cp * gs://tmsdz331







4) Проверяем доступ из Интернета, его нет так как при создании указали бакет без доступа из Интернета (без public доступа).




5) Делаем доступ по email :


   $  gcloud storage buckets add-iam-policy-binding gs://tmsdz33   --member="user:egor.belousov@altezza.org"   --role="roles/storage.objectAdmin"



 


     gcloud auth login            // авторизуемся

     gcloud config set project it-server-344307     // указываем проект

     gcloud auth activate-service-account SERVICE_ACCOUNT_EMAIL  --key-file=/path/to/key.json  

   

      gcloud iam service-accounts create gitlab-runner-sa    --description="Service account for GitLab runner on U22GITLABNODE2"     --display-name="GitLab Runner SA"  // создаем сервисную учетку

     


      gcloud iam service-accounts list

      







Генерируем токен для доступа (как служба) :


 gcloud iam service-accounts keys create key.json   --iam-account=gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com

 created key [1c1e5cad0bfe780800b579c73dc60971f0810611] of type [json] as [key.json] for [gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com]         

gcloud projects add-iam-policy-binding it-server-344307 --member="serviceAccount:gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com" --role="roles/storage.admin"




Токен создали :

egor_belousov@cloudshell:~ (it-server-344307)$ cat key.json





gcloud auth activate-service-account gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com   --key-file=key.json                         

Activated service account credentials for: [gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com]







6) Открываем доступ из Интернета  к конкретному файлу в S3 bucket :


  $  gsutil acl ch -u AllUsers:R gs://tmsdz33/1.txt


Через gui:








Проверяем в браузере




7) Удаляем из бакета s3://tmsdz33 файл 3.txt



root@U22GITLABNODE2:/home/gitlab-runner# gcloud auth activate-service-account gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com   --key-file=key.json      

Activated service account credentials for: [gitlab-runner-sa@it-server-344307.iam.gserviceaccount.com]


root@U22GITLABNODE2:/home/gitlab-runner# gcloud storage rm gs://tmsdz33/3.txt

Removing objects:

Removing gs://tmsdz33/3.txt...                                                                                                                                                                                  

  Completed 1/1








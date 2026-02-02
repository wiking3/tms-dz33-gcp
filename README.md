Домашняя работа 33 TMS по теме Google Cloud S3 Bucket + IAM.

<h5> 1й шаг) Создаем S3 Bucket  </h5>

egor@cloudshell:~ (it-server-344)$ gcloud storage buckets create gs://tmsdz33     --location=EUROPE-WEST2     --default-storage-class=STANDARD                                                                                
Creating gs://tmsdz33/...

<img width="1255" height="50" alt="image" src="https://github.com/user-attachments/assets/22d9ac71-4add-4b6a-9449-6abb1b1b9b17" />

<h5> 2й шаг) Проверяем у кого есть доступ к GCP S3  Bucket  </h5>
<kbd style="background-color: #eee; padding: 2px 4px; border-radius: 3px;"> egor@cloudshell:~ (it-server-344307)$  gcloud storage buckets list </kbd>

<pre>
acl:
- entity: project-owners-52615052819
  projectTeam:
    projectNumber: '52615052819'
    team: owners
  role: OWNER
default_storage_class: STANDARD
generation: 1770038782129218882
location: EUROPE-WEST2
location_type: region
metageneration: 1
name: tmsdz33
public_access_prevention: inherited
satisfies_pzs: true
soft_delete_policy:
  effectiveTime: '2026-02-02T13:26:22.487000+00:00'
  retentionDurationSeconds: '604800'
storage_url: gs://tmsdz33/
uniform_bucket_level_access: false
update_time: 2026-02-02T13:26:22+0000
</pre>pre>


Предоставляем всем на чтение доступ на 1 файл -    gsutil acl ch -u AllUsers:R gs://tmsdz33/1.txt 
<img width="749" height="37" alt="image" src="https://github.com/user-attachments/assets/831389c9-803f-4839-8340-6471f5e2ad23" />




# SigNoz

```shell
👋 Thank you for trying out SigNoz! 

🟡 Running installer with non-sudo permissions.
   In case of any failure or prompt, please consider running the script with sudo privileges.

🌏 Detecting your OS ...

docker compose plugin is present, using it
🐳 Starting Docker ...



🟡 Pulling the latest container images for SigNoz.

WARN[0000] /home/tuanta/Projects/jod/infra/signoz/deploy-v0.92.2/docker/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Pulling 7/7
 ✔ schema-migrator-async Skipped - Image is already being pulled by schema-migrator-sync                                                                                                                                0.0s 
 ✔ init-clickhouse Skipped - Image is already being pulled by clickhouse                                                                                                                                                0.0s 
 ✔ schema-migrator-sync Pulled                                                                                                                                                                                          2.8s 
 ✔ clickhouse Pulled                                                                                                                                                                                                    9.2s 
 ✔ signoz Pulled                                                                                                                                                                                                        2.8s 
 ✔ zookeeper-1 Pulled                                                                                                                                                                                                   3.3s 
 ✔ otel-collector Pulled                                                                                                                                                                                                4.2s 

🟡 Starting the SigNoz containers. It may take a few minutes ...

WARN[0000] /home/tuanta/Projects/jod/infra/signoz/deploy-v0.92.2/docker/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 7/7
 ✔ Container signoz-init-clickhouse  Exited                                                                                                                                                                             5.5s 
 ✔ Container signoz-zookeeper-1      Healthy                                                                                                                                                                            4.5s 
 ✔ Container signoz-clickhouse       Healthy                                                                                                                                                                           36.7s 
 ✔ Container signoz                  Healthy                                                                                                                                                                           72.6s 
 ✔ Container signoz-otel-collector   Started                                                                                                                                                                           72.4s 
 ✔ Container schema-migrator-sync    Exited                                                                                                                                                                             8.3s 
 ✔ Container schema-migrator-async   Started                                                                                                                                                                            0.2s 


++++++++++++++++++ SUCCESS ++++++++++++++++++++++

🟢 Your installation is complete!

🟢 SigNoz is running on http://localhost:8080

ℹ️  By default, retention period is set to 15 days for logs and traces, and 30 days for metrics.
To change this, navigate to the General tab on the Settings page of SigNoz UI. For more details, refer to https://signoz.io/docs/userguide/retention-period 

ℹ️  To bring down SigNoz and clean volumes:

cd docker
 docker compose down -v

+++++++++++++++++++++++++++++++++++++++++++++++++

👉 Need help in Getting Started?
Join us on Slack https://signoz.io/slack
```
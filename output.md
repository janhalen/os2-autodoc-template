```mermaid
%%{init: {'theme': 'default'}}%%
flowchart TB
  Vdevenvironmentnginxproxyconftemplate{{./dev-environment/nginx-proxy.conf.template}} -. /etc/nginx/templates/default.conf.template .-x proxy
  Vpoetrylock{{./poetry.lock}} x-. /app/poetry.lock .-x mo
  Vpyprojecttoml{{./pyproject.toml}} x-. /app/pyproject.toml .-x mo
  Vbackend{{./backend}} x-. /app/backend .-x mo
  Vpytestcache{{./.pytest_cache}} x-. /app/.pytest_cache .-x mo
  Vcovreport{{./.cov-report}} x-. /tmp/.cov-report .-x mo
  Vdevenvironmentwaitforrabbitmqsh{{./dev-environment/wait-for-rabbitmq.sh}} x-. /wait-for-rabbitmq.sh .-x mo
  Vbackend x-. /app/backend .-x amqpsubsystem[amqp-subsystem]
  Vdevenvironmentwaitforrabbitmqsh x-. /wait-for-rabbitmq.sh .-x amqpsubsystem
  Vdevenvironmentmoxproxyconftemplate{{./dev-environment/mox-proxy.conf.template}} -. /etc/nginx/templates/default.conf.template .-x mox
  Vdevenvironmentmoxdbinstallpgtapsh{{./dev-environment/moxdb-install-pgtap.sh}} -. /docker-entrypoint-initdb.d/10-install-pgtap.sh .-x moxdb[mox-db]
  Vkeycloakvolume([keycloak-volume]) -. /srv/ .-x keycloak
  Vkeycloakvolume x-. /srv/ .-x keycloakrealmbuilder[keycloak-realm-builder]
  amqpsubsystem --> fixtureloader[fixture-loader]
  frontendnewstatic[frontend_new_static] --> proxy
  frontendstatic[frontend_static] --> mo
  keycloak --> proxy
  mo --> fixtureloader
  moinit[mo-init] --> fixtureloader
  moinit --> mo
  test-->test

  moxdbinit[mox-db-init]
  msgbroker[msg-broker]
  keycloakdb[keycloak-db]
  amqpmetrics[amqp-metrics]

  classDef volumes fill:#fdfae4,stroke:#867a22
  class Vdevenvironmentnginxproxyconftemplate,Vpoetrylock,Vpyprojecttoml,Vbackend,Vpytestcache,Vcovreport,Vdevenvironmentwaitforrabbitmqsh,Vbackend,Vdevenvironmentwaitforrabbitmqsh,Vdevenvironmentmoxproxyconftemplate,Vdevenvironmentmoxdbinstallpgtapsh,Vkeycloakvolume,Vkeycloakvolume volumes
```

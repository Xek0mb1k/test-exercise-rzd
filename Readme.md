ТЗ: 
- [x] 1) Развернуть прометеус на хосте как службу (развертывание и конфигурирование организовать через ансибл, можно простой плейбук без роли)
- [x] 2) Развернуть в докер контейнере графану (организовать сохранность дашбордов в случае удаления контейнера)
- [x] 3) Создать один дашборд в графане, который будет мониторить состояние докер контейнеров на хосте
- [x] 4) Настроить мониторинг контейнеров на хосте (по факту на дашборде будет только два контейнера - один графана, второй ... нужен для мониторинга)
В конечном счете при заходе в графану мы должны увидеть рабочий дашборд с информацией по работе контейнеров.
При удалении и повторном создании контейнера графаны дашборд и авторизационные данные должны сохраняться.
Прометеус должен полностью разворачиваться и конфигурироваться через ансибл.
Хостовая машина - линукс.

Host - Fedora linux 40

**1. Развернуть прометеус на хосте как службу (развертывание и конфигурирование организовать через ансибл, можно простой плейбук без роли)**

![image1](https://github.com/user-attachments/assets/b8dbe417-5f3f-416e-b0f8-1a2b428b9ab2)

![Pasted image 20240731021401](https://github.com/user-attachments/assets/1e8b8cf9-1d10-4b40-8c89-55455a9f0af5)


**2. Развернуть в докер контейнере графану (организовать сохранность дашбордов в случае удаления контейнера)**

**Grafana**
```bash
sudo docker run -d -p 3000:3000 \
  -v grafana-storage:/var/lib/grafana \
  --name=grafana \
  grafana/grafana
```

**cAdvisor**
```bash
sudo docker run -d \
  --name=cadvisor \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  -p 8080:8080 \
  gcr.io/cadvisor/cadvisor:latest
```

**3. Создать один дашборд в графане, который будет мониторить состояние докер контейнеров на хосте**
![Pasted image 20240731041056](https://github.com/user-attachments/assets/3620ffd7-73e0-4471-8496-b640107a65ab)

## Save grafana container

```bash
docker stop 9a07
docker commit 9a07 grafana_completed
docker commit 9a07 grafana_completed
docker save -o grafana_completed.tar grafana_completed
```

**Load image**
docker load -i grafana_completed.tar



**4. Настроить мониторинг контейнеров на хосте (по факту на дашборде будет только два контейнера - один графана, второй ... нужен для мониторинга)**

Готовый дашборд
![Pasted image 20240731040615](https://github.com/user-attachments/assets/0837c206-a965-4388-8d26-b6d9ea87258d)

# Lab 7: Docker Containers and Images

- Student GitHub username: faisal-yusra
- Environment: Ubuntu Server (VMware), hostname `ubuntu`
- Submission: `DevOps/Labs/Lab07`

## Tasks completed

| Task | What was done | Screenshots |
|------|---------------|-------------|
| 1 | Checked Docker and Docker Compose versions | `docker_installation.png` |
| 2 | Pulled PostgreSQL 9.6, saw the missing-password error, ran 9.6 and 10.10 in the background | `postgres_missing_password.png`, `postgres_96_running.png`, `postgres_both_versions.png` |
| 3 | Ran `docker info`, searched Docker Hub for Ubuntu, compared `docker ps` with `docker ps -a`, restarted/stopped/removed the 9.6 container and removed its image | `docker_info.png`, `docker_search_ubuntu.png`, `ubuntu_container_status.png`, `postgres_96_cleanup.png` |
| 4 | Ran NGINX `latest` on port 6001 and `1.30.6` on port 6002, opened both pages, checked logs | `nginx_containers_started.png`, `nginx_latest_page.png`, `nginx_older_page.png`, `nginx_latest_logs.png` |
| 5 | Attached/detached, opened a shell with `docker exec`, ran `docker diff` and `docker inspect`, saved `nginx:myimage` with `docker commit` | `nginx_attach_detach.png`, `nginx_exec_shell.png`, `nginx_diff.png`, `nginx_inspect.png`, `nginx_commit.png` |

## Key commands

```bash
docker --version
docker compose version
docker pull postgres:9.6
docker run -d -e POSTGRES_PASSWORD=password postgres:9.6
docker run -d -e POSTGRES_PASSWORD=password postgres:10.10
docker info
docker search ubuntu
docker run -d ubuntu
docker ps -a
docker run -dit -p 6001:80 --name nginx-latest nginx
docker run -d -p 6002:80 --name nginx-older nginx:1.30.6
docker logs nginx-latest
docker attach nginx-latest
docker exec -it nginx-latest /bin/sh
docker diff nginx-latest
docker inspect nginx-latest
docker commit nginx-latest nginx:myimage
```

## Folder structure

```
Labs/Lab07/
├── README.md
└── screenshots/   (17 PNG files)
```

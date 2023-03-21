# Конфигурация операционной системы

## Локаль
 
 Необходимо наличие системной локали(На Альт нет)
 ```bash
 sudo locale-gen "en_US.UTF-8"
 ```

## Файловая система

Разработчиками аранги не рекомендуется использование BTRFS, AUFS а так же NFS. Последняя не рекомендуется по соображениям производительности.

## swap

Наличие свопа обязательно во избежание OOM

## Overcommit memory

/proc/sys/vm/overcommit_memory = 0

## limits

open_files 1048576

```bash
# echo '*   -   nofile  1048576' >> /etc/security/limits.d/50-defaults.conf
```

## page size

```bash
$ sudo bash -c "echo madvise >/sys/kernel/mm/transparent_hugepage/enabled"
$ sudo bash -c "echo madvise >/sys/kernel/mm/transparent_hugepage/defrag"
```

## memory mapping

расчитать размер по формуле - CPU_count * 8 * 8000

```bash
# echo 'vm.max_map_count=2048000' >> /etc/sysctl.d/99-sysctl.conf
# sysctl --system
```

но лучше внести данные значения в файл /etc/sysctl.d/99-sysctl.conf


[Назад](README.md)
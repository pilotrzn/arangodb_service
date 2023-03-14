# Конфигурация операционной системы

## Локаль
 
 Необходимо наличие системной локали
 ```bash
 sudo locale-gen "en_US.UTF-8"
 ```

## Файловая система

Разработчиками аранги не рекомендуеься использование BTRFS, AUFS а так же NFS. Последняя не рекомендуется по соображениям производительности.

## swap

Наличие свопа обязательно во избежание OOM

## Overcommit memory

/proc/sys/vm/overcommit_memory = 0

## limits

open_files 1048576

## page size

sudo bash -c "echo madvise >/sys/kernel/mm/transparent_hugepage/enabled"
sudo bash -c "echo madvise >/sys/kernel/mm/transparent_hugepage/defrag"

## memory mapping

расчитать размер по формуле - CPU * 8 * 8000

sudo bash -c "sysctl -w 'vm.max_map_count=2048000'"

но лучше внести данные значения в файл /etc/sysctl.d/fliename.conf
Понятие «Диск» на платформе VK Cloud является аналогом физического носителя информации, такого как HDD или SSD.

Диск — это сетевое блочное устройство, которые обеспечивает хранилище данных для инстансов. Все диски на платформе VK Cloud являются сетевыми и надежно защищены репликацией данных, обеспечивающими надежность хранения и отказоустойчивость.

Преимущества сетевых дисков:

- **Гибкость**. Диски — это независимые ресурсы, поэтому их можно перемещать между инстансами в одном центре обработки данных, и можно увеличивать размер диска, не выключая инстанс, к которому он подключен.
- **Простота**. Диски функционируют как универсальные блочные устройства, поэтому можно рассматривать присоединенные диски как локально подключенные накопители. Это позволяет разбивать, форматировать диски и управлять ими с помощью знакомых инструментов и методов.
- **Применение**. Диски является независимым элементом проекта и может существовать отдельно от инстанса. Это удобно, когда требуется изменить размер диска вне зависимости от конфигурации виртуальной машины.
- **Отказоустойчивость**. Диски обеспечивают надежное хранение данных и позволяют непрерывно выполнять операции чтения и записи даже при выходе из строя одновременно нескольких физических дисков.

<warn>

Созданный диск занимает место в общем хранилище, поэтому его наличие оплачивается отдельно, даже если он отключен от инстанса.

</warn>

## Типы дисков

| Тип диска | Название в API | Выбор зоны доступности при создании диска | Описание |
| --- | --- | --- | --- |
| Сетевой SSD | ceph-hdd deprecated: <br> dp1, <br> ms1 | Да | Сетевой HDD диск с низкой скоростью работы. <br> Обладает тройной репликацией между несколькими серверами СХД внутри зоны доступности. |
| Сетевой SSD с георепликацией | ceph | Не указывается по причине георепликации | Сетевой HDD диск с низкой скоростью работы. <br> Обладает тройной георепликацией между несколькими зонами доступности. |
| High IOPS SSD | high-iops deprecated:<br> local-ssd, <br> ko1-local-ssd, <br> ko1-high-iops, <br> dp1-high-iops, <br> ko1-local-ssd-g2 | Да | Сетевой SSD диск с двойной репликацией (обе копии находятся на одном сервере СХД)<br> и повышенной скоростью работы. |
| Low Latency NVME | ef-nvme | Располагается на одном гипервизоре с виртуальной машиной | Локальный SSD диск с двойной репликацией (обе копии находятся на одном гипервизоре), <br> высокой скоростью работы и низкими задержками. |

<warn>

Диски с типами Сетевой HDD, Сетевой SSD и High IOPS SSD рекомендуется располагать в той же зоне доступности, где находится виртуальная машина, к которой они будут подключены. В противном случае производительность виртуальной машины снизится, потому что диск будет расположен в другом датацентре.

</warn>

## SLA

Для каждого типа дисков существует ограничение по производительности, необходимое для гарантирования стабильности работы диска вне зависимости от его типа или объема.

| Тип диска | min IOPS read | min IOPS write | IOPS/GB read | IOPS/GB write | max IOPS read | max IOPS write | Tab8 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDD ceph (типы дисков ceph, ms1, dp1) | 300 | 150 | 1 | 1 | 2 400 | 800 | — |
| SSD ceph (типы дисков ssd, dp1-ssd, ko1-ssd) | 1 000 | 500 | 30 | 15 | 16 000 | 8 000 | — |
| SSD High IOPS | 10 000 | 5 000 | 30 | 25 | 45 000 | 30 000 | — |
| Low Latency NVME | 10 000 | 5 000 | 70 | 35 | 75 000 | 50 000 | 0,5 |

\* полные характеристики приведены ниже в отдельных таблицах для каждого типа дисков.

**HDD**

| Объём в GB | Размер блока | Чтение IOPS | Чтение MB/s | Размер блока | Запись IOPS | Запись MB/s |
|------------|--------------|-------------|-------------|--------------|-------------|-------------|
| 10         | 4K           | 300         | 1           | 4K           | 150         | 1           |
| 10         | 64K          | 300         | 9           | 64K          | 150         | 5           |
| 10         | 1M           | 300         | 38          | 1M           | 150         | 19          |
| 50         | 4K           | 300         | 1           | 4K           | 150         | 1           |
| 50         | 64K          | 300         | 9           | 64K          | 150         | 5           |
| 50         | 1M           | 300         | 38          | 1M           | 150         | 19          |
| 100        | 4K           | 300         | 1           | 4K           | 150         | 1           |
| 100        | 64K          | 300         | 9           | 64K          | 150         | 5           |
| 100        | 1M           | 300         | 38          | 1M           | 150         | 19          |
| 250        | 4K           | 300         | 1           | 4K           | 250         | 1           |
| 250        | 64K          | 300         | 9           | 64K          | 250         | 8           |
| 250        | 1M           | 300         | 38          | 1M           | 250         | 31          |
| 500        | 4K           | 500         | 2           | 4K           | 500         | 2           |
| 500        | 64K          | 500         | 16          | 64K          | 500         | 16          |
| 500        | 1M           | 500         | 63          | 1M           | 500         | 63          |
| 1000       | 4K           | 1000        | 4           | 4K           | 800         | 3           |
| 1000       | 64K          | 1000        | 31          | 64K          | 800         | 25          |
| 1000       | 1M           | 1000        | 125         | 1M           | 800         | 100         |
| 1500       | 4K           | 1500        | 6           | 4K           | 800         | 3           |
| 1500       | 64K          | 1500        | 47          | 64K          | 800         | 25          |
| 1500       | 1M           | 1500        | 188         | 1M           | 800         | 100         |
| 2000       | 4K           | 2000        | 8           | 4K           | 800         | 3           |
| 2000       | 64K          | 2000        | 63          | 64K          | 800         | 25          |
| 2000       | 1M           | 2000        | 250         | 1M           | 800         | 100         |

**SSD**

| Объём в GB | Размер блока | Чтение (IOPS) | Чтение (MB/s) | Размер блока | Чтение (IOPS) | Чтение (MB/s) |
|------------|--------------|---------------|---------------|--------------|---------------|---------------|
| 10         | 4K           | 1000          | 4             | 4K           | 500           | 2             |
| 10         | 64K          | 1000          | 31            | 64K          | 500           | 16            |
| 10         | 1M           | 1000          | 125           | 1M           | 500           | 63            |
| 50         | 4K           | 1500          | 6             | 4K           | 750           | 3             |
| 50         | 64K          | 1500          | 47            | 64K          | 750           | 23            |
| 50         | 1M           | 1500          | 188           | 1M           | 750           | 94            |
| 100        | 4K           | 3000          | 12            | 4K           | 1500          | 6             |
| 100        | 64K          | 3000          | 94            | 64K          | 1500          | 47            |
| 100        | 1M           | 3000          | 375           | 1M           | 1500          | 188           |
| 250        | 4K           | 7500          | 29            | 4K           | 3750          | 15            |
| 250        | 64K          | 7500          | 234           | 64K          | 3750          | 117           |
| 250        | 1M           | 7500          | 400           | 1M           | 3750          | 400           |
| 500        | 4K           | 15000         | 59            | 4K           | 7500          | 29            |
| 500        | 64K          | 15000         | 400           | 64K          | 7500          | 234           |
| 500        | 1M           | 15000         | 400           | 1M           | 7500          | 400           |
| 1000       | 4K           | 16000         | 63            | 4K           | 8000          | 31            |
| 1000       | 64K          | 16000         | 400           | 64K          | 8000          | 250           |
| 1000       | 1M           | 16000         | 400           | 1M           | 8000          | 400           |
| 1500       | 4K           | 16000         | 63            | 4K           | 8000          | 31            |
| 1500       | 64K          | 16000         | 400           | 64K          | 8000          | 250           |
| 1500       | 1M           | 16000         | 400           | 1M           | 8000          | 400           |
| 2000       | 4K           | 16000         | 63            | 4K           | 8000          | 31            |
| 2000       | 64K          | 16000         | 400           | 64K          | 8000          | 250           |
| 2000       | 1M           | 16000         | 400           | 1M           | 8000          | 400           |

**High-IOPS SSD**

| Объём в GB | Размер блока | Чтение (IOPS) | Чтение (MB/s) | Размер блока | Запись (IOPS) | Запись (MB/s) |
|------------|--------------|---------------|---------------|--------------|---------------|---------------|
| 10         | 4K           | 10000         | 39            | 4K           | 5000          | 20            |
| 10         | 64K          | 10000         | 313           | 64K          | 5000          | 156           |
| 10         | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 50         | 4K           | 10000         | 39            | 4K           | 5000          | 20            |
| 50         | 64K          | 10000         | 313           | 64K          | 5000          | 156           |
| 50         | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 100        | 4K           | 10000         | 39            | 4K           | 5000          | 20            |
| 100        | 64K          | 10000         | 313           | 64K          | 5000          | 156           |
| 100        | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 250        | 4K           | 10000         | 39            | 4K           | 6250          | 24            |
| 250        | 64K          | 10000         | 313           | 64K          | 6250          | 195           |
| 250        | 1M           | 10000         | 500           | 1M           | 6250          | 500           |
| 500        | 4K           | 15000         | 59            | 4K           | 12500         | 49            |
| 500        | 64K          | 15000         | 469           | 64K          | 12500         | 391           |
| 500        | 1M           | 15000         | 500           | 1M           | 12500         | 500           |
| 1000       | 4K           | 30000         | 117           | 4K           | 25000         | 98            |
| 1000       | 64K          | 30000         | 500           | 64K          | 25000         | 500           |
| 1000       | 1M           | 30000         | 500           | 1M           | 25000         | 500           |
| 1500       | 4K           | 45000         | 176           | 4K           | 30000         | 117           |
| 1500       | 64K          | 45000         | 500           | 64K          | 30000         | 500           |
| 1500       | 1M           | 45000         | 500           | 1M           | 30000         | 500           |
| 2000       | 4K           | 45000         | 176           | 4K           | 30000         | 117           |
| 2000       | 64K          | 45000         | 500           | 64K          | 30000         | 500           |
| 2000       | 1M           | 45000         | 500           | 1M           | 30000         | 500           |

**LL NVME**

| Объём в GB | Размер блока | Чтение (IOPS) | Чтение (Mb/s) | Размер блока | Запись (IOPS) | Запись (Mb/s) |
|------------|--------------|---------------|---------------|--------------|---------------|---------------|
| 10         | 4K           | 10000         | 39,0625       | 4K           | 5000          | 19,53125      |
| 10         | 64K          | 10000         | 350           | 64K          | 5000          | 200           |
| 10         | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 50         | 4K           | 10000         | 39,0625       | 4K           | 5000          | 19,53125      |
| 50         | 64K          | 10000         | 350           | 64K          | 5000          | 200           |
| 50         | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 100        | 4K           | 10000         | 39,0625       | 4K           | 5000          | 19,53125      |
| 100        | 64K          | 10000         | 350           | 64K          | 5000          | 250           |
| 100        | 1M           | 10000         | 500           | 1M           | 5000          | 500           |
| 250        | 4K           | 18750         | 73,2421875    | 4K           | 8750          | 34,1796875    |
| 250        | 64K          | 18750         | 350           | 64K          | 8750          | 250           |
| 250        | 1M           | 18750         | 585,9375      | 1M           | 8750          | 500           |
| 500        | 4K           | 37500         | 146,484375    | 4K           | 17500         | 68,359375     |
| 500        | 64K          | 37500         | 585,9375      | 64K          | 17500         | 500           |
| 500        | 1M           | 37500         | 1171,875      | 1M           | 17500         | 546,875       |
| 1000       | 4K           | 75000         | 292,96875     | 4K           | 35000         | 136,71875     |
| 1000       | 64K          | 75000         | 1171,875      | 64K          | 35000         | 546,875       |
| 1000       | 1M           | 75000         | 1200          | 1M           | 35000         | 900           |
| 2000       | 4K           | 75000         | 292,96875     | 4K           | 50000         | 195,3125      |
| 2000       | 64K          | 75000         | 1171,875      | 64K          | 50000         | 781,25        |
| 2000       | 1M           | 75000         | 1200          | 1M           | 50000         | 900           |

<info>

Производительность диска напрямую зависит от его объема. При необходимости увеличить скорость обработки данных иногда достаточно увеличить размер требуемого диска.

</info>

## Как провести тестирование

### Для Windows

Для измерения показателей IOPS чтения/записи можно воспользоваться программным обеспечением DiskSPD или Fio.

**DiskSPD**

DiskPSD — официальный инструмент тестирования, рекомендованный компанией Microsoft и [включенная в репозитории разработчиков](https://aka.ms/diskspd). Следующие шаги необходимы для выполнения тестирования:

1. Загрузить утилиту с официального ресурса и распаковать ее в удобное место: [https://github.com/microsoft/diskspd/releases/latest](https://github.com/microsoft/diskspd/releases/latest).
2. Запустить командную строку от администратора и перейти в каталог с распакованной утилитой DiskSpd-2.0.21a\\amd64\\.
3. Предварительно создать пустой файл с размером не менее 10GB:

```bash
fsutil file createnew C:\temp\test.bin 10485760000
```

4.  Для выполнения тестов необходимо применить соответствующую типу теста команду:

- Тест случайной записи:

```bash
diskspd -Suw -b4K -o1 -t32 -r -w100 C:\temp\test.bin > C:\temp\random_write_results.txt
```

- Тест случайного чтения:

```bash
diskspd -Suw -b4K -o1 -t32 -r -w0 C:\temp\test.bin > C:\temp\random_read_results.txt
```

**Fio**

Измерения показателей IOPS с помощью fio производятся с указанием параметра rate_iops. Тесты выполняются со значениями:

- \--rw (randread или randwrite)
- \--filename (имя тестируемого устройства)
- \--iodepth (8, 16, 32 или 64)

Скачать и установить Fio с [официального ресурса](https://bsdio.com/fio/).

Команда для выполнения теста:

```bash
fio --name=randwrite --iodepth=32 --rw=randwrite --bs=4k --direct=1 --size=10G --numjobs=1 --runtime=240 --group_reporting --filename=C:\Users\ADMIN\test
```

<info>

Механика работы Fio отличается от инструмента DiskSPD. Fio выполняет запись в 2 файла, поэтому результаты измерения могут быть разными у обоих инструментов. Тем не менее корпорация Microsoft доверяет своему инструменту и рекомендует на операционных системах семейства Windows использовать DiskSPD.

</info>

### Для Linux

Измерения показателей IOPS чтения/записи осуществляются программным обеспечением fio и указанием параметра rate_iops. Тесты выполняются со значениями:

- \--rw (randread или randwrite)
- \--filename (имя тестируемого устройства)
- \--iodepth (8, 16, 32 или 64)

Установка Fio:

```bash
sudo apt install fio
```

Команда для выполнения теста:

```bash
fio --name=randwrite --ioengine=libaio --iodepth=32 --rw=randwrite --bs=4k --direct=1 --size=512M --numjobs=1 --runtime=240 --group_reporting --filename=/home/user/test
```

Результаты измерений:

- read: IOPS
- write: IOPS

| Типы проводимого тестирования | Результат тестирования (Количество IOPS) |
| --- | --- |
| Чтение/запись блоками по 4 КБ в 32 потока | В соответствии со значениями SLA |
| Чтение/запись блоками по 8 КБ в 32 потока | Не менее 75% от SLA |
| Чтение/запись  по 16 КБ в 32 потока | Не менее 50% от SLA |

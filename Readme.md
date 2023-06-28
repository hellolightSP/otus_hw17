**Домашнее задание**

Настраиваем бэкапы

**Что нужно сделать**

- Настроить стенд Vagrant с двумя виртуальными машинами: backup_server и client.
- Настроить удаленный бекап каталога /etc c сервера client при помощи borgbackup. Резервные копии должны соответствовать следующим критериям:

1) директория для резервных копий /var/backup. Это должна быть отдельная точка монтирования. В данном случае для демонстрации размер не принципиален, достаточно будет и 2GB;
2) репозиторий дле резервных копий должен быть зашифрован ключом или паролем - на ваше усмотрение;
3) имя бекапа должно содержать информацию о времени снятия бекапа;
4) глубина бекапа должна быть год, хранить можно по последней копии на конец месяца, кроме последних трех. Последние три месяца должны содержать копии на каждый день. Т.е. должна быть правильно настроена политика удаления старых бэкапов;
5) резервная копия снимается каждые 5 минут. Такой частый запуск в целях демонстрации;
6) написан скрипт для снятия резервных копий. Скрипт запускается из соответствующей Cron джобы, либо systemd timer-а - на ваше усмотрение;
7) настроено логирование процесса бекапа. Для упрощения можно весь вывод перенаправлять в logger с соответствующим тегом. Если настроите не в syslog, то обязательна ротация логов.
8) Запустите стенд на 30 минут. Убедитесь что резервные копии снимаются.
9) Остановите бекап, удалите (или переместите) директорию /etc и восстановите ее из бекапа.

Для сдачи домашнего задания ожидаем настроенные стенд, логи процесса бэкапа и описание процесса восстановления.
Формат сдачи ДЗ - vagrant + ansible


**Выполнение**

1) Создал [Vagrantfile](https://github.com/hellolightSP/otus_hw17/blob/main/Vagrantfile)
- Написал автоматизацию на [ansible](https://github.com/hellolightSP/otus_hw17/tree/main/ansible/backup)
- [Лог развертывания](https://github.com/hellolightSP/otus_hw17/blob/main/otus_hw17_backup) 

2) Не смог автоматизировать процесс равертывания сервиса borg с зашифрованным ключем, в момент инициализации возникает запрос пароля, не знаю как это обойти в ansible, поэтому
  использовал инициализацию репозитория в плейбуке без ключа
```
  borg init -e none borg@192.168.56.160:/var/backup
```
Если нужен репозиторий с зашифрованным ключем или паролем, после раскатки плейбуком можно вручную создать новый репозиторий c параметром --encryption=repokey 

3) Имя бэкапов содержит информацию о времени снятия

borg list borg@192.168.56.160:/var/backup/
```
Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
etc-2023-06-28_11:37:34              Wed, 2023-06-28 11:37:35 [f4252ef7c73ac489e1ff27f948a602d038363af8137497e1d3a773c016fa4be9]
etc-2023-06-28_11:42:46              Wed, 2023-06-28 11:42:47 [a197328bb4dde7706b7973b6673f31bad61e3c0edab81f544d00dda6a083507c]
etc-2023-06-28_11:47:26              Wed, 2023-06-28 11:47:27 [c1b13743aa2528f91e7ca23e62d3072d1d5e4bc87f09266b3b7facf83a88c24c]
etc-2023-06-28_11:53:22              Wed, 2023-06-28 11:53:23 [68014dd7fbe3ee288ecb09225d2d6851568f93164480f1e1de4e5b652790f1f4]
etc-2023-06-28_11:59:22              Wed, 2023-06-28 11:59:23 [00dd443894333f5e521af9121f6f9fcd23773e5434ccf976d340c1e21f2fde97]
etc-2023-06-28_12:05:22              Wed, 2023-06-28 12:05:23 [67b7dfad48c20c35384633bd6a1a46b1109d41bbc63668e340aeea5f9242289c]
```
4) Параметры хранения бэкапов регулируются в конфиге [borg-backup.service](https://github.com/hellolightSP/otus_hw17/blob/main/ansible/backup/roles/backup/templates/borg-backup.service.j2)

5) Резервная копия снимается каждые 5 минут
```
etc-2023-06-28_11:37:34              Wed, 2023-06-28 11:37:35 
etc-2023-06-28_11:42:46              Wed, 2023-06-28 11:42:47 
etc-2023-06-28_11:47:26              Wed, 2023-06-28 11:47:27 
etc-2023-06-28_11:53:22              Wed, 2023-06-28 11:53:23 
etc-2023-06-28_11:59:22              Wed, 2023-06-28 11:59:23 
etc-2023-06-28_12:05:22              Wed, 2023-06-28 12:05:23
```
6) Написан [таймер](https://github.com/hellolightSP/otus_hw17/blob/main/ansible/backup/roles/backup/templates/borg-backup.timer.j2)

systemctl list-timers --all
```
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Wed 2023-06-28 12:10:21 UTC  3min 34s left Wed 2023-06-28 12:05:21 UTC  1min 25s ago borg-backup.timer            borg-backup.service
```
7) Логирование ведется системиным журналом, логи пишутся в /var/log/messages, так же логи можно смотреть командой 
journalctl -u borg-backup.service
```
Jun 28 12:37:00 client systemd[1]: Started Borg Backup.
Jun 28 12:42:06 client systemd[1]: Starting Borg Backup...
Jun 28 12:42:07 client borg[9281]: Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
Jun 28 12:42:08 client borg[9281]: ------------------------------------------------------------------------------
Jun 28 12:42:08 client borg[9281]: Archive name: etc-2023-06-28_12:42:06
Jun 28 12:42:08 client borg[9281]: Archive fingerprint: 4583e0594364abdf2ccaf470eea5a34fe774b356ead0d6c7cd933099972b7562
Jun 28 12:42:08 client borg[9281]: Time (start): Wed, 2023-06-28 12:42:08
Jun 28 12:42:08 client borg[9281]: Time (end):   Wed, 2023-06-28 12:42:08
Jun 28 12:42:08 client borg[9281]: Duration: 0.48 seconds
Jun 28 12:42:08 client borg[9281]: Number of files: 1700
Jun 28 12:42:08 client borg[9281]: Utilization of max. archive size: 0%
Jun 28 12:42:08 client borg[9281]: ------------------------------------------------------------------------------
Jun 28 12:42:08 client borg[9281]: Original size      Compressed size    Deduplicated size
Jun 28 12:42:08 client borg[9281]: This archive:               28.43 MB             13.42 MB            125.19 kB
Jun 28 12:42:08 client borg[9281]: All archives:               56.86 MB             26.85 MB             11.91 MB
Jun 28 12:42:08 client borg[9281]: Unique chunks         Total chunks
Jun 28 12:42:08 client borg[9281]: Chunk index:                    1284                 3390
Jun 28 12:42:08 client borg[9281]: ------------------------------------------------------------------------------
Jun 28 12:42:09 client borg[9285]: Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
Jun 28 12:42:11 client borg[9291]: Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
Jun 28 12:42:12 client systemd[1]: Started Borg Backup.
```
8) Запустите стенд на 30 минут.Запустили, убедились

9) Остановите бекап, удалите (или переместите) директорию /etc и восстановите ее из бекапа.
```
[root@client]# mkdir /restore
[root@client]# cd /restrore
[root@client restore]# borg extract -v 'borg@192.168.56.160:/var/backup'::etc-2023-06-28_12:17:22
[root@client restore]# ls -la /restore/etc
total 1072
drwxr-xr-x. 78 root root     8192 Jun 28 11:41 .
drwxr-xr-x.  3 root root       17 Jun 28 12:19 ..
-rw-------.  1 root root        0 Apr 30  2020 .pwd.lock
-rw-r--r--.  1 root root      163 Apr 30  2020 .updated
-rw-r--r--.  1 root root     5090 Aug  6  2019 DIR_COLORS
-rw-r--r--.  1 root root     5725 Aug  6  2019 DIR_COLORS.256color
-rw-r--r--.  1 root root     4669 Aug  6  2019 DIR_COLORS.lightbgcolor
-rw-r--r--.  1 root root       94 Mar 24  2017 GREP_COLORS
drwxr-xr-x.  7 root root      134 Apr  2  2020 NetworkManager
drwxr-xr-x.  5 root root       57 Apr 30  2020 X11
-rw-r--r--.  1 root root       16 Apr 30  2020 adjtime
-rw-r--r--.  1 root root     1529 Apr  1  2020 aliases
-rw-r--r--.  1 root root    12288 Jun 28 11:20 aliases.db
```
Удаляем /etc/ и восстанавливаем из бэкапа

```
[root@client restore]#rm -rf /etc/*
[root@client restore]#ll /etc/
total 0
[root@client restore]# mv ./etc/* /etc/ && mv ./etc/.* /etc/
[root@client restore]# ll /etc/
total 1072
```

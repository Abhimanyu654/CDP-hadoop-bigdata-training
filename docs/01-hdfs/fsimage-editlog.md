# FSImage, EditLog, and Checkpoint

## FSImage
A persisted snapshot of the NameNode filesystem namespace at a particular transaction point. It contains namespace metadata and file-to-block relationships, not the file's actual bytes.

## EditLog
A sequence of namespace-changing transactions after the checkpoint, such as creating/closing files, mkdir, rename, delete, chmod, or chown.

## What does not normally create an EditLog transaction?
- `hdfs dfs -ls /data/abc.txt` — read-only listing.
- `hdfs dfs -get /user/hive/db/abc.txt .` — reads HDFS data to local storage.
- `hdfs dfsadmin -report` — reports cluster state.

## What normally changes namespace state?
- `hdfs dfs -put local.csv /user/hive/db/` — creates destination namespace/file state and block associations.
- `hdfs dfs -mkdir /data/new` — creates directory metadata.
- `hdfs dfs -rm /data/old.csv` — removes namespace entry.
- `hdfs dfs -chmod 640 /data/file` — changes metadata.

Running `-ls` five times does not create five EditLog transactions merely because it ran five times. FSImage is not rewritten after each command. Checkpointing merges the prior image and applicable edits to create a newer image.

## Can FSImage be read as text?
Not directly as a normal text file; it is a structured/binary format. Use the Offline Image Viewer, with syntax verified for your Hadoop version, for example:
```bash
hdfs oiv -i /path/to/fsimage -o /tmp/fsimage.xml -p XML
```
The offline edits viewer (`hdfs oev` in supported versions) can inspect edit segments. Never modify NameNode metadata files manually. XML output can be large and sensitive.

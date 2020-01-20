# Exporting `export/verified/csv`

as of writing, exporting a csv from the webapp is expensive. Here is a
sql query that'll do it all in the database:

```sql
SELECT url as Image,
       center as Center,
       CONCAT_WS(',',
         IF(wall1 = 0, '1', NULL),
         IF(wall2 = 0, '2', NULL),
         IF(wall3 = 0, '3', NULL),
         IF(wall4 = 0, '4', NULL),
         IF(wall5 = 0, '5', NULL),
         IF(wall6 = 0, '6', NULL)
         ) as Openings,
       link1 as Link1,
       link2 as Link2,
       link3 as Link3,
       link4 as Link4,
       link5 as Link5,
       link6 as Link6,
       confidence as Confidence,
       datahash as 'Transcription hash',
       transCount as 'Transcription count',
       IF(rotatedCount, 'True', 'False') as 'Incorrect Rotation Flag'

--  INTO OUTFILE '/home/cu/Development/puzzlepieces/test.csv'
--  FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
--  LINES TERMINATED BY '\n'

  FROM collector_puzzlepiece
  INNER JOIN collector_confidentsolution ON collector_puzzlepiece.id = collector_confidentsolution.puzzlePiece_id
  INNER JOIN collector_rotatedimage ON collector_puzzlepiece.id = collector_rotatedimage.puzzlePiece_id
```

If the mysql server is run without the 'secure-file-priv' option, you
can uncomment the `INTO OUTFILE` lines and mysql itself will create a
csv

# splite3-analysis-DAND-P3
the original code with the error of "memory error" to the code part of "create table ways_nodes"
```
import sqlite3
import csv
from pprint import pprint

#connect to the database
sqlite_file= 'osmSF.db'
conn = sqlite3.connect(sqlite_file)

#create a cursor object
cur=conn.cursor()

# create the table
# drop the table if existed
cur.execute('DROP TABLE IF EXISTS nodes')
conn.commit()
# create table
cur.execute('''create table nodes(id INTEGER, lat REAL, lon REAL, user TEXT, uid INTEGER, version INTEGER, changeset INTEGER, timestamp DATE)''')
conn.commit()
# read in data
with open('nodes.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['lat'],i['lon'],i['user'],i['uid'],i['version'],i['changeset'],i['timestamp'])for i in dr]
#insert the data
cur.executemany('INSERT INTO nodes(id, lat, lon, user, uid, version, changeset, timestamp) VALUES (?,?,?,?,?,?,?,?);', to_db)
conn.commit()

---------------------------------------------------------------------------
MemoryError                               Traceback (most recent call last)
<ipython-input-47-da28f2fc2ee6> in <module>()
      9 with open('nodes.csv','rb') as fin:
     10     dr = csv.DictReader(fin)
---> 11     to_db =[(i['id'],i['lat'],i['lon'],i['user'],i['uid'],i['version'],i['changeset'],i['timestamp'])for i in dr]
     12 #insert the data
     13 cur.executemany('INSERT INTO nodes(id, lat, lon, user, uid, version, changeset, timestamp) VALUES (?,?,?,?,?,?,?,?);', to_db)

MemoryError: 


# create the other tables, nodes_tags
cur.execute('DROP TABLE IF EXISTS nodes_tags')
conn.commit()
cur.execute('''create table nodes_tags(id INTEGER, key TEXT, value TEXT, type TEXT)''')
conn.commit()
with open('nodes_tags.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['key'],i['value'].decode('utf-8'),i['type']) for i in dr]
cur.executemany('INSERT INTO nodes_tags(id, key, value, type) VALUES (?,?,?,?);', to_db)
conn.commit()    


# create the other tables ways
cur.execute('DROP TABLE IF EXISTS ways')
conn.commit()
cur.execute('''create table ways(id INTEGER, user TEXT, uid INTEGER, version INTEGER, changeset INTEGER, timestamp DATE)''')
conn.commit()
with open('ways.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['user'].decode('utf-8'),i['uid'],i['version'],i['changeset'],i['timestamp']) for i in dr]
cur.executemany('INSERT INTO ways(id, user, uid, version, changeset, timestamp) VALUES (?,?,?,?,?,?);', to_db)
conn.commit()    



# create the other tables, ways_tags
cur.execute('DROP TABLE IF EXISTS ways_tags')
conn.commit()
cur.execute('''create table ways_tags(id INTEGER, key TEXT, value TEXT, type TEXT)''')
conn.commit()
with open('ways_tags.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['key'],i['value'].decode('utf-8'),i['type']) for i in dr]
cur.executemany('INSERT INTO ways_tags(id, key, value, type) VALUES (?,?,?,?);', to_db)
conn.commit()    



# create the other tables, ways_nodes
cur.execute('DROP TABLE IF EXISTS ways_nodes')
conn.commit()
cur.execute('''create table ways_nodes(id INTEGER, node_id INTEGER, position INTEGER)''')
conn.commit()
with open('ways_nodes.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['node_id'],i['position']) for i in dr]
cur.executemany('INSERT INTO way_nodes(id, node_id, position) VALUES (?,?,?);', to_db)
conn.commit()    

---------------------------------------------------------------------------
MemoryError                               Traceback (most recent call last)
<ipython-input-49-33e6f70925eb> in <module>()
      6 with open('ways_nodes.csv','rb') as fin:
      7     dr = csv.DictReader(fin)
----> 8     to_db =[(i['id'],i['node_id'],i['position']) for i in dr]
      9 cur.executemany('INSERT INTO way_nodes(id, node_id, position) VALUES (?,?,?);', to_db)
     10 conn.commit()

MemoryError: 


I run the code for the 2nd try, the code of the table ways_nodes, a different kind of error happened.  
# create the other tables, ways_nodes
cur.execute('DROP TABLE IF EXISTS ways_nodes')conn.commit()
cur.execute('''create table ways_nodes(id INTEGER, node_id INTEGER, position INTEGER)''')
conn.commit()
with open('ways_nodes.csv','rb') as fin:
    dr = csv.DictReader(fin)
    to_db =[(i['id'],i['node_id'],i['position']) for i in dr]
cur.executemany('INSERT INTO way_nodes(id, node_id, position) VALUES (?,?,?);', to_db)
conn.commit()    

OperationalError                          Traceback (most recent call last)
<ipython-input-17-33e6f70925eb> in <module>()
      7     dr = csv.DictReader(fin)
      8     to_db =[(i['id'],i['node_id'],i['position']) for i in dr]
----> 9 cur.executemany('INSERT INTO way_nodes(id, node_id, position) VALUES (?,?,?);', to_db)
     10 conn.commit()

OperationalError: no such table: way_nodes


#check the data imported correctly
cur.execute('SELECT * FROM nodes_tags')
all_rows = cur.fetchall()
print('1):')
pprint(all_rows)

## no resultes after this code, even not print of 1):

  File "<ipython-input-32-93e44d98b3b2>", line 1
    @check the data imported correctly
             ^
SyntaxError: invalid syntax


cur.execute('SELECT count(*) FROM nodes_tags WHERE id> 10000')

<sqlite3.Cursor at 0x6871c1e0>

python分块读取大文件数据, 用StopIteration异常终止读文件



```python
def load_file_by_chunk(fname, chunk_size=100000):
    reader = pd.read_csv(fname, header=0, iterator=True)
    chunks = []
    loop = True
    while loop:
        try:
            chunk = reader.get_chunk(chunk_size)[['column1', 'column2', ...]]
            chunks.append(chunk)
        except StopIteration:
            loop = False
            print('Iteration is stopped')
            
	df_ac = pd.concat(chunks, ignore_index=True)
    return df_ac
```


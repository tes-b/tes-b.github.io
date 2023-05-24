---
published: True
title:  "[python] csv 파일 parquet 로 변환"
categories: [python, hadoop]
tag: [python, csv, parquet, hadoop]
---

## Apache Parquet

하둡에서 열(column)기반으로 데이터를 저장하는 포멧을 말한다.  
행(row) 기반 저장방식보다 압축률이 높고 처리속도가 빠르다.  
특히 CSV 파일과 비교하면 비약적적인 저장공간 절약과 처리속도 향상이 있다.  

## CSV -> Parquet

```py
# 파일 읽기
csv_file = "./train.csv"
parquet_file = "./train.parquet"
chunksize = 100_000
csv_stream = pd.read_csv(csv_file, sep=',', chunksize=chunksize, low_memory=False)
```

```py
for i, chunk in enumerate(csv_stream):
    print("Chunk", i)
    
    if i == 0:
        # 첫번째 청크에서 CSV파일 스키마 추출
        parquet_schema = pa.Table.from_pandas(df=chunk).schema
        print(parquet_schema)
        # Parquet 파일 열기
        parquet_writer = pq.ParquetWriter(parquet_file, parquet_schema, compression='snappy')
    # CSV 청크를 Parquet 파일에 쓰기
    table = pa.Table.from_pandas(chunk, schema=parquet_schema)
    parquet_writer.write_table(table)

parquet_writer.close()
```

## Error 

내 경우 파일을 변환하는 중 다음과 같은 에러를 만났다.  
```
ArrowTypeError: ('Expected a string or bytes dtype, got uint64', 'Conversion failed for column fullVisitorId with type uint64')'
```
컬럼 타입이 맞지 않아 발생한 오류였기에 변환전에 타입을 수정하고 변환을 진행했다.
```py
for i, chunk in enumerate(csv_stream):
    print("Chunk", i)

    # 컬럼 타입 변환시 에러 발생해 컬럼 타입 수정
    chunk['fullVisitorId'] = chunk['fullVisitorId'].astype(str)
    
    if i == 0:
        # 첫번째 청크에서 CSV파일 스키마 추출
        parquet_schema = pa.Table.from_pandas(df=chunk).schema
        print(parquet_schema)
        # Parquet 파일 열기
        parquet_writer = pq.ParquetWriter(parquet_file, parquet_schema, compression='snappy')
    # CSV 청크를 Parquet 파일에 쓰기
    table = pa.Table.from_pandas(chunk, schema=parquet_schema)
    parquet_writer.write_table(table)

parquet_writer.close()
```

## Parquet -> pandas Dataframe
```py
parquet_file = './train.parquet'
parquet_reader = pq.ParquetFile(parquet_file)

# 파일 내 그룹(청크)의 행 
num_row_groups = parquet_reader.num_row_groups

for i in range(num_row_groups):
    # 한줄 씩 읽어 판다스 데이터 데이터프레임으로 변환
    df = parquet_reader.read_row_group(i).to_pandas()
```


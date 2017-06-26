--- 
 
# required metadata 
title: "Manage objects in ODBC Data Sources" 
description: "Store/Retrieve objects to/from ODBC data sources. The APIs are modelled" 
keywords: "M, I, S, S, I, N, G,  , K, E, Y, W, O, R, D, S" 
author: "Microsoft Corporation Microsoft Technical Support" 
manager: "" 
ms.date: "06/26/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_list_keys



```
revoscalepy.functions.RxListKeys.rx_list_keys(src: revoscalepy.datasource.RxOdbcData.RxOdbcData, key: str = None, version: str = None, key_name: str = ‘id’, version_name: str = ‘version’)
```




## Description

Store/Retrieve objects to/from ODBC data sources. The APIs are modelled
after a simple key value store.


## Details

rx_write_object stores an object into the ODBC data source. The object
is identified by a key, and optionally, by a version (key+version). By
default, the object is serialized and compressed. Returns True if
successful.

rx_read_object loads an object from the ODBC data source (or from a
raw) decompressing and unserializing it (by default) in the process.
Returns the object. If the data source parameter defines a query, the
key and the version parameters are ignored.

rx_delete_object deletes an object from the ODBC data source. If there
are multiple objects identified by the key/version combination, all are
deleted.

rx_list_keys enumerates all keys or versions for a given key, depending
on the parameters. When key is None, the function enumerates all unique
keys in the table. Otherwise, it enumerates all versions for the given
key. Returns a single column data frame.

The key and the version column should be of some SQL character type
(CHAR, VARCHAR, NVARCHAR, etc) supported by the data source. The value
column should be a binary type (VARBINARY for instance). Some
conversions to other types might work, however, they are dependant on
the ODBC driver and on the underlying package functions.


## Parameters


### key

a character string identifying the object. The intended use is
for the key+version to be unique.


### value

the object being stored into the data source.


### version

None or a character string which carries the version of the
object. Combined with key identifies the object.


### key_name

character string specifying the column name for the key in
the underlying table.


### value_name

character string specifying the column name for the
objects in the underlying table.


### version_name

character string specifying the column name for the
version in the underlying table.


### serialize

logical value. Dictates whether the object is to be
serialized. Only raw values are supported if serialization is off.


### compress

character string defining the compression algorithm to use
for memCompress.


### deserialize

logical value. Defines whether the object is to be
de-serialized.


### decompress

character string defining the compression algorithm to
use for memDecompress.


### overwrite

logical value. If True, rx_write_object first removes the
key (or the key+version combination) before writing the new value. Even
when overwrite is False, rx_write_object may still succeed if there is no
database constraint (or index) enforcing uniqueness.


### all

logical value. True to remove all objects from the data source.
If True, the ‘key’ parameter is ignored.


## Returns

rx_read_object returns an object. rx_write_object and rx_delete_object
return logical, True on success. rx_list_keys returns a single column
data frame containing strings.


## Author

Microsoft Corporation [Microsoft Technical Support](https://go.microsoft.com/fwlink/?LinkID=698556&clcid=0x409.md)


## See also


## Example



```
from pandas import DataFrame
from numpy import random
from revoscalepy import RxOdbcData, rx_write_object, rx_read_object, rx_list_keys, rx_delete_object
connection_string = 'Driver=SQL Server;Server=.;Database=RevoTestDb;Trusted_Connection=True;'
dest = RxOdbcData(connection_string, table = "dataframe")

df = DataFrame(random.randn(10, 5))

status = rx_write_object(dest, key = "myDf", value = df)

read_obj = rx_read_object(dest, key = "myDf")

keys = rx_list_keys(dest)

rx_delete_object(dest, key = "myDf")
```

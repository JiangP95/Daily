记一次mysql limit要注意的问题

select *  from table ORDER BY DataChange_LastTime ASC limit 95, 100

最开始几条数据

| id   | uid  | DataChange_LastTime     |
| ---- | ---- | ----------------------- |
| 175  | 31   | 2019-03-06 13:48:02.660 |
| 174  | 96   | 2019-03-06 13:48:02.661 |
| 164  | 00   | 2019-03-06 13:48:02.662 |
| 166  | 44   | 2019-03-06 13:48:02.667 |
| 165  | 05   | 2019-03-06 13:48:02.669 |
| 168  | 34   | 2019-03-06 13:48:02.669 |
| 169  | 16   | 2019-03-06 13:48:02.671 |
| 167  | 88   | 2019-03-06 13:48:02.857 |

select *  from table ORDER BY DataChange_LastTime ASC limit 0, 100

最后几条数据

| id   | uid  | DataChange_LastTime     |
| ---- | ---- | ----------------------- |
| 164  | 00   | 2019-03-06 13:48:02.662 |
| 166  | 44   | 2019-03-06 13:48:02.667 |
| 168  | 34   | 2019-03-06 13:48:02.669 |

select *  from table ORDER BY DataChange_LastTime ASC limit 100, 100

最开始几条数据

| id   | uid  | DataChange_LastTime     |
| ---- | ---- | ----------------------- |
| 168  | 34   | 2019-03-06 13:48:02.669 |
| 169  | 16   | 2019-03-06 13:48:02.671 |
| 167  | 88   | 2019-03-06 13:48:02.857 |
| 44   | 09   | 2019-03-06 15:49:28.074 |

由于 05与34时间戳一样，两次的排序并不一致，导致limit后的结果丢失了一条

可以在查询语句中再加个排序比对 id ASC

select *  from bbzmbrtripcoinsshard01db.mbr_coins_accountssharding02 ORDER BY DataChange_LastTime ASC, id ASC limit 100, 100

用limit时的排序方式最好是唯一值比对
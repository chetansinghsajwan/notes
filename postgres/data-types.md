# SQL Data Types

Postgres has builtin data types but users can also add new types to PostgreSQL using the [CREATE TYPE](https://www.postgresql.org/docs/current/sql-createtype.html "CREATE TYPE") command.

Following are the builtin types postgres has:


- `bigint`
- `int8`
- signed eight-byte integer                                          |

- `bigserial`
- `serial8`
- autoincrementing eight-byte integer                                |

- ``bit [ (_`n`_) ]``
- 
- fixed-length bit string                                            |

- ``bit varying [ (_`n`_) ]``
- ``varbit [ (_`n`_) ]``
- variable-length bit string                                         |

- `boolean`
- `bool`
- logical Boolean (true/false)                                       |

- `box`
- 
- rectangular box on a plane                                         |

- `bytea`
- 
- binary data (“byte array”)                                         |

- ``character [ (_`n`_) ]``
- ``char [ (_`n`_) ]``
- fixed-length character string                                      |

- ``character varying [ (_`n`_) ]``
- ``varchar [ (_`n`_) ]``
- variable-length character string                                   |

- `cidr`
- 
- IPv4 or IPv6 network address                                       |

- `circle`
- 
- circle on a plane                                                  |

- `date`
- 
- calendar date (year, month, day)                                   |

- `double precision`
- `float8`
- double precision floating-point number (8 bytes)                   |

- `inet`
- 
- IPv4 or IPv6 host address                                          |

- `integer`
- `int`, `int4`
- signed four-byte integer                                           |

- ``interval [ _`fields`_ ] [ (_`p`_) ]``
- 
- time span                                                          |

- `json`
- 
- textual JSON data                                                  |

- `jsonb`
- 
- binary JSON data, decomposed                                       |

- `line`
- 
- infinite line on a plane                                           |

- `lseg`
- 
- line segment on a plane                                            |

- `macaddr`
- 
- MAC (Media Access Control) address                                 |

- `macaddr8`
- 
- MAC (Media Access Control) address (EUI-64 format)                 |

- `money`
- 
- currency amount                                                    |

- ``numeric [ (_`p`_, _`s`_) ]``
- ``decimal [ (_`p`_, _`s`_) ]``
- exact numeric of selectable precision                              |

- `path`
- 
- geometric path on a plane                                          |

- `pg_lsn`
- 
- PostgreSQL Log Sequence Number                                     |

- `pg_snapshot`
- 
- user-level transaction ID snapshot                                 |

- `point`
- 
- geometric point on a plane                                         |

- `polygon`
- 
- closed geometric path on a plane                                   |

- `real`
- `float4`
- single precision floating-point number (4 bytes)                   |

- `smallint`
- `int2`
- signed two-byte integer                                            |

- `smallserial`
- `serial2`
- autoincrementing two-byte integer                                  |

- `serial`
- `serial4`
- autoincrementing four-byte integer                                 |

- `text`
- 
- variable-length character string                                   |

- ``time [ (_`p`_) ] [ without time zone ]``
- 
- time of day (no time zone)                                         |

- ``time [ (_`p`_) ] with time zone``
- `timetz`
- time of day, including time zone                                   |

- ``timestamp [ (_`p`_) ] [ without time zone ]``
- 
- date and time (no time zone)                                       |

- ``timestamp [ (_`p`_) ] with time zone``
- `timestamptz`
- date and time, including time zone                                 |

- `tsquery`
- 
- text search query                                                  |

- `tsvector`
- 
- text search document                                               |

- `txid_snapshot`
- 
- user-level transaction ID snapshot (deprecated; see `pg_snapshot`) |

- `uuid`
- 
- universally unique identifier                                      |

- `xml`
- 
- XML data                                                           |

## References

- https://www.postgresql.org/docs/current/datatype.html

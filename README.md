**Status**

![CI](https://github.com/fishi0x01/libdb-rs/workflows/CI/badge.svg)
[![crates.io badge](https://img.shields.io/crates/v/libdb.svg)](https://crates.io/crates/libdb)
[![docs.rs badge](https://docs.rs/libdb/badge.svg)](https://docs.rs/libdb)

# libdb-rs

Statically linked rust bindings for Berkeley DB.

This is a humble fork from [jesterpm's](https://github.com/jesterpm/libdb-rs) libdb-rs.

## Features

`v4_8` uses bindings for Berkeley DB 4.8.x.

`v5_3` uses bindings for Berkeley DB 5.3.x.

By default, Berkeley DB 5.3.x is used. 

## Example

```rust
extern crate libdb;

let env = libdb::EnvironmentBuilder::new()
    .flags(libdb::DB_CREATE | libdb::DB_RECOVER | libdb::DB_INIT_TXN | libdb::DB_INIT_MPOOL)
    .open()
    .unwrap();

let txn = env.txn(None, libdb::DB_NONE).unwrap();

let db = libdb::DatabaseBuilder::new()
    .environment(&env)
    .transaction(&txn)
    .db_type(libdb::DbType::BTree)
    .flags(libdb::DB_CREATE)
    .open()
    .unwrap();

txn.commit(libdb::CommitType::Inherit).expect("Commit failed!");

let mut key   = String::from("key").into_bytes();
let mut value = String::from("value").into_bytes();
db.put(None, key.as_mut_slice(), value.as_mut_slice(), libdb::DB_NONE).expect("Put failed!");

let result = db.get(None, key.as_mut_slice(), libdb::DB_NONE).unwrap();
println!("{:?}", result);
```

## crev

This crate has its author's [crev review](https://github.com/fishi0x01/crev-proofs).

It is recommended to always use [cargo-crev](https://github.com/crev-dev/cargo-crev)
to verify the trustworthiness of each of your dependencies, including this one.

## Berkeley DB licensing notice

Website: http://www.oracle.com/database/berkeley-db/

License: Sleepycat

Description:
```
The Berkeley Database (Berkeley DB) is a programmatic toolkit that
provides embedded database support for both traditional and
client/server applications. The Berkeley DB includes B+tree, Extended
Linear Hashing, Fixed and Variable-length record access methods,
transactions, locking, logging, shared memory caching, and database
recovery. The Berkeley DB supports C, C++, Java, and Perl APIs. It is
used by many applications, including Python and Perl, so this should
be installed on all systems.
```

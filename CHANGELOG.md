# Changelog
All notable changes to this project will be documented in this file.
Documented changes are those specific to Splice. 
We use `fakeredis` version `1.5.0` as the base.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- We import `django` package to introduce taint-aware data types.

### Changed
- Dependencies specified in `setup.py`:
    - We use our modified, Splice-aware 
      [`redis-py`](https://github.com/michael-hahn/redis-py), instead of the
      original package. See its `CHANGELOG` for more information.
    - We include our [`django`](https://github.com/michael-hahn/django-1/tree/splice)
      package to introduce taint-aware Splice data types.
      
- Modify `fakeredis`'s `Int` class to propagate taint properly: 
  `decode` method returns taint-aware `int` data.
  
- Modify `fakeredis`'s `Float` class to propagate taint properly:
    - `decode` method returns taint-aware `float` data.
    - `encode` method return taint-aware `str` data.
    
- `_parse_commands` method in the `FakeSocket` class uses taint-aware `bytes`
  data type instead of bytes literals.
  
- The arguments to the `@command` decorator of the following commands
  (defined in the `FakeSocket` class) are modified to use taint-aware data 
  types instead of built-in data types (more commands are being modified as we go):
  - `get`
  - `set_`
  - `lpush`
  - `rpush`
  - `sadd`
  - `zadd`
    
  Note that *not all* commands need to be modified to properly propagate taints.
  Some commands' decorator can stay as-is.
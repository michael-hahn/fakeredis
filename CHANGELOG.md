# Changelog
All notable changes to this project will be documented in this file.
Documented changes are those specific to Splice. 
We use `fakeredis` version `1.5.0` as the base.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- We import [`django`](https://github.com/michael-hahn/django-1/tree/splice)
  package to introduce taint-aware data types.
- We add `splice.py` that introduces an `Encoder`, which works similarly as the
  one in [`redis-py`](https://github.com/michael-hahn/redis-py).

### Changed
- Dependencies specified in `setup.py`:
    - We use our modified, Splice-aware 
      [`redis-py`](https://github.com/michael-hahn/redis-py), instead of the
      original package. See its `CHANGELOG` for more information.
      
- (Possibly outdated) Modify `fakeredis`'s `Int` class to propagate taint properly: 
  `decode` method returns taint-aware `int` data.
  
- (Possibly outdated) Modify `fakeredis`'s `Float` class to propagate taint properly:
    - `decode` method returns taint-aware `float` data.
    - `encode` method return taint-aware `str` data.
  
- `FakeServer` contains an encoder, a compressor, and a serializer as its attributes.

- In `FaksSocket`:
  - `_apply_withscores()` method must encode scores using the compressor and the
    serializer to preserve taints.
  - `zadd()` method must also decode scores using the compressor and the serializer.
# Python 3.10 / JSONSchema Bug Reproduction Container

## Issues

- https://github.com/poljar/weechat-matrix/issues/293
  - There is a bug with both Fedora Linux (35) and Void Linux (latest) with regards to the python script execution with weechat-matrix when it calls a python script which includes `jsonschema._types.TypeChecker` 
- https://github.com/GrahamDumpleton/mod_wsgi/issues/729
  - There is a bug with Fedora Linux (35) with `mod-wsgi` when it calls a python script which includes `jsonschema._types.TypeChecker`

## Progress

So far, this is the information I have:

- Since this occurs on Void Linux, and Fedora Linux, this is *not likely* a Fedora packaging issue.
- This is *not* reproduceable directly in the REPL
  - Leads me to believe that this is specific to when the libraries are imported using `cpython`
    - This comment supports this argument: https://github.com/GrahamDumpleton/mod_wsgi/issues/729#issuecomment-968665948
- Managed to add https://weechat.org/scripts/source/pybuffer.py.html/, but didn't have much success getting an `ipdb` debugger through this debugger.
  - Did, however verify that this line causes the issue: `from jsonschema._types import TypeChecker`
- The `TypeChecker` class
  - Uses an `attr.s` decorator, could py3.10 have broken this as it relates to cpython?
    - More likely considering the error mentioning it wasn't expecting an argument
  - Uses `pyrsistent.pmap`, could py3.10 have broken this as it relates to cpython?

## Reproduce

Pull/run docker image:

```bash
docker pull docker.io/omnidapps/weechat-matrix-293:latest
docker run -ti --rm omnidapps/weechat-matrix-293

cd ~/.weechat/python
weechat
```

In weechat:

```
/script load matrix.py
```

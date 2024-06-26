
# Python snippet cookbook

## table of contents

- [subprocess - capture output](#subprocess---capture-output)
- [cProfile - profile a process](#cProfile)
- [json - dict to json](#dict2json)
- [debugging](#debugging)


## snippets

### subprocess - capture output

```python3
subprocess.run(["ls -l"], shell=True, capture_output=True, text=True)
```

### cProfile

```sh
pip install cprofilev  # cProfile wrapper, allows to view output in the browser
cprofilev ~/.local/bin/pytest <pytest-args> tests/some_test.py::TestClass::test_method
```

### dict2json

```python3
import json
the_dict = {'a': 42}
with open('file.json', 'w+') as f:
  f.write(json.dumps(the_dict))
```

### debugging

```python3
breakpoint()  # set breakpoint >=3.7
import pdb; pdb.set_trace()  # < 3.7
```

```sh
export PYTHONBREAKPOINT=IPython.core.debugger.set_trace  # select a custom debugger
export PYTHONBREAKPOINT=IPython.embed  # start python session in the breakpoint
export PYTHONBREAKPOINT=0  # disable all breakpoints
python3 main.py
```

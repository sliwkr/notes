
# Python snippet cookbook

## table of contents

- [subprocess - capture output](#subprocess---capture-output)
- [cProfile - profile a process](#cProfile)
- [json - dict to json](#dict2json)


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

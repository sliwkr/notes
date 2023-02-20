
# Python snippet cookbook

## table of contents

- [subprocess - capture output](#subprocess---capture-output)

## snippets

### subprocess - capture output

```python3
subprocess.run(["ls -l"], shell=True, capture_output=True, text=True)
```

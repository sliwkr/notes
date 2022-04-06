# Pytest snippet cookbook

## table of contents

- [fail fast](#fail-fast)
- [logging](#logging)

## snippets

### fail fast

```sh
pytest -x  # stop on first failure
pytest --maxfail=2  # stop after 2 failures
```

### logging

https://docs.pytest.org/en/latest/how-to/logging.html#logging
https://docs.pytest.org/en/latest/reference/reference.html#caplog

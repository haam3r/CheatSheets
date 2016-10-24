# Useful Jinja stuff you can do

## Enhancing $PATH in a cmd state

```Jinja
{% set current_path = salt['environ.get']('PATH', '/bin:/usr/bin') %}

mycommand:
  cmd.run:
    - name: ls -l /
    - env:
      - PATH: {{ [current_path, '/my/special/bin']|join(':') }}
```

## Parsing grains values

```Jinja
{% set cluster = salt['grains.get']('domain').rsplit('.', 2)[0] %}
```

# 📚️ Platform docs

## 🏗️ Setup
```
pip install mkdocs
pip install mkdocs-material
```

or on ubuntu (if not in path)

```
python3 -m pip install mkdocs
python3 -m pip install mkdocs-material
```
or on windows (if not in path)

```
py -m pip install mkdocs
py -m pip install mkdocs-material
```

## ⚙️ Run the doc
`mkdocs serve`

or on ubuntu (if not in path)

`python3 -m mkdocs serve` 

or on windows (if not in path)

`py -m mkdocs serve`

and visit `http://localhost:8000`.

## 🚀 Deploy the doc

`mkdocs gh-deploy`

or on ubuntu (if not in path)

`python3 -m mkdocs gh-deploy`

or on windows (if not in path)

`py -m mkdocs gh-deploy`

The doc is available at https://keninette.github.io/cheatsheets/ after a short period.

### Build the doc locally (without deploying)
`mkdocs build --site-dir public`

or on ubuntu (if not in path)

`python3 -m mkdocs build --site-dir public`

or on windows (if not in path)

`py -m mkdocs build --site-dir public`

# Flask
Flask is a web framework

### Installation
```
pip3 install flask
```

### Directory structure is important:
```bash

mkdir flaskapp && cd flaskapp

flaskapp/
   |
   +-templates/
   |     |
   |     +--index.html
   |     +--home.html
   |     +--about.html
   |
   +--flaskapp.py
```

- `flaskapp.py`: The python code to route URLs to objects and templates
- `templates/`: The folder to store the Jinja and HTML templates. Flask checks for files in the `/templates/` directory by default.
- `templates/*.html`: The Jinja and HTML templates

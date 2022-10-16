# How to start a new python project on windows

cd into your project folder

create a virtual environment:
```
python -m venv venv
```
activate the virtual environment in cmd:
```
venv\Scripts\activate.bat
```

now you can install the packages

in order to use the venv interpreter in vscode, open view/command palette in vscode and select the venv interpreter

# If you use github in the project

create a .gitignore file in the project folder:
```
# Virtual Environment
venv/
```

# Save a list of the requirements as .txt

```
pip freeze > requirements.txt
```
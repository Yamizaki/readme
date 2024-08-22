# Cloud Run

## ETL Job Service

We are using fastapi to create a REST API that will process the ETL jobs. The API is deployed to Cloud Run.

<!-- ![ETL Job Service](../images/fastapi-docs.png) -->
<img src="../images/fastapi-docs.png" width=50% height=50%>

## Local Development Setup


### Make sure you are in the global directory **etl-jobs**!

1. Install [pyenv](https://github.com/pyenv/pyenv-installer).


    **Most common errors** 
             Forgetting to add environment variables in your shell config (bash/zsh)
            try with (Dond't forget to change your shell's name):
    ```
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
    echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(pyenv init -)"' >> ~/.bashrc
    ```     
    **Install necessary libraries**
    You are probably missing important libraries, you can check the libraries [here](https://github.com/pyenv/pyenv/wiki/Common-build-problems).
    
2. Install python 3.10.13.
    ```
    pyenv install 3.10.13
    ```
3. Create the environment.

    ```bash
    SERVICE_HOME=$(pwd)/cloud_run/etl
    pyenv shell 3.10.13
    rm -rf $SERVICE_HOME/.venv
    python -m venv $SERVICE_HOME/.venv
    source $SERVICE_HOME/.venv/bin/activate && pip install --upgrade pip
    pip install -r $SERVICE_HOME/requirements.txt
    ```

4. Run the service. You can also use the vscode debugger. 

    ```bash
    cd $SERVICE_HOME && uvicorn main:app --reload --port 8000 --env-file "../.env"
    ```

5. Run the tests.

    ```bash
    SERVICE_HOME=$(pwd)/cloud_run/etl
    cd $SERVICE_HOME && pytest -vv
    ```

6. Run the linter.

    ```bash
    SERVICE_HOME=$(pwd)/cloud_run/etl
    cd $SERVICE_HOME
    black .
    isort .
    flake8 .
    mypy .
    ```

# Guidelines for development

1. Apply SOLID principles as much as possible. Use chatgpt as a reference.
2. Create at least unit tests for DAOs, services or utils.
3. Connections to services should handle retries mechanisms.
4. Use the logger to log important information.
5. Use the linter to check the code before commiting.

# Boston House Price Prediction

This project focuses on building and deploying a machine learning model to predict house prices in Boston using a Linear Regression model. It covers data exploration, model training, web application development with Flask, and deployment to Heroku using two methods: basic deployment and a more advanced CI/CD approach with Docker and GitHub Actions.

## Project Overview

The project follows these main steps:

1. **Data Understanding and Preparation:**
    *   Load the Boston Housing Dataset.
    *   Perform Exploratory Data Analysis (EDA) to understand feature relationships and distributions.
    *   Preprocess the data by splitting it into training/testing sets and standardizing features using `StandardScaler`.

2. **Model Building and Training:**
    *   Train a Linear Regression model using the training data.
    *   Evaluate the model's performance using metrics like MAE, MSE, RMSE, R-squared, and adjusted R-squared.
    *   Save the trained model and scaler to pickle files (`regression_model.pkl` and `scaling.pkl`).

3. **Web Application Development (Flask):**
    *   Create a Flask web application with the following routes:
        *   `/`: Home route that displays the main page with an input form (`home.html`).
        *   `/predict_api`: API route for testing with tools like Postman (accepts JSON input and returns JSON prediction).
        *   `/predict`: Route that handles form submissions, predicts the house price, and displays the result on `home.html`.

4. **Deployment to Heroku:**
    *   **Basic Deployment:** Deploy the application directly from the GitHub repository to Heroku.
    *   **Deployment with Docker and GitHub Actions:**
        *   Create a `Dockerfile` to containerize the application.
        *   Set up a GitHub Actions workflow (`main.yaml`) to automate the build, push, and release process to Heroku.
        *   Store Heroku credentials as secrets in the GitHub repository.

## Tools and Software Requirements

1. **GitHub Account:**
    *   Create a GitHub account to store the project code and manage deployments.
    *   [https://github.com/](https://github.com/)

2. **VS Code IDE:**
    *   Download and install Visual Studio Code for code editing and development.
    *   [https://code.visualstudio.com/](https://code.visualstudio.com/)

3. **Heroku Account:**
    *   Create a Heroku account to deploy the web application.
    *   [https://www.heroku.com/](https://www.heroku.com/)

4. **Git CLI:**
    *   Download and install Git CLI for version control and interacting with GitHub.
    *   [https://git-scm.com/downloads](https://git-scm.com/downloads)

5. **Postman (Optional):**
    *   Download and install Postman to test the API endpoints.
    *   [https://www.postman.com/downloads/](https://www.postman.com/downloads/)

6. **Docker Desktop (Optional but recommended):**
    *   For using the Docker deployment method, you will need to install Docker Desktop.
    *   [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

## Project Setup

1. **Create a GitHub Repository:**
    *   Create a new GitHub repository named `boston-house-pricing` (or a name of your choice).
    *   Add a `README.md` file.
    *   Add a `.gitignore` file (select the Python template).

2. **Clone the Repository:**
    *   Clone the repository to your local machine using:
        ```bash
        git clone <repository_url>
        ```

3. **Create a Virtual Environment:**
    *   Navigate to the cloned repository directory.
    *   Create a new conda environment:
        ```bash
        conda create -p venv python=3.7 -y
        ```
    *   Activate the environment:
        ```bash
        conda activate venv
        ```

4. **Install Dependencies:**
    *   Create a `requirements.txt` file with the following content:

        ```
        Flask
        scikit-learn
        pandas
        numpy
        matplotlib
        gunicorn
        ```
    *   Install the dependencies:
        ```bash
        pip install -r requirements.txt
        ```

## File Structure
content_copy
download
Use code with caution.
Markdown

boston-house-pricing/
├── .github/
│ └── workflows/
│ └── main.yaml # GitHub Actions workflow file
├── templates/
│ └── home.html # HTML template for the web app
├── app.py # Flask application code
├── regression_model.pkl # Pickled trained regression model
├── scaling.pkl # Pickled StandardScaler object
├── Dockerfile # Dockerfile for containerization
├── Procfile # Procfile for Heroku deployment
├── requirements.txt # Project dependencies
├── Boston_housing.ipynb # Jupyter Notebook for EDA and model training (Optional)
└── README.md # Project README file

## Running the Application Locally

1. **Navigate to the project directory:**
    ```bash
    cd boston-house-pricing
    ```

2. **Activate the virtual environment:**
    ```bash
    conda activate venv
    ```

3. **Run the Flask app:**
    ```bash
    python app.py
    ```

4. **Access the application in your browser:**
    *   Open `http://127.0.0.1:5000/` in your web browser.

## Deployment to Heroku

### Basic Deployment

1. **Log in to your Heroku account.**
2. **Create a new Heroku app:**
    *   Click "New" -> "Create new app".
    *   Give your app a name (e.g., `boston-house-pricing-yourname`).
3. **Choose "Connect to GitHub" as the deployment method.**
4. **Connect to your GitHub repository.**
5. **Select the `main` branch.**
6. **Click "Deploy Branch" to initiate a manual deployment.**
7. **Once deployed, click "View" to access the app.**

### Deployment with Docker and GitHub Actions

1. **Create a `Dockerfile`:** (See the "Dockerfile" section below for content).
2. **Create `.github/workflows/main.yaml`:** (See the "GitHub Actions Workflow" section below for content).
3. **Store Heroku credentials as GitHub secrets:**
    *   Go to your repository's "Settings" -> "Secrets".
    *   Add the following secrets:
        *   `HEROKU_API_KEY`: Your Heroku API key (found in your Heroku account settings).
        *   `HEROKU_EMAIL`: Your Heroku account email.
        *   `HEROKU_APP_NAME`: The name of your Heroku app.

4. **Commit the `Dockerfile` and `main.yaml` changes to your repository.**
5. **The commit will trigger the GitHub Actions workflow, which will build, push, and release the Docker container to Heroku.**

## Dockerfile

```dockerfile
FROM python:3.7

COPY . /app

WORKDIR /app

RUN pip install -r requirements.txt

EXPOSE $PORT

CMD ["gunicorn", "--workers", "4", "--bind", "0.0.0.0:$PORT", "app:app"]
content_copy
download
Use code with caution.
GitHub Actions Workflow (main.yaml)
name: Deploy to Heroku

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Build, Push and Release a Docker container to Heroku
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          dockerfile_directory: ./
          dockerfile_name: Dockerfile
          docker_options: "--no-cache"
          process_type: web
content_copy
download
Use code with caution.
Yaml
Important Notes:

The Jupyter Notebook (Boston_housing.ipynb) is optional and can be used for the initial EDA and model training steps.

Remember to replace placeholder values (like <repository_url>, app name, etc.) with your actual values.

You may need to adjust the commands in this readme if you make changes to your project's file structure or dependencies.

This detailed README provides a comprehensive guide to understanding, setting up, running, and deploying the Boston House Price Prediction project. Remember to refer to the video tutorial for visual guidance and further explanations.

This README provides a good starting point for your project. You can further customize it with more details about your specific implementation, findings, or any additional instructions you want to include. Remember to update it as your project evolves!
content_copy
download
Use code with caution.

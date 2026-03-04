# Fake Review Detection

A Django-based web application that uses machine learning to detect fake product reviews. The system classifies reviews as **Fake**, **Positive**, or **Negative** using an ensemble of three ML models.

## Features

- **User registration & login** for remote users
- **Product review submission** with real-time prediction (Fake / Positive / Negative)
- **Service provider dashboard** with:
  - View all registered remote users
  - Train and test ML models, with per-algorithm accuracy display
  - View all predicted reviews
  - Compute fake/positive/negative review ratios
  - Visualize results with multiple chart types
  - Download trained data as an Excel file

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Web framework | Django 3.x |
| Database | MySQL |
| ML / Data | scikit-learn, pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Templating | Django Templates (HTML/CSS) |

## Machine Learning Models

The application trains three classifiers on a bag-of-words representation (CountVectorizer) and combines them with a **VotingClassifier** ensemble:

1. **Multinomial Naive Bayes**
2. **Linear SVC (Support Vector Machine)**
3. **Logistic Regression**

Label mapping applied to the dataset:

| Sentiment value | Meaning |
|-----------------|---------|
| 3 | Fake (0) |
| 1 | Positive (1) |
| other | Negative (2) |

## Dataset

`Product_Reviews.csv` — columns used:

| Column | Description |
|--------|-------------|
| `Text` | Raw review text |
| `Sentiment` | Numeric sentiment label |
| `Text_ID` | Unique identifier (dropped before training) |
| `Product_Type` | Product category (dropped before training) |

## Project Structure

```
fake-review-detection/
├── detecting_fake_reviews/      # Django project settings & URL config
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── Remote_User/                 # App for end-users (registration, review prediction)
│   ├── models.py
│   └── views.py
├── Service_Provider/            # App for admin/service provider
│   └── views.py
├── Template/
│   └── htmls/
│       ├── RUser/               # Templates for remote users
│       └── SProvider/           # Templates for service provider
├── Product_Reviews.csv          # Training dataset
├── Tested_Results.csv           # Results exported after training
└── manage.py
```

## Prerequisites

- Python 3.7+
- MySQL server running locally
- pip

## Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/tejesh012/fake-review-detection.git
   cd fake-review-detection
   ```

2. **Create and activate a virtual environment**

   ```bash
   python -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   ```

3. **Install dependencies**

   ```bash
   pip install django mysqlclient pandas numpy scikit-learn matplotlib seaborn openpyxl xlwt
   ```

4. **Create the MySQL database**

   ```sql
   CREATE DATABASE detecting_fake_reviews;
   ```

5. **Configure database credentials**

   Open `detecting_fake_reviews/settings.py` and update the `DATABASES` section with your MySQL username and password:

   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'detecting_fake_reviews',
           'USER': 'your_mysql_user',
           'PASSWORD': 'your_mysql_password',
           'HOST': '127.0.0.1',
           'PORT': '3306',
       }
   }
   ```

6. **Apply database migrations**

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

7. **Run the development server**

   ```bash
   python manage.py runserver
   ```

   Visit `http://127.0.0.1:8000/` in your browser.

## Usage

### Remote User

1. Navigate to `http://127.0.0.1:8000/` and **register** a new account.
2. **Log in** with your credentials.
3. Go to **Product Review Prediction**, enter a review text, and click submit.
4. The system will classify the review as *Fake*, *Positive*, or *Negative*.

### Service Provider (Admin)

1. Navigate to `http://127.0.0.1:8000/serviceproviderlogin/`.
2. Log in with username **Admin** and password **Admin**.
3. Available actions:
   - **View Remote Users** — see all registered users
   - **Train & Test Datasets** — run ML training and view accuracy for each model
   - **View Predicted Reviews** — browse all submitted reviews and their predictions
   - **Find Review Type Ratio** — compute percentage of Fake / Positive / Negative reviews
   - **Charts** — visualise detection ratios and model accuracy
   - **Download Trained Data** — export results to an Excel file

## License

This project is provided for educational purposes.

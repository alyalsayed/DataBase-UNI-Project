# DataBase-UNI-Project

An e-commerce demonstration project that combines a Flutter storefront, a PHP/MySQL backend, and a Flask machine-learning service to flag suspicious activity and content.

> This is a university project and starting point for experimentation. Review and secure the code before running it with real data or exposing it to a network.

## Detection features

The Flask service provides predictions for:

- **Malicious URLs** — classifies URLs as benign, defacement, malware, or phishing.
- **Product/content moderation** — predicts whether submitted product content is fraudulent.
- **Transaction fraud** — labels a transaction fraudulent or non-fraudulent.
- **Bot detection** — classifies account behavior as human or bot.

The PHP backend connects storefront operations to the database and calls the Flask service for predictions. The Flutter application is the client interface.

## Project structure

```text
.
├── AiMLOps/Flask/       # Flask API, preprocessing, and model code
├── Backend/             # PHP backend
├── Designs/             # Project requirements, designs, and reports
├── Flutter/             # Flutter application
├── MigrateData/         # Data migration scripts and sample data
└── Testing/             # Test resources and requirements
```

## Prerequisites

Install the tools needed for the components you plan to run:

- Python 3 and pip
- Flutter SDK (for the client)
- PHP with MySQLi and cURL extensions
- MySQL

You will also need the database schema/data and any model assets required by the service. Check the files under `MigrateData/` and `AiMLOps/Flask/` and configure them for your environment; this repository does not provide a one-command deployment setup.

## Run the Flask prediction service

From the repository root:

```bash
cd AiMLOps/Flask
python -m venv .venv
```

Activate the virtual environment, then install dependencies and start the service:

```bash
# macOS/Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1

pip install -r requirements.txt
python app.py
```

The API listens on `http://127.0.0.1:5000` by default. The current development server enables Flask debug mode; do not expose it to the internet or use it as a production server.

### Prediction endpoints

All prediction routes use `POST` and accept JSON.

| Endpoint | Purpose | Example request body |
|---|---|---|
| `/externalUrl` | Classify a URL | `{"url":"https://example.com"}` |
| `/contentModeration` | Check product/content records | `{"data":[{"title":"Example product"}]}` |
| `/transaction` | Classify a transaction | `{"user_id":1,"signup_time":"2024-01-01","purchase_time":"2024-02-01","purchase_value":25,"device_id":"device-1","source":"Web","browser":"Chrome","sex":"unknown","age":30,"ip_address":"127.0.0.1","IP_country":"US"}` |
| `/detect_bots` | Classify one account record | A JSON object containing the account behavior fields expected by the bot model |
| `/detect_bots_batch` | Classify account records in a batch | A JSON array of account behavior objects |

Example URL request:

```bash
curl -X POST http://127.0.0.1:5000/externalUrl \
  -H 'Content-Type: application/json' \
  -d '{"url":"https://example.com"}'
```

For exact input fields and expected model assets, see `AiMLOps/Flask/app.py` and the corresponding modules under `AiMLOps/Flask/models/`.

## Configure the backend and database

Before using the PHP backend:

1. Create and populate a local MySQL database using the project’s migration/schema files.
2. Update the database connection settings and Flask service URLs in `Backend/ecommerce-workd.php` for your local environment.
3. Configure the Flutter app to use the address where your PHP backend is running.

**Security:** Do not commit real database passwords, API keys, or other secrets. Use local configuration or environment variables, and do not expose the development database or backend publicly without adding appropriate authentication and security controls.

## Run the Flutter app

```bash
cd Flutter
flutter pub get
flutter run
```

Select a connected emulator or device. The client also needs the backend URL configured for the environment where it is running.

## Testing

Testing notes and requirements are in `Testing/Testing_Requirements.md`. The documented Python test workflow uses `pytest`; some mobile tests require Appium and Appium Inspector, and test reports use Allure.

## Project documents

See `Designs/` for the requirements, high-level and low-level designs, testing document, and Power BI insights.

## Forking and contributions

This repository is a fork of [RamiRafee/DataBase-UNI-Project](https://github.com/RamiRafee/DataBase-UNI-Project). Contributions to this fork are welcome through issues and pull requests.

## License

No license is currently specified. Unless a license is added by the project owner, the code is not granted standard open-source reuse rights.

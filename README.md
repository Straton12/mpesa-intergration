# M-Pesa Integration API

This project provides a Django-based API for integrating M-Pesa payment services. It includes features for initializing transactions, sending money, and retrieving transaction status.

## Table of Contents

1. [Project Setup](#project-setup)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [API Endpoints](#api-endpoints)
5. [Usage Examples](#usage-examples)
6. [Error Handling](#error-handling)
7. [Security Considerations](#security-considerations)
8. [Testing](#testing)
9. [Contributing](#contributing)
10. [License](#license)

## Project Setup

To set up this project:

1. Clone the repository:
git clone https://github.com/yourusername/mpesa-integration-api.git cd mpesa-integration-api


2. Create a virtual environment:
python -m venv venv source venv/bin/activate # On Windows, use venv\Scripts\activate


3. Install dependencies:
pip install -r requirements.txt


## Installation

To install the API:

1. Ensure you have Django installed:
pip install django


2. Add `'mpesa_api'` to your `INSTALLED_APPS` in your Django settings file.

## Configuration

Configure the M-Pesa integration by adding the following to your Django settings:

python MPESA_API_KEY = 'YOUR_API_KEY' MPESA_SECRET_KEY = 'YOUR_SECRET_KEY' MPESA_SHORTCODE = 'YOUR_SHORTCODE'

Optional: Set default currency and transaction type
DEFAULT_CURRENCY = 'KES' DEFAULT_TRANSACTION_TYPE = 'paybill'


Replace `'YOUR_API_KEY'`, `'YOUR_SECRET_KEY'`, and `'YOUR_SHORTCODE'` with your actual M-Pesa credentials obtained from Safaricom.

## API Endpoints

The API provides the following endpoints:

### Initialize Transaction

POST `/api/initiate_transaction/`

Request body:
json { "amount": 100, "phone_number": "+254712345678", "description": "Test transaction" }


Response:
json { "transaction_id": "TXN123456789", "reference_code": "REF123456789", "status": "pending" }


### Send Money

POST `/api/send_money/`

Request body:
json { "amount": 500, "recipient_phone": "+254712345678", "description": "Sending money to customer" }


Response:
json { "transaction_id": "TXN987654321", "reference_code": "REF987654321", "status": "completed" }


### Check Transaction Status

GET `/api/check_status/?transaction_id=TXN123456789`

Response:
json { "transaction_id": "TXN123456789", "reference_code": "REF123456789", "status": "completed", "amount": 100, "timestamp": "2023-05-15T14:30:00Z" }


## Usage Examples

Here's how to use the API in your views:

python from rest_framework.views import APIView from rest_framework.response import Response from .models import Transaction from .serializers import TransactionSerializer

class InitiateTransactionView(APIView): def post(self, request): serializer = TransactionSerializer(data=request.data) if serializer.is_valid(): transaction = serializer.save() return Response({ 'transaction_id': transaction.id, 'reference_code': transaction.reference_code, 'status': transaction.status }) return Response(serializer.errors, status=400)

class CheckStatusView(APIView): def get(self, request, transaction_id): try: transaction = Transaction.objects.get(id=transaction_id) return Response(TransactionSerializer(transaction).data) except Transaction.DoesNotExist: return Response({'error': 'Transaction not found'}, status=404)


## Error Handling

The API handles common errors such as:

- Invalid phone numbers
- Insufficient funds
- Network issues
- Duplicate transactions

All errors are returned with appropriate HTTP status codes and error messages.

## Security Considerations

1. Never expose your M-Pesa API keys in client-side code.
2. Use HTTPS for all API requests.
3. Implement rate limiting to prevent abuse.
4. Validate and sanitize all user inputs before processing them.

## Testing

To run tests:

1. Install test dependencies:
pip install -r dev_requirements.txt


2. Run tests:
pytest


## Contributing

Contributions are welcome! Please fork the repository and submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
This README covers everything from setup to usage, including installation, configuration, API endpoints, usage examples, error handling, security considerations, testing, contributing guidelines, and licensing information. It should provide a comprehensive guide for developers working on or using the M-Pesa integration API.

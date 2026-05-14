# 📲 M-Pesa Daraja Payment Gateway Microservice

A production-ready microservice engineered to handle automated Safaricom Daraja API integrations safely and asynchronously. Handles STK Push triggers, Paybill validation/confirmation loops, and ledger syncs.

## 🛠️ Tech Stack & Architecture
- **Language & Framework:** Python 3.11+ | Flask / Django REST Framework
- **Task Queue & Deferral:** Celery | Redis (Prevents HTTP timeout issues on long-polling webhooks)
- **Database:** MySQL 8.0 (Relational transactional ledger structure)

## 🛡️ Security Implementation
1. **IPN Webhook Validation:** Strict verification of payload signatures against Safaricom public keys.
2. **Replay Attack Mitigation:** Unique transaction tracking IDs using Redis idempotency keys.
3. **Data Isolation:** Financial operations run on dedicated schemas with least-privilege database user permissions.

## 🚀 Step-by-Step Installation

1. **Clone the repository:**
   ```bash
   git clone github.com
   cd mpesa-daraja-gateway
   ```

2. **Configure Environment Variables:**
   Create a `.env` file in the root directory:
   ```env
   MPESA_CONSUMER_KEY=your_key_here
   MPESA_CONSUMER_SECRET=your_secret_here
   MPESA_INITIATOR_PASSWORD=your_password
   DATABASE_URL=mysql://user:pass@localhost/db
   REDIS_URL=redis://localhost:6373/0
   ```

3. **Install Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run Database Migrations:**
   ```bash
   python manage.py migrate # or flask db upgrade
   ```

5. **Start Worker & App:**
   ```bash
   celery -A tasks worker --loglevel=info &
   python app.py
   ```

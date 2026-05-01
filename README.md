<p align="center">
  <img src="https://marcbrainltd.com/images/pos-sys.jpg" alt="SwiftClaim Banner" width="100%" style="border-radius:10px;" />
</p>

<h1 align="center">SwiftClaim V3</h1>

<p align="center">
  <strong>Android SMS-to-Webhook automation for mobile money payments</strong><br/>
  Built by <a href="https://marcbrainltd.com">MarcBrain Ltd</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" />
  <img src="https://img.shields.io/badge/Version-3.0-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/License-Commercial-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Key-Required-orange?style=for-the-badge" />
</p>

---

## Table of Contents

- [What is SwiftClaim?](#-what-is-swiftclaim)
- [Real-World Use Case](#-real-world-use-case)
- [How It Works](#-how-it-works)
- [Features](#-features)
- [Installation](#-installation)
  - [Google Play Protect Warning](#google-play-protect-warning)
  - [App Key & Licensing](#app-key--licensing)
- [Permissions Required](#-permissions-required)
- [Configuration](#-configuration)
  - [Webhook URL Setup](#webhook-url-setup)
  - [SMS Pattern Settings](#sms-pattern-settings)
- [Foreground Service & Persistence](#-foreground-service--persistence)
- [Offline & Queue Behavior](#-offline--queue-behavior)
- [Webhook Payload Reference](#-webhook-payload-reference)
- [Webhook Integration Examples](#-webhook-integration-examples)
- [Retry Behavior](#-retry-behavior)
- [Security Tips](#-security-tips)
- [Version History](#-version-history)
- [Support & Contact](#-support--contact)

---

## 📲 What is SwiftClaim?

**SwiftClaim** is an Android app developed by **MarcBrain Ltd** that runs silently in the background and monitors your SMS inbox for mobile money (MoMo) payment notifications.

When a matching payment SMS is detected — from MTN, Telecel, AirtelTigo, or any configured provider — the app extracts the transaction data and **HTTP POSTs it as structured JSON to your configured webhook URL**, in real time.

> No polling. No manual checks. SwiftClaim pushes data to your server the moment a matching SMS arrives.

Your server receives clean, structured JSON. You decide what to do with it — credit a wallet, update a database, trigger a notification, mark an order as paid, and more.

---

## 🏪 Real-World Use Case

> **Scenario: Mobile Data Reseller Portal**

A typical mobile data selling site has agents and customers who top up their wallets before purchasing data bundles. While payment gateways like Paystack exist, their charges on large deposits can be significant — so many portal owners prefer to receive payments directly via MoMo.

**Here's where SwiftClaim comes in:**

```
Customer/Agent sends MoMo payment to site owner's number
        ↓
MTN/Telecel/AirtelTigo sends payment received SMS to site owner's phone
        ↓
SwiftClaim detects the SMS and extracts transaction data
        ↓
Data is POSTed to the portal's webhook endpoint
        ↓
Portal stores the transaction and credits the agent/customer wallet
        ↓
Agent can also manually enter Transaction ID to claim deposited funds
```

The **reference field** in the MoMo SMS can carry an agent code or customer ID (provided by the portal), enabling fully automatic wallet crediting without any manual intervention.

---

## ⚙️ How It Works

```
Phone receives SMS
      ↓
SwiftClaim detects matching SMS (via configured regex pattern)
      ↓
Parses fields: amount, sender, reference, transaction ID, balances, fee
      ↓
POSTs JSON payload to your webhook URL
      ↓
Your server handles the business logic
```

The app runs as a **persistent foreground service** — similar to how WhatsApp or Google Maps stays running in the background. It survives:

- App close
- Screen off
- Phone reboot

---

## ✅ Features

| Feature | Details |
|---|---|
| 🔍 **Auto SMS Detection** | Detects payment SMS as they arrive, no manual action needed |
| 🔁 **Auto-start on Boot** | App service restarts automatically after device reboot or power cycle |
| 📡 **Real-time Webhook POST** | Sends structured JSON to your endpoint immediately on detection |
| 📥 **Offline Queue** | When offline, detections are queued and forwarded once internet is restored |
| 🔄 **Manual Resend** | Retry failed webhook deliveries from the History tab |
| 🔔 **Persistent Notification** | Non-dismissible status bar notification confirms service is always active |
| 🛡️ **Foreground + Background Service** | Stays alive in both foreground and background; user can stop it if needed |
| 🧩 **Custom SMS Patterns** | Configure regex patterns to match any MoMo provider's SMS format |
| 📋 **History Tab** | Review past detections and failed deliveries |
| 🔐 **App Key Licensing** | Unique key per device; monthly or annual renewal |

---

## 📦 Installation

1. **Download** the SwiftClaim V3 APK from your order confirmation email or the [SwiftClaim page](https://marcbrainltd.com/automations/swiftclaim).

2. **Transfer** the APK to your Android device.

3. **Open** the APK file to begin installation.

4. If prompted with *"Install from unknown sources"*, follow these steps:
   - Go to **Settings → Security** (or **Settings → Apps → Special app access**)
   - Enable **Install unknown apps** for your file manager or browser
   - Return and proceed with the installation

### Google Play Protect Warning

If you see a warning saying **"App blocked by Play Protect"**, do the following to disable Google Play Protect:

1. Open the **Google Play Store** app
2. Tap your **profile icon** (top-right corner)
3. Select **Play Protect**
4. Tap the **gear/settings icon** (top-right)
5. Toggle off **"Scan apps with Play Protect"**
6. Confirm by tapping **Turn off**
7. Re-attempt the APK installation

> You can re-enable Play Protect after installation if preferred.

---

### App Key & Licensing

SwiftClaim V3 requires a valid **App Key** before it can process any SMS.

- Each key is **unique per customer** and **locks to a single device** on first activation
- Keys are available on a **monthly or annual** subscription basis
- To get your key, contact the developer (see [Support & Contact](#-support--contact))
- If you change phones, contact the developer to **unbind your old device** before activating on a new one

**To activate:**
1. Launch the app
2. Paste your App Key exactly as provided (it is case-sensitive)
3. The key is verified against the MarcBrain server and permanently bound to your device ID

---

## 🔐 Permissions Required

SwiftClaim requires the following permissions to function correctly:

| Permission | Purpose |
|---|---|
| **Read SMS** | To detect and read incoming MoMo payment messages |
| **Receive SMS** | To listen for new SMS messages as they arrive |
| **Internet** | To forward payment data to your webhook |
| **Receive Boot Completed** | To auto-start the service when the device reboots |
| **Foreground Service** | To keep the SMS listener running persistently |
| **Disable Battery Optimization** | Prevents Android from killing the background service; you will be prompted to allow this |
| **Post Notifications** | To display the persistent status notification (required on Android 13+) |

> **Battery Optimization:** You will be prompted to disable battery optimization for SwiftClaim. This is critical — without it, Android may suspend the app and cause missed detections.

---

## 🛠️ Configuration

### Webhook URL Setup

1. Open the app and tap the **Settings** icon
2. Locate the **Webhook URL** field
3. Paste your full HTTPS endpoint, for example:

```
https://yourdomain.com/webhook.php
```

4. For added security, append a secret token as a query parameter:

```
https://yourdomain.com/webhook.php?token=your_secret_token
```

5. Tap **Save**

> SwiftClaim only sends to **HTTPS** endpoints. Plain HTTP URLs are not supported.

---

### SMS Pattern Settings

SwiftClaim uses a **regex pattern** to identify and extract data from payment SMS messages. A default pattern is included for standard MTN MoMo messages, but you can customize it for any provider.

**Default SMS Format Matched:**

```
Payment received for GHS 310.00 from RICHMOND OBENG  Current Balance: GHS 558.92 . Available Balance: GHS 558.92. Reference: 1. Transaction ID: 79864960076. TRANSACTION FEE: 0.00
```

**Default Regex Pattern:**

```regex
Payment received for GHS ([\d,.]+) from (.+?)\s{2,}Current Balance: GHS ([\d,.]+)\s*\.\s*Available Balance: GHS ([\d,.]+)\s*\.\s*Reference:\s*(\S+)\s*\.\s*Transaction ID:\s*(\d+)\s*\.\s*TRANSACTION FEE:\s*([\d,.]+)
```

**Capture Groups (in order):**

| Group | Field |
|---|---|
| 1 | `amount` |
| 2 | `from` |
| 3 | `current_balance` |
| 4 | `available_balance` |
| 5 | `reference` |
| 6 | `transaction_id` |
| 7 | `transaction_fee` |

**Custom Pattern:**
Go to **Settings → SMS Pattern**, clear the default regex, and paste your own. Ensure your regex uses the same 7 capture groups in the same order.

> 💡 Test your regex at [regex101.com](https://regex101.com) using a real sample SMS before saving.

---

## 🔁 Foreground Service & Persistence

SwiftClaim V3 runs as a **persistent foreground service**:

- A **permanent, non-dismissible notification** appears in the status bar confirming the service is active
- The service **survives screen off, app close, and device reboot**
- **No manual trigger is needed** — SMS are detected automatically as they arrive
- You can also perform a **manual scan** from the app's main screen at any time
- The user can **stop the foreground/background service** from within the app if needed

> The foreground service is optimized for minimal battery usage. Unlike earlier versions, V3 reacts to incoming SMS events rather than polling on a timer.

---

## 📶 Offline & Queue Behavior

SwiftClaim handles network interruptions gracefully:

| State | Behavior |
|---|---|
| **Online** | Detected SMS is forwarded to the webhook immediately |
| **Offline** | SMS is still detected and stored in a local queue |
| **Back Online** | Queued detections are automatically forwarded to the webhook |
| **Manual Resend** | You can also manually retry failed deliveries from the **History** tab |

> Even without internet, SwiftClaim never misses a detection — it just waits until the network is available before forwarding.

---

## 📦 Webhook Payload Reference

When a payment SMS is detected, SwiftClaim sends a `POST` request to your webhook URL with the following JSON body:

```json
{
  "event": "payment_received",
  "timestamp": "2025-10-14T09:45:00.000Z",
  "data": {
    "amount": "310.00",
    "from": "RICHMOND OBENG",
    "current_balance": "558.92",
    "available_balance": "558.92",
    "reference": "1",
    "transaction_id": "79864960076",
    "transaction_fee": "0.00"
  }
}
```

| Field | Type | Description |
|---|---|---|
| `event` | `string` | Always `"payment_received"` |
| `timestamp` | `string` | ISO 8601 datetime of detection |
| `data.amount` | `string` | Amount received (GHS) |
| `data.from` | `string` | Sender name as it appears in the SMS |
| `data.current_balance` | `string` | Account balance after the transaction |
| `data.available_balance` | `string` | Available balance after the transaction |
| `data.reference` | `string` | Payment reference provided by sender (can be agent code/ID) |
| `data.transaction_id` | `string` | Unique transaction ID — **use this to detect duplicates** |
| `data.transaction_fee` | `string` | Fee charged for the transaction |

---

## 🔧 Webhook Integration Examples

Your webhook must be a publicly accessible HTTPS endpoint that accepts `POST` requests with `Content-Type: application/json` and returns HTTP `200` on success.

<details>
<summary><strong>PHP</strong></summary>

```php
<?php
// webhook.php
header('Content-Type: application/json');

// Optional: validate secret token
$token = $_GET['token'] ?? '';
if ($token !== 'your_secret_token') {
    http_response_code(403);
    echo json_encode(['status' => 'forbidden']);
    exit;
}

// Read raw payload
$payload = json_decode(file_get_contents('php://input'), true);
if (!$payload || ($payload['event'] ?? '') !== 'payment_received') {
    http_response_code(400);
    echo json_encode(['status' => 'error', 'message' => 'Invalid payload']);
    exit;
}

$data       = $payload['data'] ?? [];
$amount     = $data['amount'] ?? null;
$from       = $data['from'] ?? null;
$txn_id     = $data['transaction_id'] ?? null;
$reference  = $data['reference'] ?? null;

// TODO: Check for duplicate transaction
// e.g. SELECT COUNT(*) FROM payments WHERE transaction_id = '$txn_id'

// TODO: Your business logic here
// e.g. credit agent wallet using $reference as agent code

// Log to file
file_put_contents(
    __DIR__ . '/webhook_log.txt',
    "[" . date("Y-m-d H:i:s") . "] GHS $amount from $from | TxnID: $txn_id" . PHP_EOL,
    FILE_APPEND
);

http_response_code(200);
echo json_encode(['status' => 'received']);
```

</details>

<details>
<summary><strong>Node.js / Express</strong></summary>

```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/webhook', (req, res) => {
    const token = req.query.token;
    if (token !== 'your_secret_token') {
        return res.status(403).json({ status: 'forbidden' });
    }

    const { event, timestamp, data } = req.body;
    if (event !== 'payment_received') {
        return res.status(400).json({ status: 'error', message: 'Invalid event' });
    }

    const { amount, from, transaction_id, reference } = data;

    // TODO: check duplicate by transaction_id
    // TODO: credit wallet using reference as agent code

    console.log(`[${timestamp}] GHS ${amount} from ${from} | TxnID: ${transaction_id}`);
    res.status(200).json({ status: 'received' });
});

app.listen(3000, () => console.log('Webhook server running on port 3000'));
```

</details>

<details>
<summary><strong>Python / Flask</strong></summary>

```python
from flask import Flask, request, jsonify
import logging

app = Flask(__name__)
logging.basicConfig(level=logging.INFO)
SECRET_TOKEN = 'your_secret_token'

@app.route('/webhook', methods=['POST'])
def webhook():
    token = request.args.get('token', '')
    if token != SECRET_TOKEN:
        return jsonify({'status': 'forbidden'}), 403

    payload = request.get_json(silent=True)
    if not payload or payload.get('event') != 'payment_received':
        return jsonify({'status': 'error', 'message': 'Invalid payload'}), 400

    data      = payload.get('data', {})
    amount    = data.get('amount')
    from_name = data.get('from')
    txn_id    = data.get('transaction_id')
    reference = data.get('reference')
    timestamp = payload.get('timestamp')

    # TODO: check duplicate by txn_id
    # TODO: credit wallet using reference as agent/customer ID

    logging.info(f"[{timestamp}] GHS {amount} from {from_name} | TxnID: {txn_id}")
    return jsonify({'status': 'received'}), 200

if __name__ == '__main__':
    app.run(port=5000)
```

</details>

<details>
<summary><strong>Laravel (PHP)</strong></summary>

```php
// routes/api.php
Route::post('/webhook/swiftclaim', [SwiftClaimWebhookController::class, 'handle']);

// app/Http/Controllers/SwiftClaimWebhookController.php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Payment;
use Illuminate\Support\Facades\Log;

class SwiftClaimWebhookController extends Controller
{
    public function handle(Request $request)
    {
        if ($request->query('token') !== config('services.swiftclaim.secret')) {
            return response()->json(['status' => 'forbidden'], 403);
        }

        $payload = $request->json()->all();
        if (empty($payload) || ($payload['event'] ?? '') !== 'payment_received') {
            return response()->json(['status' => 'error'], 400);
        }

        $data = $payload['data'] ?? [];

        // Prevent duplicate processing
        if (Payment::where('transaction_id', $data['transaction_id'])->exists()) {
            return response()->json(['status' => 'duplicate'], 200);
        }

        Payment::create([
            'amount'           => $data['amount'],
            'from'             => $data['from'],
            'transaction_id'   => $data['transaction_id'],
            'current_balance'  => $data['current_balance'],
            'reference'        => $data['reference'],
            'transaction_fee'  => $data['transaction_fee'],
            'received_at'      => now(),
        ]);

        Log::info('SwiftClaim payment received', $data);
        return response()->json(['status' => 'received'], 200);
    }
}

// Note: Exclude this route from CSRF in VerifyCsrfToken.php:
// protected $except = ['api/webhook/swiftclaim'];
```

</details>

<details>
<summary><strong>WordPress</strong></summary>

```php
// Registers: POST https://yourdomain.com/wp-json/swiftclaim/v1/webhook
add_action('rest_api_init', function () {
    register_rest_route('swiftclaim/v1', '/webhook', [
        'methods'             => 'POST',
        'callback'            => 'swiftclaim_handle_webhook',
        'permission_callback' => '__return_true',
    ]);
});

function swiftclaim_handle_webhook(WP_REST_Request $request) {
    $token = $request->get_param('token');
    if ($token !== 'your_secret_token') {
        return new WP_REST_Response(['status' => 'forbidden'], 403);
    }

    $payload = $request->get_json_params();
    if (empty($payload) || ($payload['event'] ?? '') !== 'payment_received') {
        return new WP_REST_Response(['status' => 'error'], 400);
    }

    $data   = $payload['data'] ?? [];
    $txn_id = sanitize_text_field($data['transaction_id'] ?? '');

    global $wpdb;
    $table  = $wpdb->prefix . 'swiftclaim_payments';
    $exists = $wpdb->get_var($wpdb->prepare("SELECT COUNT(*) FROM $table WHERE transaction_id = %s", $txn_id));

    if ($exists > 0) {
        return new WP_REST_Response(['status' => 'duplicate'], 200);
    }

    $wpdb->insert($table, [
        'amount'         => sanitize_text_field($data['amount'] ?? ''),
        'sender'         => sanitize_text_field($data['from'] ?? ''),
        'transaction_id' => $txn_id,
        'current_balance'=> sanitize_text_field($data['current_balance'] ?? ''),
        'reference'      => sanitize_text_field($data['reference'] ?? ''),
        'transaction_fee'=> sanitize_text_field($data['transaction_fee'] ?? ''),
        'received_at'    => current_time('mysql'),
    ]);

    return new WP_REST_Response(['status' => 'received'], 200);
}
```

</details>

---

## 🔄 Retry Behavior

- If your webhook is unreachable or returns a non-`200` status, SwiftClaim marks the record as **failed** in local history
- Failed entries are **not automatically retried** — this is intentional to prevent duplicate processing on your server
- You can review failed entries in the **History** tab and **manually resend** them at any time

> **Best Practice:** Build your webhook to be **idempotent** — processing the same `transaction_id` twice should have no harmful side effects. This protects you if a manual resend occurs.

---

## 🔒 Security Tips

- **Use a secret token** — Append a token to your webhook URL (`?token=abc123`) and validate it on every request. Reject any call that doesn't match.
- **Validate `transaction_id`** — Always check if you've already processed this ID before taking action. Never credit the same payment twice.
- **HTTPS only** — SwiftClaim will only POST to HTTPS endpoints. Never use plain HTTP.
- **Respond with `200` quickly** — Do heavy processing (DB writes, email notifications) asynchronously. A slow response may cause the app to treat the delivery as failed.
- **Log all incoming requests** — Keep a raw log of payloads so you can debug and replay if needed.
- **Never expose your webhook URL publicly** without a secret token. Anyone who knows the URL can POST fake payment data to your endpoint.

---

## 📋 Version History

### V1 — Deprecated
- Basic SMS reading capability
- Required a paid API key to function
- Manual trigger only — no automatic detection
- High battery drain due to aggressive polling

### V2 — Legacy
- Resolved battery drain — no longer aggressively polls
- Removed payment requirement — free to use
- Still manual trigger only
- No persistent background service

### V3 — Current ✅
- Supports both **manual triggers** and **automatic foreground/background running**
- Persistent non-dismissible notification confirms the service is always active
- Survives app close, screen off, and device reboot
- Requires a licensed **App Key**, locked to one device on first use
- **History tab** to review past detections and failed webhook deliveries

---

## 📞 Support & Contact

Built and maintained by **MarcBrain Ltd**.

| Channel | Details |
|---|---|
| 📧 Email | [info@marcbrainltd.com](mailto:info@marcbrainltd.com) |
| 📞 Phone | [+233 2487 70024](tel:+233248770024) |
| 🌐 Website | [marcbrainltd.com](https://marcbrainltd.com) |
| 💬 Contact Form | [marcbrainltd.com/contact](https://marcbrainltd.com/contact) |
| 📄 Full Docs | [SwiftClaim Documentation](https://marcbrainltd.com/automations/swiftclaim/documentation) |
| 🔖 Changelog | [Version History](https://marcbrainltd.com/automations/swiftclaim/changelog) |

> For App Key purchase, renewal, or device unbinding requests, reach out via email or phone above.

---

<p align="center">
  <sub>© 2026 MarcBrain Ltd · All rights reserved · <a href="https://marcbrainltd.com/privacy">Privacy Policy</a> · <a href="https://marcbrainltd.com/terms">Terms of Service</a></sub>
</p>

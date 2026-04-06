# EmailJS Setup Guide — Dirtbox Club

This guide walks you through setting up EmailJS so all order emails work automatically.
EmailJS is free for 200 emails/month (plenty for getting started) and works directly from
the static site — no server needed.

---

## Step 1 — Create your EmailJS account

1. Go to **https://www.emailjs.com** and sign up with your email
2. Verify your email address

---

## Step 2 — Add an Email Service

1. In the EmailJS dashboard, click **Email Services** → **Add New Service**
2. Choose **Gmail** and connect the Google account that manages **info@dirtboxclub.co.uk**
   (This ensures all emails are sent FROM info@dirtboxclub.co.uk — branded as Dirtbox Club)
3. Click **Connect Account** and authorise EmailJS to send on your behalf
4. Give it a Service ID of `dirtboxclub_service` (or note down whatever ID it generates)

---

## Step 3 — Create the Email Templates

You need **5 templates** in total. Create each one in EmailJS → **Email Templates** → **Create New Template**.

---

### Template 1: `dbc_order_confirmation`
**Customer order confirmation (sent to buyer, from info@dirtboxclub.co.uk)**

- **Template ID:** `dbc_order_confirmation`
- **Subject:** `Your Dirtbox Club order is confirmed ✓ — {{order_ref}}`
- **To:** `{{to_email}}`
- **From name:** `Dirtbox Club`
- **Reply-To:** `info@dirtboxclub.co.uk`
- **Body (HTML):**

```html
<!DOCTYPE html>
<html>
<head><meta charset="UTF-8"></head>
<body style="background:#0c0c0c;color:#fff;font-family:Arial,sans-serif;margin:0;padding:0">
<div style="max-width:560px;margin:0 auto;padding:40px 20px">

  <div style="text-align:center;margin-bottom:32px">
    <h1 style="font-size:28px;letter-spacing:0.05em;margin:0">DIRTBOXCLUB<span style="color:#e63226">.</span></h1>
    <p style="color:#777;font-size:12px;letter-spacing:0.15em;margin-top:8px">NOT FOR EVERYONE</p>
  </div>

  <div style="background:#1c1c1c;border-radius:12px;padding:32px;margin-bottom:24px">
    <div style="text-align:center;margin-bottom:24px">
      <div style="width:60px;height:60px;border-radius:50%;background:rgba(34,197,94,0.1);border:2px solid rgba(34,197,94,0.4);display:inline-flex;align-items:center;justify-content:center;font-size:24px;margin-bottom:16px">✓</div>
      <h2 style="font-size:24px;margin:0 0 8px 0">Order Confirmed!</h2>
      <p style="color:#999;font-size:14px;margin:0">Hi {{to_name}}, your order is in and we're on it.</p>
    </div>

    <div style="background:#111;border-radius:8px;padding:20px;margin-bottom:20px">
      <p style="font-size:11px;color:#777;letter-spacing:0.1em;text-transform:uppercase;margin:0 0 8px 0">Order Reference</p>
      <p style="font-size:18px;color:#e63226;margin:0;font-weight:bold">{{order_ref}}</p>
    </div>

    <h3 style="font-size:13px;color:#777;letter-spacing:0.08em;text-transform:uppercase;margin:0 0 12px 0">Your Items</h3>
    <table style="width:100%;border-collapse:collapse">
      {{order_items_html}}
    </table>

    <div style="border-top:1px solid #2a2a2a;margin-top:16px;padding-top:16px">
      <div style="display:flex;justify-content:space-between;font-size:13px;color:#999;margin-bottom:8px">
        <span>Subtotal</span><span>{{subtotal}}</span>
      </div>
      <div style="display:flex;justify-content:space-between;font-size:13px;color:#999;margin-bottom:8px">
        <span>Shipping</span><span>{{shipping}}</span>
      </div>
      <div style="display:flex;justify-content:space-between;font-size:16px;color:#fff;font-weight:bold;border-top:1px solid #2a2a2a;padding-top:12px;margin-top:8px">
        <span>Total Paid</span><span style="color:#e63226">{{order_total}}</span>
      </div>
    </div>
  </div>

  <div style="background:#1c1c1c;border-radius:12px;padding:24px;margin-bottom:24px;text-align:center">
    <h3 style="font-size:13px;letter-spacing:0.08em;text-transform:uppercase;margin:0 0 16px 0">What Happens Next</h3>
    <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:12px;text-align:center">
      <div>
        <div style="font-size:24px;margin-bottom:8px">📧</div>
        <p style="font-size:11px;color:#999;margin:0">You got this confirmation email</p>
      </div>
      <div>
        <div style="font-size:24px;margin-bottom:8px">📦</div>
        <p style="font-size:11px;color:#999;margin:0">We pick & pack your order (1–2 business days)</p>
      </div>
      <div>
        <div style="font-size:24px;margin-bottom:8px">🚚</div>
        <p style="font-size:11px;color:#999;margin:0">Shipping email with tracking number sent</p>
      </div>
    </div>
    <p style="font-size:12px;color:#777;margin:16px 0 0 0">Estimated delivery: <strong style="color:#fff">{{delivery_estimate}}</strong></p>
  </div>

  <div style="text-align:center">
    <a href="{{shop_url}}" style="display:inline-block;background:#e63226;color:#fff;padding:12px 28px;border-radius:6px;text-decoration:none;font-size:13px;letter-spacing:0.05em">Continue Shopping</a>
  </div>

  <p style="text-align:center;font-size:11px;color:#555;margin-top:32px">
    Questions? Email us at <a href="mailto:info@dirtboxclub.co.uk" style="color:#e63226">info@dirtboxclub.co.uk</a> or visit <a href="https://fitch83.github.io/dirtboxclub/contact.html" style="color:#e63226">dirtboxclub.co.uk</a><br>
    © 2025 DIRTBOXCLUB. — Not For Everyone
  </p>
</div>
</body>
</html>
```

---

### Template 2: `dbc_admin_new_order`
**Admin sale notification (sent to info@dirtboxclub.co.uk)**

- **Template ID:** `dbc_admin_new_order`
- **Subject:** `💰 New Order — {{order_ref}} — {{order_total}}`
- **To:** `{{to_email}}` (will be info@dirtboxclub.co.uk)
- **Body (HTML):**

```html
<!DOCTYPE html>
<html>
<body style="background:#0c0c0c;color:#fff;font-family:Arial,sans-serif;margin:0;padding:20px">
<div style="max-width:500px;margin:0 auto">
  <h1 style="color:#e63226;font-size:24px;margin:0 0 8px">💰 New Sale!</h1>
  <h2 style="font-size:18px;margin:0 0 24px;color:#fff">{{order_ref}}</h2>

  <table style="width:100%;background:#1c1c1c;border-radius:8px;padding:20px;border-collapse:collapse">
    <tr><td style="padding:8px 0;color:#777;font-size:13px;width:120px">Customer</td><td style="color:#fff;font-size:13px">{{customer_name}}</td></tr>
    <tr><td style="padding:8px 0;color:#777;font-size:13px">Email</td><td style="color:#fff;font-size:13px">{{customer_email}}</td></tr>
    <tr><td style="padding:8px 0;color:#777;font-size:13px">Total</td><td style="color:#e63226;font-size:16px;font-weight:bold">{{order_total}}</td></tr>
    <tr><td style="padding:8px 0;color:#777;font-size:13px">Date</td><td style="color:#fff;font-size:13px">{{order_date}}</td></tr>
  </table>

  <div style="background:#1c1c1c;border-radius:8px;padding:20px;margin-top:12px">
    <p style="font-size:12px;color:#777;margin:0 0 8px">Items Ordered:</p>
    <p style="font-size:13px;color:#fff;margin:0;line-height:1.8">{{order_items}}</p>
  </div>

  <div style="text-align:center;margin-top:24px">
    <a href="{{dashboard_url}}" style="background:#e63226;color:#fff;padding:12px 24px;border-radius:6px;text-decoration:none;font-size:13px">View in Dashboard →</a>
  </div>
</div>
</body>
</html>
```

---

### Template 3: `dbc_shipping_update`
**Shipping notification (sent to customer when dispatched)**

- **Template ID:** `dbc_shipping_update`
- **Subject:** `Your Dirtbox Club order is on its way! 🚚 — {{order_ref}}`
- **To:** `{{to_email}}`
- **Body (HTML):**

```html
<!DOCTYPE html>
<html>
<body style="background:#0c0c0c;color:#fff;font-family:Arial,sans-serif;margin:0;padding:0">
<div style="max-width:560px;margin:0 auto;padding:40px 20px">
  <div style="text-align:center;margin-bottom:32px">
    <h1 style="font-size:28px;letter-spacing:0.05em;margin:0">DIRTBOXCLUB<span style="color:#e63226">.</span></h1>
  </div>

  <div style="background:#1c1c1c;border-radius:12px;padding:32px;margin-bottom:24px;text-align:center">
    <div style="font-size:48px;margin-bottom:16px">🚚</div>
    <h2 style="font-size:22px;margin:0 0 8px">Your order is on its way!</h2>
    <p style="color:#999;font-size:14px;margin:0">Hi {{to_name}}, your Dirtbox Club order has been dispatched.</p>
  </div>

  <div style="background:#1c1c1c;border-radius:12px;padding:24px;margin-bottom:24px">
    <div style="margin-bottom:16px">
      <p style="font-size:11px;color:#777;text-transform:uppercase;letter-spacing:0.1em;margin:0 0 6px">Order Reference</p>
      <p style="font-size:16px;color:#fff;margin:0">{{order_ref}}</p>
    </div>
    <div style="margin-bottom:16px">
      <p style="font-size:11px;color:#777;text-transform:uppercase;letter-spacing:0.1em;margin:0 0 6px">Tracking Number</p>
      <p style="font-size:20px;color:#e63226;font-weight:bold;margin:0">{{tracking_number}}</p>
    </div>
    <div style="margin-bottom:16px">
      <p style="font-size:11px;color:#777;text-transform:uppercase;letter-spacing:0.1em;margin:0 0 6px">Carrier</p>
      <p style="font-size:14px;color:#fff;margin:0">{{carrier}}</p>
    </div>
    <div>
      <p style="font-size:11px;color:#777;text-transform:uppercase;letter-spacing:0.1em;margin:0 0 6px">Estimated Delivery</p>
      <p style="font-size:14px;color:#fff;margin:0">{{delivery_estimate}}</p>
    </div>
  </div>

  <div style="text-align:center;margin-bottom:24px">
    <a href="{{tracking_url}}" style="background:#e63226;color:#fff;padding:14px 32px;border-radius:6px;text-decoration:none;font-size:14px;font-weight:bold">Track Your Order →</a>
  </div>

  <p style="text-align:center;font-size:11px;color:#555;margin-top:32px">
    © 2025 DIRTBOXCLUB. — Not For Everyone
  </p>
</div>
</body>
</html>
```

---

### Template 4: `dbc_status_update`
**Order status change notification**

- **Template ID:** `dbc_status_update`
- **Subject:** `Your Dirtbox Club order update — {{order_ref}}`
- **To:** `{{to_email}}`
- **Body (HTML):**

```html
<!DOCTYPE html>
<html>
<body style="background:#0c0c0c;color:#fff;font-family:Arial,sans-serif;margin:0;padding:0">
<div style="max-width:560px;margin:0 auto;padding:40px 20px">
  <div style="text-align:center;margin-bottom:32px">
    <h1 style="font-size:28px;letter-spacing:0.05em;margin:0">DIRTBOXCLUB<span style="color:#e63226">.</span></h1>
  </div>
  <div style="background:#1c1c1c;border-radius:12px;padding:32px;text-align:center">
    <h2 style="font-size:20px;margin:0 0 12px">Order Update</h2>
    <p style="color:#999;font-size:14px;margin:0 0 20px">Hi {{to_name}}, here's an update on your order.</p>
    <div style="background:#111;border-radius:8px;padding:16px;margin-bottom:20px">
      <p style="font-size:11px;color:#777;margin:0 0 6px;text-transform:uppercase;letter-spacing:0.1em">Order {{order_ref}}</p>
      <p style="font-size:22px;color:#e63226;font-weight:bold;margin:0">{{new_status}}</p>
    </div>
    <a href="{{shop_url}}" style="background:#e63226;color:#fff;padding:12px 24px;border-radius:6px;text-decoration:none;font-size:13px">Visit the Shop</a>
  </div>
  <p style="text-align:center;font-size:11px;color:#555;margin-top:32px">© 2025 DIRTBOXCLUB.</p>
</div>
</body>
</html>
```

---

### Template 5: `dbc_custom_message`
**Custom message from admin to customer**

- **Template ID:** `dbc_custom_message`
- **Subject:** `{{subject}}`
- **To:** `{{to_email}}`
- **Body (HTML):**

```html
<!DOCTYPE html>
<html>
<body style="background:#0c0c0c;color:#fff;font-family:Arial,sans-serif;margin:0;padding:0">
<div style="max-width:560px;margin:0 auto;padding:40px 20px">
  <div style="text-align:center;margin-bottom:32px">
    <h1 style="font-size:28px;letter-spacing:0.05em;margin:0">DIRTBOXCLUB<span style="color:#e63226">.</span></h1>
  </div>
  <div style="background:#1c1c1c;border-radius:12px;padding:32px">
    <p style="color:#999;font-size:13px;margin:0 0 6px">Hi {{to_name}},</p>
    <p style="font-size:13px;color:#777;margin:0 0 20px">Regarding order <strong style="color:#fff">{{order_ref}}</strong>:</p>
    <div style="background:#111;border-radius:8px;padding:20px;margin-bottom:20px">
      <p style="font-size:14px;color:#fff;line-height:1.8;margin:0;white-space:pre-wrap">{{message}}</p>
    </div>
    <p style="font-size:12px;color:#777;margin:0">— The Dirtbox Club Team</p>
  </div>
  <p style="text-align:center;font-size:11px;color:#555;margin-top:32px">© 2025 DIRTBOXCLUB.</p>
</div>
</body>
</html>
```

---

## Step 4 — Get your Public Key

1. In EmailJS dashboard → **Account** → **General**
2. Copy your **Public Key**

---

## Step 5 — Update the code

Open **two files** and replace the placeholder values:

### In `order-success.html` (around line 135):
```javascript
const EMAILJS_PUBLIC_KEY  = 'YOUR_ACTUAL_PUBLIC_KEY';
const EMAILJS_SERVICE_ID  = 'dirtboxclub_service';  // or whatever you named it
```

### In `dashboard.html` (around line 835):
```javascript
const EMAILJS_PUBLIC_KEY          = 'YOUR_ACTUAL_PUBLIC_KEY';
const EMAILJS_SERVICE_ID          = 'dirtboxclub_service';
```

Then push to GitHub and the emails will work automatically.

---

## Summary

| Template ID | When it sends | Who receives |
|---|---|---|
| `dbc_order_confirmation` | Customer completes checkout | Customer |
| `dbc_admin_new_order` | Customer completes checkout | Matt (info@dirtboxclub.co.uk) |
| `dbc_shipping_update` | Matt clicks "Shipping Update" in dashboard | Customer |
| `dbc_status_update` | Matt changes order status | Customer (optional) |
| `dbc_custom_message` | Matt clicks "Send Message" in dashboard | Customer |

Free EmailJS plan: **200 emails/month**. Paid plans start at $15/month for 1,000/month.

---

## Stripe Integration Note

When Stripe redirects to `order-success.html` after payment, pass the customer details
in the URL or store them in `sessionStorage` before redirecting:

```javascript
// Before redirecting to Stripe checkout, save order to sessionStorage:
sessionStorage.setItem('dbc_pending_order', JSON.stringify({
  ref: 'DBC-' + Date.now().toString(36).toUpperCase().slice(-6),
  customerName: 'John Smith',
  customerEmail: 'john@example.com',
  items: [{ name: 'Awkward Since Birth Tee', qty: 1, price: 35.00 }],
  subtotal: 35.00,
  shipping: 0,
  total: 35.00
}));

// Then redirect to Stripe. On success, Stripe sends customer to:
// https://fitch83.github.io/dirtboxclub/order-success.html
```

The success page reads this from `sessionStorage` on load, saves it to Firestore,
and triggers all the emails automatically.

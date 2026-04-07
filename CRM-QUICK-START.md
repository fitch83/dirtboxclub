# CRM Quick Start Guide

## What's New?

Your Dirtbox Club website now has a professional marketing CRM system with three main features:

### 1. Customer Management (CRM Tab)
- View all customers in a searchable, filterable table
- Click any customer to see their detailed profile
- Track customer lifetime value, order history, and purchase dates
- Add internal notes to customer records

### 2. Email Campaigns
- Send targeted email campaigns to customer segments
- Choose from pre-defined segments: VIP, Recent Buyers, At Risk, Members
- Personalize emails with merge tags like {{name}}
- Preview emails before sending
- Track campaign success rates

### 3. Newsletter Signup
- Website visitors can subscribe from the homepage footer
- Automatic duplicate prevention
- Subscribers stored securely in Firebase
- Ready to target with email campaigns

---

## Admin Dashboard - CRM Tab

### Accessing the CRM

1. Go to `admin-login.html`
2. Login with your admin credentials
3. Click the **💌 CRM** button in the tab bar
4. You'll see two sections: Customers and Campaigns

### Customer Management

#### Viewing All Customers
- Scroll to the "Customers" tab
- See a table with: Name, Email, Total Spend, Orders, Last Order, Status, Tags
- Sort by clicking customer name or email

#### Customer Segments
Quick-view cards show:
- **All Customers**: Total customer count
- **VIP (>£100)**: Customers who've spent more than £100
- **Recent Buyers**: Purchased in the last 30 days
- **At Risk**: Haven't bought in 90+ days
- **Members**: Have member status
- **Newsletter Subscribers**: Signed up for emails

Click any segment card to instantly filter the list.

#### Searching & Filtering

**Search by name or email:**
- Type in the "Search by name or email" box
- Results update instantly

**Advanced Filters:**
- Min/Max Spend (£)
- Minimum Order Count
- Member Status (Member / Non-member)
- Mix and match filters

#### Customer Profile

Click any customer row to open their profile:

**Personal Info**
- Name, email, phone, membership status

**Purchase History Summary**
- Total Spend (£)
- Number of Orders
- First Order Date
- Last Order Date

**Order Details Table**
- Complete list of all orders
- Order ID, date, items, total price

**Notes Section**
- Add internal notes about the customer
- Saves automatically to Firestore
- Use for: VIP reminders, special requests, complaint tracking

---

## Email Campaigns

### Creating a Campaign

1. Click the **📧 Campaigns** tab in CRM
2. Select your target segment (who to send to)
3. Enter subject line
4. Write your message

#### Using Merge Tags

Make emails personal by using these tags:
- `{{name}}` → "Hello John"
- `{{email}}` → "john@example.com"
- `{{totalSpend}}` → "£150.00"
- `{{orderCount}}` → "3"

Example message:
```
Hi {{name}},

As one of our VIP customers who's spent {{totalSpend}},
you have exclusive early access to our new drop!

Best,
Dirtbox Club
```

#### Preview & Review

- Live preview shows how email will look with sample data
- Recipient count shows exactly how many will receive it
- Review before sending

#### Sending

1. Click **Send Campaign**
2. Confirm the number of recipients
3. Campaign sends to all selected customers
4. Success/failure counts displayed

### Campaign History

Scroll down to see all past campaigns:
- Date sent
- Subject line
- Target segment
- Number of recipients
- Status (Sent / Partial)

---

## Newsletter Signup (For Website Visitors)

### Footer Newsletter Form

The homepage now has an enhanced newsletter signup in the footer:

**Fields:**
- Name (optional)
- Email (required)
- Subscribe button

**Validation:**
- Checks for valid email format
- Prevents duplicate signups
- Shows success/error messages

### Subscriber Data

Each signup is saved with:
- Email address
- Name (if provided)
- Signup date and time
- Source page (which page they signed up from)

Subscribers automatically appear in your CRM under the "Newsletter Subscribers" segment.

---

## Database Structure (Firebase)

### Customers Collection

Each customer record contains:
```
- name: String
- email: String (unique)
- phone: String or empty
- totalSpend: Number (£ formatted)
- orderCount: Number
- lastOrderDate: Timestamp or null
- firstOrderDate: Timestamp or null
- memberStatus: "member" or "non-member"
- tags: Array of strings
- orders: Array (order details)
- notes: String (internal notes)
```

### Campaigns Collection

Each campaign record contains:
```
- date: Timestamp (when sent)
- subject: String
- segment: String (all, vip, recent, atrisk, members, subscribers)
- recipients: Number (how many were targeted)
- successCount: Number (successful sends)
- failureCount: Number (failed sends)
- status: "Sent" or "Partial"
- body: String (full email text sent)
```

### Subscribers Collection

Each subscriber record contains:
```
- email: String (unique, prevents duplicates)
- name: String or null
- subscribedAt: Timestamp (ISO format)
- source: String (URL where they signed up)
```

---

## Security & Access Control

### Admin Access
- Only admin users can access CRM tab
- Only admins can read/write customers and campaigns
- Admin authentication via Firebase

### Public Access
- Anyone can sign up for newsletter
- Newsletter signups are write-only (subscribers can't see each other)
- All other data is protected

### Firestore Rules
Security rules are defined in `firestore.rules`:
- Admins: Full access to customers, campaigns, products, orders, etc.
- Public: Can only create new newsletter signups
- Everyone else: No access

---

## Common Tasks

### Send a "Welcome Back" Campaign to At-Risk Customers

1. Go to CRM → Campaigns
2. Select "At Risk (90d+)" segment
3. Subject: "We miss you! Special offer inside 💝"
4. Body:
```
Hi {{name}},

It's been a while since we've seen you!
As one of our valued customers, here's 15% off your next order.

Use code: COMEBACK15

Shop now → [link]

Best,
Dirtbox
```
5. Click Send Campaign

### Send VIP Preview of New Drop

1. Go to CRM → Campaigns
2. Select "VIP Customers (>£100)" segment
3. Subject: "First access: New drop for VIPs only"
4. Body:
```
{{name}},

Thanks for being a VIP! You get exclusive first access to drops.

Our latest collection drops tomorrow at midnight.

Get early access now → [link]

Cheers,
Dirtbox Club
```
5. Click Send Campaign

### Track Customer Lifetime Value

1. Go to CRM → Customers
2. Click any customer
3. View "Total Spend" and "Order Count"
4. See all past orders with dates and amounts
5. Use this to identify VIPs and at-risk customers

---

## Important Notes

### Performance
- System works smoothly with up to ~1000 customers
- Email campaigns send one at a time (suited for ~500 recipients)
- Data loads when you switch to CRM tab (not real-time)

### Best Practices

1. **Segment First**
   - Always target the right segment
   - Avoid sending to "All Customers" unless necessary

2. **Personalize**
   - Use {{name}} and {{totalSpend}} merge tags
   - Makes emails feel personal, improves open rates

3. **Review Before Sending**
   - Check preview panel
   - Confirm recipient count
   - Verify subject and body

4. **Track Segments**
   - Monitor how many are in "At Risk"
   - Build up "VIP" segment over time
   - Watch new "Recent Buyers" grow

5. **Keep Notes Updated**
   - Add notes about special requests
   - Flag VIPs for personal attention
   - Track issue resolutions

---

## Troubleshooting

### Campaign Won't Send
- Check EmailJS is configured in admin dashboard
- Verify email addresses are valid
- Check Firebase has internet connection
- Try with smaller segment first

### Newsletter Signup Not Working
- Verify email address format
- Check browser console for JavaScript errors
- Ensure Firebase is initialized
- Try different browser

### Customers Not Showing
- Refresh the page
- Switch away from CRM tab, then back
- Check browser console for errors
- Verify Firebase has customer data

### Segment Counts Wrong
- These calculate automatically
- Based on: totalSpend, lastOrderDate, memberStatus
- Counts update when you reload CRM
- Make sure customer data is accurate

---

## Advanced Features

### Custom Customer Notes

Each customer has a notes field for internal use:
- VIP customer alerts
- Special requests tracking
- Issue history
- Follow-up reminders
- Internal tags

### Campaign Analytics (Coming Soon)

Currently tracked:
- Date sent
- Recipients targeted
- Success/failure counts

Future enhancements:
- Open rates
- Click tracking
- Reply handling

### Email Merge Tags (Reference)

Available in campaign emails:
- `{{name}}` - Customer first name
- `{{email}}` - Customer email address
- `{{totalSpend}}` - Formatted as £0.00
- `{{orderCount}}` - Number of orders

---

## Support & Documentation

For more details, see:
- `CRM-IMPLEMENTATION-SUMMARY.md` - Technical details
- `firestore.rules` - Security rules
- `dashboard.html` - Source code (lines ~2300+)
- `index.html` - Newsletter signup code

For questions:
1. Check the implementation summary
2. Review Firestore documentation
3. Check EmailJS template configuration
4. Verify Firebase project settings

---

**Your Marketing CRM is now ready to use!**

Start by:
1. Send a welcome email to new newsletter subscribers
2. Create a VIP campaign for your top customers
3. Reach out to at-risk customers with a special offer

Good luck! 🚀

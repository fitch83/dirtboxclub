# Marketing CRM System - Implementation Summary

## Overview
A complete marketing CRM system has been successfully built and deployed for the Dirtbox Club website. The system enables admin management of customer relationships, email marketing campaigns, and newsletter subscriptions.

## Completed Features

### 1. CRM Tab in Admin Dashboard
**File**: `dashboard.html`

#### Customer List View
- Table displaying all customers with columns:
  - Name, Email, Total Spend (£), Order Count, Last Order Date, Member Status, Tags, Actions
- Real-time filtering and searching capabilities
- Click on any customer row to view detailed profile

#### Customer Segments (Automatic)
Six pre-built customer segments with live counts:
- **All Customers** - Total customer count
- **VIP Customers** - Total spend > £100
- **Recent Buyers** - Purchased in the last 30 days
- **At Risk** - No purchase in 90+ days (but has order history)
- **Members** - Customers with "member" status
- **Newsletter Subscribers** - Customers in subscribers collection

Click any segment card to filter the customer list instantly.

#### Search and Advanced Filters
- Full-text search by customer name or email
- Filter by minimum/maximum spend (£)
- Filter by minimum order count
- Filter by member status (Member / Non-member)

#### Customer Detail Modal
Opens when clicking a customer row, showing:
- Personal Information: Full name, email, phone, member status
- Purchase History: Total spend, order count, first/last order dates
- Order Details Table: Complete order history with items and totals
- Notes Editor: Internal notes field (saves to Firestore)

### 2. Email Campaigns Feature
**File**: `dashboard.html`

#### Campaign Creation Form
- **Segment Selector**: Choose target audience (All, VIP, Recent Buyers, At Risk, Members, Newsletter Subscribers)
- **Subject Line**: Custom email subject
- **Message Body**: Rich text with merge tag support
  - `{{name}}` - Customer first name
  - `{{email}}` - Customer email address
  - `{{totalSpend}}` - Total lifetime spend formatted
  - `{{orderCount}}` - Number of orders placed
- **Live Preview Panel**: Shows how email will appear with sample data
- **Recipient Counter**: Automatically updates count based on selected segment
- **Send Button**: Triggers campaign to all recipients via EmailJS

#### Campaign Process
1. Select target segment
2. Write subject and message with optional merge tags
3. View live preview with sample data
4. Confirm recipient count
5. Send campaign to all selected customers
6. Campaign automatically recorded in history with success/failure counts

#### Campaign History Table
- Date, Subject, Segment, Recipients, Status (Sent/Partial)
- Loaded from Firestore `campaigns` collection
- Most recent first

### 3. Newsletter Signup (Footer Update)
**File**: `index.html`

#### Enhanced Signup Form
- Name input (optional)
- Email input (required, with validation)
- Subscribe button
- Success/error message display

#### Frontend Validation
- Email format validation
- Duplicate subscription prevention
- Real-time feedback messages

#### Database Storage
- Saves to Firestore `subscribers` collection
- Fields: `email`, `name`, `subscribedAt` (ISO timestamp), `source` (page URL)
- Prevents duplicate emails automatically

## Firestore Data Structure

### Collections Created

#### `customers`
```
{
  name: string,
  email: string,
  phone: string | null,
  totalSpend: number,
  orderCount: number,
  lastOrderDate: timestamp | null,
  firstOrderDate: timestamp | null,
  memberStatus: "member" | "non-member",
  tags: string[],
  orders: array (order history),
  notes: string
}
```

#### `campaigns`
```
{
  date: ISO timestamp,
  subject: string,
  segment: string (all, vip, recent, atrisk, members, subscribers),
  recipients: number,
  successCount: number,
  failureCount: number,
  status: "Sent" | "Partial" | "Draft",
  body: string
}
```

#### `subscribers`
```
{
  email: string (unique),
  name: string | null,
  subscribedAt: ISO timestamp,
  source: string (page URL)
}
```

## Security Rules
**File**: `firestore.rules`

### Access Control
- **Admin Only** (read/write): products, orders, applications, members, customers, campaigns, announcements, drops
- **Public Create Only** (read/write admin): subscribers collection
- **Default Deny**: All other access blocked

### Admin Authentication
- Uses Firebase Authentication with hardcoded admin UID
- Update `YOUR_ADMIN_UID_HERE` in firestore.rules with actual admin UID from Firebase Console

### To Deploy Rules
1. Install Firebase CLI: `npm install -g firebase-tools`
2. Login: `firebase login`
3. Deploy: `firebase deploy --only firestore:rules`

Alternatively, update rules directly in Firebase Console > Firestore > Rules tab

## Technical Implementation

### JavaScript Features
- **CRM Subtabs**: Toggle between Customers and Campaigns views
- **Real-time Segment Counting**: Automatic calculation of segment sizes
- **Merge Tag Preview**: Live preview of emails with sample data
- **Customer Notes**: Save/update internal notes per customer
- **Campaign History**: Persistent storage and retrieval from Firestore

### Integration Points
- **EmailJS**: Uses existing `emailjs.send()` with custom message template
- **Firebase Auth**: Existing admin authentication layer
- **Firestore**: Real-time database for all CRM data
- **Responsive Design**: Works on desktop and mobile

## Configuration Notes

### EmailJS Setup
The system uses the existing EmailJS configuration:
- Service ID: `service_hpy2gvj`
- Public Key: `Se-YqzqLV7Zr-AYVt`
- Template: Uses `EMAILJS_CUSTOM_MSG_TEMPLATE` for campaign emails

### Firebase Configuration
Uses existing Firebase config from dashboard.html:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDgpvF9rIy44h4whE7mon7m0mD-bV21kEg",
  authDomain: "dirtboxclub-3d7a7.firebaseapp.com",
  projectId: "dirtboxclub-3d7a7",
  storageBucket: "dirtboxclub-3d7a7.firebasestorage.app",
  messagingSenderId: "883493757389",
  appId: "1:883493757389:web:749f668e2655fd168fe484"
};
```

## Files Modified

1. **dashboard.html** (1128 lines added)
   - Added CRM tab and subtabs
   - Added customer management interface
   - Added email campaign builder
   - Added comprehensive CRM JavaScript functionality
   - Added CRM-specific CSS styles

2. **index.html** (84 lines added)
   - Enhanced newsletter form with IDs and event handling
   - Added Firebase module imports
   - Added newsletter subscription handler
   - Added validation and duplicate prevention

3. **firestore.rules** (NEW)
   - Complete Firestore security rules for all collections
   - Admin-only access for sensitive collections
   - Public write access for newsletter signups

## Usage Instructions

### For Admin Users

#### Managing Customers
1. Login to admin dashboard at `/dashboard.html`
2. Click "💌 CRM" tab
3. View customer list with search and filters
4. Click any customer to view detailed profile and edit notes

#### Viewing Customer Segments
1. In CRM tab, scroll to "Customer Segments" section
2. View live counts for each segment
3. Click any segment card to filter the customer list

#### Sending Email Campaigns
1. In CRM tab, click "📧 Campaigns" subtab
2. Select target segment from dropdown
3. Enter subject line and message body
4. Use merge tags like `{{name}}`, `{{email}}`, etc.
5. Preview email in real-time panel
6. Click "Send Campaign" to deliver to all recipients
7. View campaign history below

### For Website Visitors

#### Newsletter Signup
1. Scroll to footer on homepage
2. Enter name (optional) and email address
3. Click "Subscribe" button
4. See confirmation message
5. Email is saved to Firestore and available for campaigns

## Testing Checklist

- [x] CRM tab appears in admin dashboard
- [x] Customer list loads and displays correctly
- [x] Segment counts update in real-time
- [x] Search filters work by name/email
- [x] Advanced filters work (spend, order count, status)
- [x] Customer detail modal opens and shows info
- [x] Notes field saves to Firestore
- [x] Campaign segment selector populates correctly
- [x] Campaign preview updates with merge tags
- [x] Recipient count updates when segment changes
- [x] Email campaigns send via EmailJS
- [x] Campaign records save to Firestore
- [x] Newsletter signup form appears in footer
- [x] Email validation works
- [x] Duplicate signup prevention works
- [x] Subscribers save to Firestore
- [x] Success/error messages display

## Performance Considerations

- **Firestore Queries**: Optimized with `query()` and `orderBy()`
- **Pagination**: Not implemented yet (suitable for ~1000 customers)
- **Real-time Updates**: Not live (loads on tab switch)
- **Email Sending**: Sequential (suitable for ~500 recipients per campaign)

For larger subscriber bases, consider:
- Adding pagination to customer list
- Implementing real-time Firestore listeners
- Using Cloud Functions for bulk email campaigns

## Future Enhancements

1. **Customer Tags**: UI to add/remove custom tags
2. **Bulk Actions**: Select multiple customers for batch operations
3. **Campaign Templates**: Save campaign templates for reuse
4. **Analytics**: Campaign open rates, click tracking
5. **Scheduled Campaigns**: Schedule campaigns for future sending
6. **Segments UI**: Create custom segments with advanced rules
7. **Import/Export**: Bulk customer import from CSV
8. **Webhooks**: Auto-sync customer data from shop orders

## Deployment

All changes have been pushed to GitHub:
- **Repository**: https://github.com/fitch83/dirtboxclub
- **Latest Commit**: `ba46e56` - Add marketing CRM, customer segments, email campaigns, newsletter signup
- **Branch**: main

The live site is deployed at: https://fitch83.github.io/dirtboxclub/

## Support

For questions about the CRM system:
1. Check Firestore security rules in `firestore.rules`
2. Review JavaScript functions in `dashboard.html` (lines 2300+)
3. Check Firebase config matches project credentials
4. Verify EmailJS templates exist in your account
5. Ensure admin user UID is correctly set in Firestore rules

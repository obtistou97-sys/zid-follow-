# PRD: ZidFollow – Social Media Ordering Platform

## 1. Overview
ZidFollow is a bilingual (Arabic/French) single-page web application for ordering social media services (followers, likes, views) for Instagram, Facebook, and TikTok. It includes a customer-facing order page and a password-protected admin dashboard.

**Live URL:** https://wonderful-blancmange-76f601.netlify.app/  
**Repository:** `C:\Users\user\Desktop\demo1`

---

## 2. Goals
- Provide a simple, mobile-friendly order form for Arabic and French speaking users
- Allow admin to manage orders, edit site text, configure EmailJS for notifications, and optionally send orders to an external CRM
- Deploy as a static site (no backend server required)

---

## 3. Target Audience
- Arabic-speaking users in Algeria and North Africa (primary)
- French-speaking users (secondary)
- Admin/operator managing the business

---

## 4. Features

### 4.1 Customer-Facing (`index.html`)
| Feature | Description |
|---------|-------------|
| Language toggle | Switch between Arabic (RTL) and French (LTR) |
| Theme toggle | Dark/light mode via `data-theme` attribute |
| Services grid | 2-column grid: Instagram Followers, Instagram Likes, Facebook Followers, Facebook Page Likes, TikTok Views, TikTok Likes |
| Quantity/price grid | Shows available quantities and prices for selected service; 3-column desktop, 2-column mobile |
| "Most ordered" badge | Red pulsing badge on 10,000 qty for Instagram Followers and Facebook Followers |
| Discount display | Strikes through old price next to current price on 10,000 qty |
| Order form | Collects full name, username, phone, wilaya (state), payment method |
| WhatsApp integration | After order, sends order details to admin's WhatsApp number |
| Google Sheets backup | POSTs order data to a Google Apps Script URL if configured |
| CRM webhook | POSTs JSON order data to an external CRM URL if configured |
| Local orders | Saves order to localStorage for admin dashboard display |
| Editable text | `data-edit` attributes allow admin to change any text live |

### 4.2 Admin Dashboard (`admin.html`)
| Feature | Description |
|---------|-------------|
| Password login | Randomly generated password stored in localStorage |
| Dashboard stats | Total orders, total revenue, today's orders |
| Orders table | Lists all orders with status, delete, and mark-as-contacted actions |
| Text editor | Edit any `data-edit` field on the main page |
| EmailJS config | Configure EmailJS service/template/key for order notifications and password changes |
| WhatsApp number | Set the WhatsApp number for order notifications |
| Google Sheets URL | Set the Google Apps Script webhook URL |
| CRM API URL + Key | Set an external CRM endpoint to receive orders |
| Admin email | Email for password change verification |
| Password change | Sends verification code via EmailJS if configured, otherwise changes directly |

---

## 5. File Structure
```
demo1/
├── index.html       (single-file app: HTML + CSS + JS)
├── admin.html       (admin dashboard)
├── PRD.md           (this document)
└── icons/
    ├── logo.png
    ├── instagram-icon.png
    ├── facebook-icon.png
    ├── tiktok-icon.png
    ├── baridimob.png
    ├── binance-pay.png
    ├── ccp.webp
    ├── redotpay.png
    └── tether-logo.webp
```

---

## 6. Technical Stack
- **Frontend:** Vanilla HTML, CSS, JavaScript (no frameworks)
- **Storage:** Browser localStorage (admin config, password, orders)
- **Notifications:** EmailJS (email), WhatsApp link (chat)
- **Deployment:** Netlify Drop (static hosting)
- **External integrations:** Google Sheets (no-cors POST), CRM webhook (POST JSON)

---

## 7. Data Flow (Order Submission)
1. Customer fills form → clicks "إرسال / Envoyer"
2. Order data written to localStorage (`zidfollow_orders`)
3. If Google Sheets URL configured → POST (no-cors) to Apps Script
4. If CRM URL configured → POST JSON with X-API-Key header
5. WhatsApp link opened with order summary to admin's number

---

## 8. Admin Configuration (localStorage keys)
| Key | Purpose |
|-----|---------|
| `zidfollow_admin_pass` | Admin password (auto-generated) |
| `zidfollow_config` | JSON: wa-number, backup-url, crm-url, crm-key, admin-email, ej-service, ej-template, ej-key, plus all data-edit text overrides |
| `zidfollow_orders` | Array of order objects |
| `zidfollow_last_order` | Timestamp of most recent order |

---

## 9. Constraints & Design Decisions
- All code in two HTML files (no bundler, no build step)
- RTL by default, LTR when French selected
- Arabic text stored in `data-ar`, French in `data-fr`
- Editable text uses `data-edit` attribute name as key in config
- Icons use hyphenated filenames for web compatibility
- No database; data persists in browser only (cleared if user clears site data)
- Admin password regenerates if localStorage is cleared

---

## 10. Known Limitations
- Orders are stored per-browser (no central database without CRM/Sheet integration)
- Admin password is local-only; no multi-admin support
- No payment processing (payment info is discussed via WhatsApp)
- File `index.html` is ~400KB due to inline CSS/JS

---

## 11. Future Opportunities
- Add payment gateway integration (CCI, BaridiMob)
- Add real-time order notifications via WebSocket or polling
- Migrate to a backend (Node.js/Firebase) for persistent multi-admin data
- Add order status tracking for customers
- Add more social platforms (YouTube, Twitter, Telegram)

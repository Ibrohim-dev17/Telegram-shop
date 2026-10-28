# Telegram Web App do'kon — o'rnatish va ishga tushirish

Bu paket to'liq ishlaydigan Telegram Web App do'konni o'z ichiga oladi:
- **Sayt** (`index.html`, `cart.html`, `product.html`, `profil.html` va boshqalar, `script.js`, `style.css`)
- **Admin panel** (`admin.html` va `admin-*.html/js/css` fayllari)
- **Server** (`server.js`, Node.js 18+, `pg` kutubxonasi)
- **Boshlang'ich ma'lumot** (`database.json`, namunaviy kategoriyalar va mahsulotlar)

## 1. Fayllar tuzilishi
Hamma fayllar bitta papkada turadi. Server shu papkadan sayt va admin fayllarini o'zi uzatadi.
`server.js`, `package.json`, `database.json` (va `data.json`) brauzerdan yashirin.

## 2. Telegram bot yaratish
1. Telegram'da **@BotFather** ga yozing, `/newbot` buyrug'ini yuboring.
2. Bot nomi va username kiriting. BotFather bergan **token**ni saqlang.
3. Bot username'ini (masalan `@sizning_bot`) eslab qoling. Bu `BOT_USERNAME` bo'ladi.

## 3. Ma'lumotlar bazasi (tavsiya etiladi)
Render bepul rejimida fayl diski har qayta ishga tushganda tozalanishi mumkin. Shuning uchun Neon (PostgreSQL) ulanishi tavsiya etiladi:
1. neon.tech da loyiha oching, **connection string** (`postgresql://...`) ni nusxalang.
2. Uni `DATABASE_URL` sifatida Render'ga kiriting. Server jadvallarni o'zi yaratadi.

## 4. Render'ga joylash
1. Render'da **New → Web Service** tanlang, kodni GitHub'ga yuklang yoki papkani ulang.
2. **Runtime:** Node
3. **Build Command:** `npm install`
4. **Start Command:** `npm start`
5. **Environment Variables** bo'limiga quyidagilarni kiriting:

| Nomi | Nima uchun | Majburiy |
|---|---|---|
| `BOT_TOKEN` | BotFather bergan token | Ha |
| `ADMIN_KEY` | Admin paneliga kirish kaliti (uzun, tasodifiy matn, kamida 24 belgi) | Ha |
| `SECRET_KEY` | Sessiya kaliti (uzun tasodifiy matn) | Tavsiya etiladi |
| `BOT_USERNAME` | Bot username'i, `@`siz (masalan `sizning_bot`) | Ha |
| `OWNER_CHAT_ID` | Do'kon egasining Telegram chat ID si (buyurtma xabarlari uchun) | Tavsiya etiladi |
| `DATABASE_URL` | Neon PostgreSQL ulanish satri | Tavsiya etiladi |
| `PUBLIC_URL` | Sayt manzili, masalan `https://sizning-servis.onrender.com` (Render'da odatda avtomatik) | Yo'q |
| `CLOUDINARY_CLOUD_NAME`, `CLOUDINARY_UPLOAD_PRESET` | Katta video va rasmlar uchun (ixtiyoriy) | Yo'q |
| `CORS_ORIGIN` | Saytni boshqa domendan ochsangiz, shu domen | Yo'q |
| `RESEND_API_KEY` yoki `BREVO_API_KEY`, `EMAIL_FROM` | Email xabarlar uchun (ixtiyoriy) | Yo'q |

6. Deploy qiling. Brauzerda `https://<servis>/api/health` oching. `"ok": true` va `"telegram": true` chiqishi kerak.
7. Server ishga tushganda Telegram bot uchun "Do'kon" menyu tugmasi avtomatik sozlanadi.

## 5. Sayt manzili sozlamasi
Sayt va server bitta Render servisida bo'lsa, `script.js` ichida `CONFIG.API_BASE` **bo'sh** qoladi (paketda shunday).
Agar sayt boshqa joyda (masalan GitHub Pages) turgan bo'lsa, `script.js` ichida `API_BASE` ga server manzilini yozing.

## 6. Admin panel
- Manzil: `https://<servis>/admin.html`
- Kirish uchun `ADMIN_KEY` ni kiriting.
- Mahsulot, kategoriya, banner, buyurtma, chat, hisobotlar va sozlamalar shu yerda.
- Sozlamalar → **Zaxira nusxa** bo'limida hamma ma'lumotni faylga saqlab qo'ying.

## 7. Xavfsizlik
- **Tokenlar va kalitlarni hech qachon kodga yoki GitHub'ga yozmang.** Hammasi faqat Render "Environment Variables" da turadi.
- Token yoki parolni biror joyga tasodifan chiqarib qo'ygan bo'lsangiz, darhol BotFather'da `/revoke` qiling va yangi `ADMIN_KEY` o'rnating.
- `database.json` namunaviy ma'lumot. Haqiqiy do'kon uchun uni o'z mahsulotlaringiz bilan almashtiring yoki admin paneldan kiriting.

## 8. Muammolarni tekshirish
- `/api/health` ishlamasa: Render **Logs** ni oching, `BOT_TOKEN` va `DATABASE_URL` to'g'ri kiritilganini tekshiring.
- Bot javob bermasa: `PUBLIC_URL` to'g'ri ekanini va webhook logida `[Telegram] bot: ... webhook: OK` yozuvi borligini tekshiring.
- Admin kirmasa: `ADMIN_KEY` ni nusxalashda bo'sh joy qolmaganini tekshiring.

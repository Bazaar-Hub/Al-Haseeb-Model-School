# Al Haseeb Model Sec. School – AHMSS Website

Ye sari files **ek hi folder** mein hain — koi alag `css/` ya `pages/` subfolder nahi. Bas is folder ko upload/deploy kar dein.

## Admin Login
- Username: `Al Haseeb Database`
- Password: `123456`

## Files
- `index.html` → Public homepage
- `admin-login.html` → Admin login
- `admin-dashboard.html` → Full admin panel
- `parent-register.html` → Parent register
- `parent-login.html` → Parent login
- `parent-portal.html` → Submit admission
- `main.css` → All styles (single shared stylesheet)
- `logo.png` → School logo (referenced by every page, no more embedded base64)
- `firestore.rules`, `firebase.json` → Firebase config

## GitHub Upload Steps
1. Go to your GitHub repo
2. Click "uploading an existing file"
3. Drag **all files** from this folder (don't create any subfolders)
4. Click "Commit changes"

## Firebase Hosting (Live URL)
```
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```
Your site: https://al-haseeb-model-sec-school.web.app

## Cleanup notes
- `admin-login-fixed.html` ko hata kar usi ko `admin-login.html` bana diya gaya hai (purani buggy copy hata di gayi).
- Har page mein bar bar repeat ho rahi logo ki base64 image hata kar `logo.png` file use ki gayi hai — isse files bohot halki ho gayi hain aur load bhi tezi se hota hai.
- Design mein hover shine effect, smooth fade-in animation aur behtar focus states add kiye gaye hain.

## Phone (OTP) Verification on Parent Register
`parent-register.html` ab 2 steps mein kaam karta hai:
1. **Phone verify** — parent apna number daalta hai, Firebase SMS se 6-digit code bhejta hai, code enter karke verify karta hai.
2. **Account create** — phone verify hone ke baad hi Name/Email/Password wala form khulta hai aur account banta hai. Verified phone number Firestore mein `parents/{uid}` doc ke andar `phone` aur `phoneVerified:true` ke sath save hota hai.

### Zaroori Firebase Console setup (one-time)
1. Firebase Console → **Authentication → Sign-in method** → **Phone** provider ko **Enable** karein.
2. **Authentication → Settings → Authorized domains** mein apna hosting domain (e.g. `al-haseeb-model-sec-school.web.app`) zaroor add ho — by default Firebase hosting domain already authorized hota hai.
3. Test ke liye localhost pe bhi chalega (`localhost` already authorized domain hota hai by default).
4. Free Spark plan par bhi Phone Auth kaam karta hai, lekin har number par daily/monthly SMS quota hoti hai — zyada testing ke liye Firebase Console mein **Phone numbers for testing** add kar sakte hain (bina real SMS bheje verify ho jata hai).

### "auth/internal-error" milay to ye check karein
Ye error tab aata hai jab Firebase request ko reject kar deta hai — generic SDK error hai, neeche di gayi cheezein order mein check karein:

1. **Authorized domain missing** — Console → Authentication → Settings → **Authorized domains** mein jis domain/URL se site khol rahe ho wo add hona chahiye. Localhost test ke liye `localhost` default add hota hai, lekin agar koi custom local server/IP (jaise `127.0.0.1` ya kisi LAN IP) use kar rahe ho to wo bhi yahan add karna parega.
2. **File ko seedha double-click karke khola hai (`file:///...`)** — ye sabse common wajah hai. Phone Auth/reCAPTCHA `file://` protocol pe kabhi nahi chalta. Local testing ke liye `firebase emulators:start --only hosting` ya kisi simple local server (`python3 -m http.server`) se kholna zaroori hai, ya phir seedha `firebase deploy` karke live URL pe test karein.
3. **Identity Toolkit API disabled** — Google Cloud Console → APIs & Services mein project ke liye "Identity Toolkit API" enabled honi chahiye (Firebase Phone Auth enable karte waqt ye usually khud ho jati hai, lekin confirm kar lein).
4. **reCAPTCHA Enterprise key mismatch** — agar Firebase project naya hai to App Check / reCAPTCHA Enterprise involved ho sakta hai. Console → Authentication → Settings → **App verification** section check karein; agar reCAPTCHA Enterprise enforce ho raha hai aur key sahi se configure nahi hai to internal-error aata hai.
5. Browser console (F12 → Console tab) khol kar dekhein — humne code mein `console.error('OTP send error:', e)` add kiya hai, wahan poora error object dikhega jisme zyada detail milegi.
6. **"SMS unable to be sent until this region enabled by the app developer"** — ye bhi `auth/operation-not-allowed` ke sath aata hai. Fix: Console → Authentication → Sign-in method → **Phone** provider open karein → neeche **SMS region policy** section mein **Pakistan (PK)** ko allow list mein add karein (ya "Allow all regions" select karein).
7. Naye Firebase projects mein by default **10 SMS/day** ki limit hoti hai jab tak billing account add na ho — testing zyada karni ho to Phone provider ke andar hi **"Phone numbers for testing"** mein fake number + fake code add kar sakte hain.


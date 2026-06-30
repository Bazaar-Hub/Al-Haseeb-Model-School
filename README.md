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

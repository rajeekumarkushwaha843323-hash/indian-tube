# Vicky Flix — Android + Backend Starter

यह एक starter source project है, तैयार/साइन किया हुआ APK नहीं। इसमें Android Kotlin ऐप और Node.js backend का प्रारंभिक कोड है।

## क्या शामिल है
- Android app (Kotlin + Jetpack Compose)
- मूवी कैटलॉग API से लोड करना
- AndroidX Media3 player से स्ट्रीम चलाना
- Admin login और मूवी metadata/video upload API
- लोकल वीडियो स्टोरेज (केवल development के लिए)
- अधिकार-आधारित upload policy और copyright report endpoint का आधार

## आवश्यकताएँ
- Android Studio (हाल का stable version)
- JDK 17
- Android SDK (compile SDK 35)
- Node.js 20+ और npm

## 1) Backend चलाएँ
```bash
cd backend
cp .env.example .env
npm install
npm start
```
`.env` में `ADMIN_USERNAME`, `ADMIN_PASSWORD`, `JWT_SECRET` और `PUBLIC_BASE_URL` सेट करें।
Development में वीडियो `backend/uploads/` में सहेजे जाते हैं। Production में persistent disk या S3-compatible object storage/CDN का उपयोग करें। Render जैसी ephemeral filesystem सेवा पर स्थानीय uploads खो सकते हैं।

Backend डिफ़ॉल्ट रूप से `http://10.0.2.2:8080` पर Android Emulator से उपलब्ध है। असली फोन के लिए `app/build.gradle.kts` में `API_BASE_URL` को अपने HTTPS backend URL से बदलें।

## 2) Android ऐप खोलें
Android Studio में `android/` फ़ोल्डर खोलें और Gradle sync करें। `app/build.gradle.kts` में `API_BASE_URL` को अपने deployed HTTPS API URL से बदलें। Emulator के लिए default URL `http://10.0.2.2:8080/` है।

## 3) Admin upload
Backend के `/api/admin/login` पर username/password से token लें, फिर `Authorization: Bearer <token>` के साथ `POST /api/admin/movies` पर multipart fields भेजें:
- `title` (required)
- `description`
- `posterUrl` (HTTPS URL)
- `video` (MP4/WebM, max size backend config के अनुसार)
- `rightsConfirmed=true` (required)

## सुरक्षा और रिलीज़ से पहले जरूरी काम
- डेमो admin password बदलें; `.env` को कभी commit न करें।
- HTTPS, rate limiting, secure persistent database, object storage/CDN, signed URLs और backup लगाएँ।
- अभी metadata JSON फ़ाइल में है; multi-user production use से पहले PostgreSQL जैसे database में बदलें।
- Admin moderation, copyright takedown workflow, abuse reporting, terms of service और live privacy policy तैयार करें।
- केवल अपने या लिखित अनुमति वाले वीडियो अपलोड करें।
- Google/Indus के current publishing rules देखें; privacy policy URL, app screenshots, store listing और signed APK/AAB तैयार करें।

## GitHub Actions से APK बनाना (मोबाइल से)
1. इस ZIP को extract करके इसकी सभी files/folders को GitHub repository के root में upload करें। `.github/workflows/android-apk.yml` फ़ाइल भी शामिल है।
2. GitHub repository खोलें → **Actions** tab → यदि workflow prompt दिखे तो enable करें।
3. **Build Vicky Flix APK** workflow चुनें → **Run workflow** दबाएँ।
4. Build सफल होने पर run खोलें → **Artifacts** में `vicky-flix-debug-apk` डाउनलोड करें → ZIP extract करके `app-debug.apk` लें।
5. यह testing/debug APK है, signed release APK/AAB नहीं। Store publication के लिए release signing, privacy policy, store listing और app review अभी अलग से करने होंगे।

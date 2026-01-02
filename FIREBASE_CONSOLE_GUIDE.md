# Firebase Console Kontrol Rehberi

## 🔍 Hızlı Kontrol Listesi

### ✅ 1. Authentication - Email/Password Kontrolü

**Adımlar:**
1. [Firebase Console](https://console.firebase.google.com/) açın
2. Projenizi seçin: **whoboom-b2c29**
3. Sol menüden **Authentication** → **Sign-in method** sekmesine gidin
4. **Email/Password** satırını bulun
5. **Enable** toggle'ının açık olduğundan emin olun
6. **Save** butonuna tıklayın

**Beklenen Durum:**
```
✅ Email/Password → Enabled
```

---

### ✅ 2. Realtime Database Kontrolü

**Adımlar:**
1. Sol menüden **Realtime Database** → **Create database** (eğer yoksa)
2. Location seçin (örn: `europe-west1`)
3. **Start in test mode** seçin (daha sonra kuralları güncelleyeceğiz)
4. **Rules** sekmesine gidin
5. Şu kuralları yapıştırın ve **Publish** butonuna tıklayın:

```json
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null",
    "users": {
      "$userId": {
        ".read": "$userId === auth.uid || true",
        ".write": "$userId === auth.uid || !data.exists()"
      }
    },
    "tweets": {
      ".read": "auth != null",
      ".write": "auth != null",
      "$tweetId": {
        ".read": true,
        ".write": "auth != null && (!data.exists() || data.child('userId').val() === auth.uid)"
      }
    },
    "chats": {
      "$conversationId": {
        ".read": "auth != null && (data.child('userId1').val() === auth.uid || data.child('userId2').val() === auth.uid)",
        ".write": "auth != null && (data.child('userId1').val() === auth.uid || data.child('userId2').val() === auth.uid)"
      }
    },
    "notifications": {
      "$userId": {
        ".read": "$userId === auth.uid",
        ".write": "auth != null"
      }
    },
    "followers": {
      ".read": "auth != null",
      ".write": "auth != null"
    },
    "following": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

**Beklenen Durum:**
```
✅ Database oluşturuldu
✅ Rules ayarlandı ve publish edildi
✅ Database URL mevcut
```

---

### ✅ 3. Storage Kontrolü

**Adımlar:**
1. Sol menüden **Storage** → **Get started** (eğer yoksa)
2. Location seçin (Database ile aynı bölgeyi seçin)
3. **Start in test mode** seçin
4. **Rules** sekmesine gidin
5. Şu kuralları yapıştırın ve **Publish** butonuna tıklayın:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    // Kullanıcı profil resimleri
    match /users/{userId}/profile/{fileName} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Kullanıcı kapak resimleri
    match /users/{userId}/cover/{fileName} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Tweet görüntüleri
    match /tweets/{tweetId}/{fileName} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    // Chat görüntüleri
    match /chats/{conversationId}/{messageId}/{fileName} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
  }
}
```

**Beklenen Durum:**
```
✅ Storage oluşturuldu
✅ Rules ayarlandı ve publish edildi
```

**Not:** Storage kullanmak için `pubspec.yaml`'a `firebase_storage: ^12.1.0` ekleyip `flutter pub get` çalıştırmanız gerekecek.

---

## 🧪 Test Senaryoları

### Test 1: Authentication
1. Uygulamayı çalıştırın: `flutter run`
2. "Kayıt Ol" butonuna tıklayın
3. Email ve şifre ile yeni hesap oluşturun
4. Firebase Console → Authentication → Users'da yeni kullanıcıyı görmelisiniz

### Test 2: Database
1. Kayıt olduktan sonra uygulamada bir tweet gönderin
2. Firebase Console → Realtime Database → Data sekmesinde `tweets` node'unu kontrol edin
3. Tweet'inizi görmelisiniz

### Test 3: Storage (Paket eklendikten sonra)
1. Profil düzenleme sayfasından profil resmi yükleyin
2. Firebase Console → Storage'da `users/{userId}/profile/` klasörünü kontrol edin

---

## ⚠️ Önemli Uyarılar

1. **Test Mode Kuralları**: Şu anki kurallar geliştirme için. Production'a geçmeden önce mutlaka sıkılaştırın!

2. **Location Seçimi**: Database ve Storage için aynı location'ı seçmeniz önerilir (performans için).

3. **Quotas**: Firebase ücretsiz planında limitler var. Kontrol edin:
   - Authentication: 50K MAU (Monthly Active Users)
   - Database: 1GB storage, 10GB/month transfer
   - Storage: 5GB storage, 1GB/day transfer

---

## 📊 Kontrol Tablosu

| Servis | Oluşturuldu mu? | Kurallar Ayarlandı mı? | Test Edildi mi? |
|--------|-----------------|------------------------|-----------------|
| Authentication | ☐ | ☐ | ☐ |
| Realtime Database | ☐ | ☐ | ☐ |
| Storage | ☐ | ☐ | ☐ |

---

## 🔗 Hızlı Erişim

- [Firebase Console](https://console.firebase.google.com/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Firebase Pricing](https://firebase.google.com/pricing)


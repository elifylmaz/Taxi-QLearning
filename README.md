# 6×6 Taxi Ortamında Optimize Q-Learning  

## 📌 Özet  
Bu çalışmada klasik *Taxi-v3* ortamı, 6×6 boyutunda engeller içerecek şekilde yeniden tasarlanmış, **action masking** uygulanmış ve durum uzayı sadeleştirilerek 622.080 durumdan **47.952** duruma düşürülmüştür.  
5 milyon episodluk Q-Learning eğitimi sonucunda ajan **ortalama 12 adımda** görevi tamamlamayı öğrenmiş ve en iyi ortalama ödül **8.99** olarak elde edilmiştir.

---

## 1. Giriş  
Taxi-v3 ortamı küçük yapısı nedeniyle tabular yöntemlerle kolay çözülebilmektedir.  
Bu çalışmada daha zorlaştırılmış bir **6×6 grid-world** tasarlanmış, haritaya engeller ve duvarlar eklenmiş, başlangıç koşulları dinamik hale getirilmiştir.  

Bu tasarım ile hedef:  
- Action masking’in,  
- Durum uzayı sadeleştirmenin  

tabular Q-Learning performansına etkisini incelemektir.

---

## 2. Ortam Tasarımı

### 🗺️ Harita Özellikleri
- 6×6 grid  
- **3 yasak hücre**, **4 sabit duvar**
- Yolcu ve hedef her reset’te rastgele belirlenir
- Pickup/dropoff tüm geçerli hücrelerde yapılabilir
- **Action masking**: duvara çarpma ve geçersiz pickup/dropoff engellenir

### 🎮 Aksiyonlar (6 adet)
- Yukarı  
- Aşağı  
- Sol  
- Sağ  
- Pickup  
- Dropoff  

### 🎯 Ödüller
- Normal adım: **–1**  
- Geçersiz hareket: **–10**  
- Başarılı bırakma: **+20**

---

## 3. Durum Uzayı  
Yolcu taksideyken konumu ayrıca tutulmadığı için:

- **Yolcu dışarıda:**  
  - 6² × 6² = **46.656**  
- **Yolcu takside:**  
  - 6² × 6² = **1.296**  

### ✔️ Toplam  
**47.952 durum**  
Bu sadeleştirme durum uzayını yaklaşık **13 kat küçültmüştür.**

---

## 4. Yöntem  

- **Algoritma:** Q-Learning  
- **Hiperparametreler:**  
  - α = 0.1  
  - γ = 1.0  
  - ε = 0.1  
- **Eğitim Süreci:**  
  - 5.000.000 episod  
  - Her 50.000 episodda performans değerlendirme  
  - Action masking tüm aşamalarda aktif  
- En iyi politika **best_q_table.npy** dosyasına kaydedildi  

---

## 5. Eğitim Sonuçları  

| Episod | Ortalama Ödül | Adım |
|--------|---------------|------|
| 50.000 | –33.26 | 54.3 |
| 100.000 | +3.85 | 17.1 |
| 500.000 | +8.94 | 12.1 |
| 2.350.000 | +8.99 | 12.0 |
| 5.000.000 | +8.98 | 12.0 |

Ajan yaklaşık **150k** episodda görevi çözebilir hale gelmiş,  
**2.3–2.6 milyon** episod aralığında en iyi performansa ulaşmıştır.  

Toplam eğitim süresi: **1 saat 8 dakika 49 saniye**

---

## 6. Test Sonuçları  

| Senaryo | Adım | Ödül |
|---------|------|-------|
| 1 | 25 | +8 |
| 2 | 19 | +12 |
| 3 | 25 | +8 |

Ajan tüm testlerde **engellere takılmadan**, yasak hücrelere girmeden ve **optimuma yakın rotalar** kullanarak görevi başarıyla tamamlamıştır.

---

## 7. Sonuç  
Bu çalışmada iki temel geliştirme uygulanmıştır:

1. **Action masking**  
2. Yolcu taksiye bindiğinde yolcu konumunun durumdan çıkarılması  

Bu iki iyileştirme sayesinde 6×6 engelli Taxi ortamı,  
**5 milyon episod sonunda ortalama 12 adımda çözülebilir** hale gelmiş ve  
**en iyi ortalama ödül 8.99** elde edilmiştir.

Sonuçlar, küçük fakat etkili ön işlemlerin tabular Q-Learning’i halen güçlü ve uygulanabilir kıldığını göstermektedir.

---

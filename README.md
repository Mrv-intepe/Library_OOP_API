### Kütüphane Yönetim Sistemi ve API'si 
Bu proje, temel bir Kütüphane Yönetim Sistemi oluşturmayı ve bunu bir FastAPI web 
API'si aracılığıyla sunmayı amaçlar. Ayrıca, harici bir API (Open Library) kullanarak kitap 
bilgilerini zenginleştirme özelliği de içerir. 
## Bölüm 1: Temel OOP Yapısı Proje 
Kitapları ve kütüphane operasyonlarını yönetmek için Nesne Yönelimli Programlama 
(OOP) prensiplerini kullanır. 
# Book Sınıfı: 
Her bir kitabı temsil eder. title, author ve isbn gibi temel bilgilere 
sahiptir. JSON dönüşümleri için to_dict() ve from_dict() metodları bulunur.  
# Library Sınıfı: 
Kitap koleksiyonunu yönetir. Kitap ekleme, silme, listeleme ve 
arama gibi temel fonksiyonları sağlar. Kitap verilerini kalıcı olarak saklamak için 
library.json adlı bir JSON dosyası kullanır.  
# Çıktı örneği: 
Seçiminizi yapın (1-5): 1 --- Kitap Ekleme --- 
Kitap başlığı: Uçurtma Avcısı 
Yazar adı: Khaled Hosseini 
ISBN numarası: 9789752893962 
Kitap başarıyla eklendi: Uçurtma Avcısı by Khaled Hosseini (ISBN: 9789752893962) 
=== KÜTÜPHANE YÖNETİM SİSTEMİ === 
1. Kitap Ekle 
2. Kitap Sil 
3. Kitapları Listele 
4. Kitap Ara 
5. Çıkış 
=================================== 
Seçiminizi yapın (1-5): 5 
Kütüphane sistemi kapatılıyor. Güle güle 
## Bölüm 2: Harici API Entegrasyonu Proje 
Kitap bilgilerini Open Library API'sinden çekerek veri zenginleştirme yeteneği sunar. 
# APILibrary Sınıfı: 
Library sınıfının genişletilmiş halidir. httpx kütüphanesini kullanarak 
Open Library API'sine (https://openlibrary.org/isbn/{isbn}.json) istek gönderir. 
add_book_by_isbn() metodu, sadece ISBN girerek API'den kitap bilgilerini (başlık, yazar) 
çeker ve kütüphaneye ekler. API iletişimindeki hataları (kitap bulunamadı - 404, bağlantı 
sorunları vb.) yönetir.  
# Çıktı örnekleri:  
Seçiminizi yapın (1-6): 1 --- Kitap Ekleme (API'den) --- 
ISBN numarası: 9789750738609 
API'den kitap bilgileri alınıyor... 
API hatası: 302 
=== KÜTÜPHANE YÖNETİM SİSTEMİ (API Entegrasyonlu) === 
1. Kitap Ekle (ISBN ile API'den) 
2. Kitap Ekle (Manuel) 
3. Kitap Sil 
4. Kitapları Listele 
5. Kitap Ara 
6. Çıkış 
======================================================= 
Seçiminizi yapın (1-6): 1 --- Kitap Ekleme (API'den) --- 
ISBN numarası: 9780140449136 
API'den kitap bilgileri alınıyor... 
ISBN 9780140449136 ile bir kitap zaten mevcut! 
=== KÜTÜPHANE YÖNETİM SİSTEMİ (API Entegrasyonlu) === 
1. Kitap Ekle (ISBN ile API'den) 
2. Kitap Ekle (Manuel) 
3. Kitap Sil 
4. Kitapları Listele 
5. Kitap Ara 
6. Çıkış 
======================================================= 
Seçiminizi yapın (1-6): 6 
Kütüphane sistemi kapatılıyor. Güle güle! 
## Bölüm 3: FastAPI API Oluşturma Proje 
Kütüphane işlevlerini web üzerinden sunmak için bir FastAPI API'si oluşturur. 
# Pydantic Modelleri: 
API'nin girdi ve çıktı verilerinin yapısını tanımlar (BookModel, 
ISBNRequest, BookResponse, MessageResponse, ErrorResponse). Veri doğrulaması ve 
otomatik API dokümantasyonu sağlar. 
# API Endpoint'leri: 
GET /: API hakkında bilgi verir.  
GET /books: Kütüphanedeki tüm kitapları listeler.  
POST /books: ISBN alarak Open Library API'den kitap bilgilerini çeker ve kütüphaneye 
ekler. 
GET /books/{isbn}: Belirtilen ISBN'e sahip kitabı getirir.  
DELETE /books/{isbn}: Belirtilen ISBN'e sahip kitabı siler.  
API, uvicorn ile çalıştırılabilir (uvicorn api:app --reload). Otomatik Swagger/OpenAPI 
dokümantasyonu /docs adresinde bulunur.  
# Çıktı örneği:  
2 kitap yüklendi. 
INFO:     Will watch for changes in these directories: ['/content'] 
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit) 
INFO:     Started reloader process [363] using StatReload 
INFO:     Stopping reloader process [363] 
## Bölüm 4: Testler 
Projenin farklı katmanları için otomatik testler yazılmıştır. 
# Birim Testleri: 
Book, Library, APILibrary sınıflarının fonksiyonlarını test eder.  
# API Testleri: 
FastAPI endpoint'lerinin doğru yanıt verip vermediğini test etmek için 
fastapi.testclient. 
TestClient kullanılır. Bu testler, gerçek bir sunucu başlatmadan uygulama objesi 
üzerinde çalışır. 
# Çıktı örneği: 
0 kitap yüklendi.  
Kitap başarıyla eklendi: Test Book by Test Author (ISBN: 123456789) 
Kitap başarıyla silindi: Test Book by Test Author (ISBN: 123456789)  
ISBN 999999999 ile kitap bulunamadı!  
0 kitap yüklendi.  
Debug: Boş ISBN status code: 422  
Debug: Response text: 
{"detail":[{"type":"string_too_short","loc":["body","isbn"],"msg":"String should have at 
least 1 character","input":"","ctx":{"min_length":1}}]}  
CREATE status: 409  
CREATE response: {"detail":"ISBN 978-0451524935 ile bir kitap zaten mevcut: 1984"} 
Test hatası:  
Mock veya API problemi olabilir - test atlandı  
Tüm testler başarılı! 

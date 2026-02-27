# Kredi Kartı Sahtekarlık (Fraud) Tespiti ve Risk Optimizasyonu
Aşırı dengesiz (imbalanced) veri setlerinde makine öğrenmesi modeli kurup "%99 accuracy aldım" diye sevinir çoğu kişi. Halbuki o %99'luk başarı oranının içinde tek bir dolandırıcı bile yakalanamamıştır muhtemelen. Ben bu projede işin "skor kasma" tarafıyla değil, bankanın kasasından çıkacak parayla ilgilendim daha çok.

Amacım sadece fraud tespiti yapmak değil; masum bir müşterinin kartını boş yere bloke etmenin maliyetiyle, bir dolandırıcıyı gözden kaçırmanın maliyeti arasındaki o hassas dengeyi kurmaktı.

# Problem Ne Kadar Büyük?
Kaggle'ın meşhur Credit Card Fraud veri setiyle çalıştım. Elimdeki 284 bin işlemin sadece 492'si sahteydi (Fraud). Yani oran %0.17 gibi korkunç düşük bir seviyede. Eğer hiç kod yazmayıp "bütün işlemler normaldir" diyen bir fonksiyon çalıştırsaydım bile zaten %99.8 doğru bilecektim. O yüzden projeye başlarken "Accuracy" (Doğruluk) metriğini tamamen çöpe attım.

Görselleştirme aşamasında normal grafikler hiçbir şey ifade etmediği için Y eksenini logaritmik olarak ezmek zorunda kaldım. Gece saatlerinde fraud oranının düştüğü yanılgısını kırdım, veriyi zaman ve tutar (Amount) bazında iyice bir deştim modele geçmeden önce.

# Model Yaklaşımım: Neden SMOTE Kullanmadım?
Bu tarz projelerde herkesin ilk aklına gelen şey azınlık sınıfı çoğaltmaktır (SMOTE vs.). Ama sentetik dolandırıcı verisi üretmenin, gerçek dünyadaki anormallik kalıplarını bozacağını düşünüyorum finansal sistemlerde. Veri uydurmak yerine, cezalandırma mantığına gittim.

Veriyi Stratified Split ile böldükten sonra XGBoost modelini kurdum ve içine scale_pos_weight parametresini gömdüm. Modele aslında şunu söyledim: "Masum müşteriyi yanlışlıkla engellersen çok kızmam ama bir dolandırıcıyı kaçırırsan cezan çok büyük olur."

Modeli değerlendirirken de yanıltıcı olan ROC-AUC yerine, doğrudan Precision-Recall (PR-AUC) eğrisine odaklandım.

# Çıktılar ve İş Simülasyonu (ROI)
Test seti üzerinde PR-AUC skorum 0.853 geldi. Sıfıra yakın dengesizlikteki bir veri için epey sağlam bir yakalama oranı bu.

Ama asıl şov kısmı teknik metrikler değil. Basit bir kâr/zarar (Cost Matrix) simülasyonu yazdım test setinin üstüne:

Yanlış alarm verip müşteriyi kızdırmanın operasyonel maliyetini işlem başı 10$ aldım.

Kaçan dolandırıcının faturasını ise ortalama sepet tutarı olan 122$ olarak belirledim.

Eğer ortada hiçbir model olmasaydı banka o test setinde 11.956$ zarar edecekti. Modeli devreye aldığımızda, o yanlış alarmların yarattığı operasyonel maliyeti de hesaptan düşmemize rağmen bankanın cebinde net 9.548$ para kaldı. Bunu tüm yıla vurduğunuzda ortaya çıkan bütçe optimizasyonu gerçekten muazzam.

# Gelecek Planları ve Limitasyonlar
Tabii her şey bu kadar tozpembe değil canlı ortamda. Aylar geçtikçe dolandırıcılar taktik değiştiriyor (Data Drift). Bu modeli bir kere kurup bankanın köşesine atamayız, sürekli yeni verilerle retrain (yeniden eğitim) edilmesi gerekecek.

Ayrıca bu projede işimi kolaylaştırsın diye XGBoost ve Scikit-Learn gibi kütüphaneleri kullandım. Bir sonraki aşamada bunları daha iyi öğrenip daha yetkin projeler yapmayı hedefliyorum.

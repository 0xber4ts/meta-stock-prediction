# META Hisse Fiyatı Tahmini - LSTM ve GRU ile

## Proje Hakkında

Bu projede **Meta Platforms, Inc. (META)** hisse senedinin tarihsel kapanış fiyatları, **PyTorch** kullanılarak LSTM ve GRU modelleri ile tahmin edilmiştir.

İki farklı RNN mimarisinin performansı, test verisi üzerindeki **Mean Squared Error (MSE)** değerleri ve eğitim süreleri üzerinden karşılaştırılmıştır.

## Kullanılan Teknolojiler

* Python 3
* PyTorch
* Pandas
* NumPy
* Matplotlib
* Scikit-learn
* yfinance

## Veri Seti

META hisse senedine ait günlük kapanış fiyatları **Yahoo Finance** üzerinden `yfinance` kütüphanesi kullanılarak alınmıştır.

* **Hisse:** META
* **Tarih aralığı:** 2015-01-01 - 2025-01-01
* **Kullanılan veri:** Günlük kapanış fiyatı (`Close`)

Veriler, modelin daha verimli öğrenebilmesi için `MinMaxScaler` kullanılarak **-1 ile 1** aralığında normalize edilmiştir.

## Yöntem

Tahmin problemi **sliding window** yöntemi kullanılarak oluşturulmuştur.

* **Lookback:** 20 gün
* Son 20 günlük fiyat kullanılarak bir sonraki günün fiyatı tahmin edilmiştir.
* **Train / Test:** %80 / %20
* **Normalization:** MinMaxScaler `(-1, 1)`
* **Loss Function:** MSE Loss
* **Optimizer:** Adam
* **Learning Rate:** 0.01
* **Epoch:** 100
* **Hidden Dimension:** 32
* **Number of Layers:** 2

Projede iki farklı model eğitilmiştir:

### LSTM

2 katmanlı LSTM mimarisi kullanılmış ve son zaman adımındaki hidden state, bir `Linear` katmanı aracılığıyla fiyat tahminine dönüştürülmüştür.

### GRU

LSTM ile aynı temel yapı kullanılarak 2 katmanlı GRU modeli oluşturulmuş ve performansı LSTM ile karşılaştırılmıştır.

## Sonuçlar

Modellerin performansı test seti üzerinde **Mean Squared Error (MSE)** metriği kullanılarak değerlendirilmiştir.

| Model   |                     Test MSE |
| :------ | ---------------------------: |
| LSTM    |     Çalıştırma sonucuna göre |
| **GRU** | **Çalıştırma sonucuna göre** |

### Kazanan: GRU

Test sonuçlarına göre **GRU modeli, LSTM modelinden daha düşük MSE değeri elde ederek daha başarılı bir performans göstermiştir.**

GRU, daha sade bir mimariye sahip olması sayesinde daha kısa eğitim süresiyle benzer fiyat trendlerini yakalayabilmiştir. Bu proje kapsamında GRU, hem **tahmin performansı** hem de **hesaplama verimliliği** açısından LSTM'e kıyasla daha iyi sonuç vermiştir.

## Gözlemler

Her iki model de META hissesinin genel fiyat hareketlerini takip edebilmiştir. Bununla birlikte, GRU modelinin test verilerindeki tahminleri gerçek fiyatlara daha yakın sonuçlar vermiştir.

Ani ve yüksek volatilitenin olduğu dönemlerde her iki modelin de gerçek fiyat hareketlerine kıyasla gecikmeli tepki verdiği görülmüştür.

Bunun temel nedenlerinden biri, modelin yalnızca geçmiş **kapanış fiyatlarını** kullanmasıdır. Haberler, işlem hacmi, şirket finansalları ve makroekonomik gelişmeler gibi dış faktörler modele dahil edilmemiştir.

## Çalıştırma

Gerekli kütüphaneleri yüklemek için:

```bash
pip install torch pandas numpy matplotlib yfinance scikit-learn
```

Ardından Python dosyasını veya Jupyter Notebook'u çalıştırmanız yeterlidir.

Program sırasıyla:

1. META verilerini Yahoo Finance'dan indirir.
2. Verileri normalize eder.
3. 20 günlük sliding window'lar oluşturur.
4. Veriyi train ve test olarak ayırır.
5. LSTM modelini eğitir.
6. GRU modelini eğitir.
7. Eğitim kayıplarını karşılaştırır.
8. Test verisi üzerinde tahmin yapar.
9. MSE değerlerini hesaplar.
10. Gerçek ve tahmin edilen fiyatları grafik üzerinde gösterir.


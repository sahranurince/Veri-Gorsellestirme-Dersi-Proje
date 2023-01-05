# Veri-Gorsellestirme-Dersi-Proje
Eskişehir Teknik Üniversitesi İstatistik Bölümü lisans programında, 2022-2023 Öğretim Yılı - Güz Dönemi'nde yürütülen Veri Görselleştirme dersi projesinin materyallerini içermektedir.

# Özet
Müzik ruhun gıdasıdır sözü herkes tarafından bilinir. İnsanların bir çoğu günlük zamanının büyük bir kısmını müzik dinlemeye ayırır. Biz de buradan yola çıkarak bu posterde müziğin mental hastalıklara herhangi bir etkisi olup olmadığını çeşitli değişkenler yardımıyla araştırmak istedik. Veri seti Kaggle.com'dan alınmıştır. Bu veri setinde 616 gözlem, 33 değişken vardır. Bu projede kullanılmak için 33 değişkenden 7 tanesi ele alınmıştır. Mental hastalıklar dört ana başlıkta ele alınmıştır. Bunlar; Anksiyete, Depresyon, Uykusuzluk Hastalığı ve Obsesif Kompulsif Bozukluktur. Müziğin etkisi; dinlenilen müzik türü, enstrüman çalma durumu, beste yapma durumu ve günde dinlenilen müzik süresi(saat) olarak ele alınmıştır. Görselleştirme için R Programında; Histogram, Kernel Yoğunluk, Donut ve Saçılım Grafiği ile Dikdörtgen Gruplama Grafikleri oluşturulmuştur.
https://www.kaggle.com/datasets/catherinerasgaitis/mxmh-survey-results?resource=download

# Gerekli Paketlerin İndirilmesi

```
install.packages("ggplot2")     # Veri görselleştirme aracı olarak kullanılır.
library(ggplot2)
install.packages("dplyr")     # Veri üzerinde manipülasyon yapmak için kullanılır.
library(dplyr)
install.packages("tidyr")     # Veri üzerinde düzenlemeler yapmak için kullanılır.
library(tidyr)
install.packages("webr")     # PieDonut Grafiğini çizmek için kullanılır.
library(webr)
install.packages("MetBrewer")     # Grafikler üzerinde renklendirme yapmak için kullanılır.
library(MetBrewer)
library(readr)
```

# Veri Setinin Kullanıma Hazırlanması

## Veri Setinin Çağırılması
```
mxmh_survey_results <- read_csv("mxmh_survey_results.csv")
```
## Yeni Veri Setinin Oluşturulması
```
yeniveriseti <- drop_na(mxmh_survey_results)     # drop_na() fonksiyonu veri setindeki eksik gözlemleri çıkarmak için kullanılır.
```

## Veri Setinin Kontrol Edilmesi
```
head(yeniveriseti)     # head() fonksiyonu veri setinin ilk altı satırını kontrol etmek için kullanılır.
tail(yeniveriseti)     # tail() fonksiyonu veri setinin son altı satırını kontrol etmek için kullanılır.
summary(yeniveriseti)     # summary() fonksiyonu veri setini özetlemek için kullanılır.(Değişkenlerin minimum, maksimum, ortalama, medyan, 1. çeyrek ve 3. çeyrek değerlerini gösterir.)
dim(yeniveriseti)     # dim() fonksiyonu veri setinin satır ve sütun sayılarını gösterir.
colnames(yeniveriseti)     #colnames() fonksiyonu veri setinin sütun isimlerini gösterir.
```

# Grafiklerin Çizilmesi

## Müzik Türlerinin Mental Hastalıklara Etkisi
Müzik türlerinin mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi çoklu çubuk grafiği ile görselleştirilmiştir. Görselleştirilmeden önce değişkenler üzerinde çeşitli işlemler uygulanmıştır.

### Müzik Türlerinin Anksiyeteye Etkisi
İlk olarak veri setini müzik türleri ve anksiyeteye göre gruplanıp, x ismi atanmıştır.

```
x <- yeniveriseti %>%
  group_by(`Fav genre`,Anxiety) %>%
  summarise(n1=n(),n2=n())
```

Burada gruplanan müzik türleri ve anksiyetenin oranları hesaplanıp, x2 ismi atanmıştır.
```
x2 <- x %>%
group_by(`Fav genre`) %>%
  summarize(anxietyoran = sum(Anxiety)/616)
```
Çoklu Çubuk Grafiğinin Çizilmesi
```
 ggplot(x2, aes(fill = `Fav genre`, 
               y = anxietyoran, 
               reorder(x = `Fav genre`,+anxietyoran))) + 
 geom_bar(position = "dodge", 
           stat = "identity")+
 coord_flip()+
  theme_bw()+
  labs(y= "Anksiyete Oranlari",
       x= "Müzik Türleri",
       fill="Müzik Türleri",
       title= "Müzik Türlerinin Anksiyete Üzerindeki Etkisi",
       subtitle="Çoklu Çubuk Grafiği")+
  scale_fill_manual(values= met.brewer("Klimt",16))
```  
![1](https://user-images.githubusercontent.com/91891099/210836350-3d51f5c2-1486-4972-b84f-3df37c130038.png)

### Müzik Türlerinin Depresyona Etkisi
```
y <- yeniveriseti %>%
  group_by(`Fav genre`,Depression) %>%
  summarise(n1=n(),n2=n())

y2 <- y %>%
  group_by(`Fav genre`) %>%
  summarize(Depressionoran = sum(Depression)/616)
```
```
ggplot(y2, aes(fill = `Fav genre`, 
               y = Depressionoran, 
               reorder(x = `Fav genre`,+Depressionoran))) + 
  geom_bar(position = "dodge", 
           stat = "identity")+
  coord_flip()+
  theme_bw()+
  labs(y= "Depresyon Oranlari",
       x= "Müzik Türleri",
       fill="Müzik Türleri",
       title= "Müzik Türlerinin Depresyon Üzerindeki Etkisi",
       subtitle="Çoklu Çubuk Grafiği") +
  scale_fill_manual(values= met.brewer("Klimt",16))
 ``` 
 ![2](https://user-images.githubusercontent.com/91891099/210836504-9de24e7c-d639-4d90-bd45-3233dacfa444.png)

 ### Müzik Türlerinin Uykusuzluk Hastalığına Etkisi
 ```
 v <- yeniveriseti %>%
  group_by(`Fav genre`,Insomnia) %>%
  summarise(n1=n(),n2=n())

v2 <- v %>%
  group_by(`Fav genre`) %>%
  summarize(Insomniaoran = sum(Insomnia)/616)
```
```
ggplot(v2, aes(fill = `Fav genre`, 
               y = Insomniaoran, 
               reorder(x = `Fav genre`,+Insomniaoran))) + 
  geom_bar(position = "dodge", 
           stat = "identity")+
  coord_flip()+
  theme_bw()+
  labs(y= "Uykusuzluk Hastalıgı Oranlari",
       x= "Müzik Türleri",
       fill="Müzik Türleri",
       title= "Müzik Türlerinin Uykusuzluk Hastalığı Üzerindeki Etkisi",
       subtitle="Çoklu Çubuk Grafiği") +
  scale_fill_manual(values= met.brewer("Klimt",16))
```
![3](https://user-images.githubusercontent.com/91891099/210836583-9da136f2-24ab-4d49-a312-4c5f5f0d393b.png)

### Müzik Türlerinin Obsesif Kompulsif Bozukluğa Etkisi
``` 
t <- yeniveriseti %>%
  group_by(`Fav genre`,OCD) %>%
  summarise(n1=n(),n2=n())

t2 <- t %>%
  group_by(`Fav genre`) %>%
  summarize(OCDoran = sum(OCD)/616)
```
```
ggplot(t2, aes(fill = `Fav genre`, 
               y = OCDoran, 
               reorder(x = `Fav genre`,+OCDoran))) + 
  geom_bar(position = "dodge", 
           stat = "identity")+
  coord_flip()+
  theme_bw()+
  labs(y= "Obsesif Kompulsif Bozukluk Oranlari",
       x= "Müzik Türleri",
       fill="Müzik Türleri",
       title= "Müzik Türlerinin Obsesif Kompulsif Bozukluk Hastalığı Üzerindeki Etkisi",
       subtitle="Çoklu Çubuk Grafiği") +
  scale_fill_manual(values= met.brewer("Klimt",16))
```
![4](https://user-images.githubusercontent.com/91891099/210836667-db107f72-8cec-4707-a3af-ab6a0dd58145.png)

#### Sonuç
Bu kısımda elde edilen sonuçlara göre; dört mental hastalığı ele aldığımızda “pop ve rock” müzik türü diğer müzik türlerine göre en yüksek orana sahiptir. Aynı şekilde “latin ve gospel” müzik türü diğer müzik türlerine göre en düşük orana sahiptir. Diğer bir ifadeyle; “pop ve rock” müzik türünün mental hastalıkların seviyesini arttırdığını, “latin ve gospel” müzik türünün ise azalttığını söyleyebiliriz.

## Enstrüman Çalmanın Mental Hastalıklara Etkisi
Enstrüman çalmanın mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi kernel yoğunluk grafiği ile görselleştirilmiştir.

### Enstrüman Çalmanın Anksiyeteye Etkisi
```
ggplot(yeniveriseti, aes(x = Anxiety, fill = Instrumentalist )) +
  geom_density(alpha = 0.5) +
  labs(x = "0-10 Arasi Anksiyete Seviyeleri",
       y = "Yogunluk",
       title = "Enstrüman Çalmanin Anksiyeteye Etkisi",
       subtitle = "Kernel Yogunluk Tahmini",
       fill= "Enstrüman Çalma Durumu")+
  scale_fill_manual(values= met.brewer("Hiroshige",2),
                    labels = c("Hayir", "Evet")) +
  theme_bw()
```
![5](https://user-images.githubusercontent.com/91891099/210836742-eabd4cf6-9bc9-437b-82a6-f0d7acd417bb.png)

### Enstrüman Çalmanın Depresyona Etkisi
```
ggplot(yeniveriseti, aes(x = Depression, fill = Instrumentalist )) +
  geom_density(alpha = 0.5) +
  labs(x = "0-10 Arasi Depresyon Seviyeleri",
       y = "Yogunluk",
       title = "Enstrüman Çalmanin Depresyona Etkisi",
       subtitle = "Kernel Yogunluk Tahmini",
       fill= "Enstrüman Çalma Durumu")+
  scale_fill_manual(values= met.brewer("Cassatt1",2),
                    labels = c("Hayir", "Evet")) +
  theme_bw()
``` 
![6](https://user-images.githubusercontent.com/91891099/210836794-59d2018f-dd49-4a97-a879-26655ce811cc.png)

### Enstrüman Çalmanın Uykusuzluk Hastalığına Etkisi
```
ggplot(yeniveriseti, aes(x = Insomnia, fill = Instrumentalist )) +
  geom_density(alpha = 0.5) +
  labs(x = "0-10 Arasi Uykusuzluk Hastaligi Seviyeleri",
       y = "Yogunluk",
       title = "Enstrüman Çalmanin Uykusuzkuk Hastaligina Etkisi",
       subtitle = "Kernel Yogunluk Tahmini",
       fill= "Enstrüman Çalma Durumu")+
  scale_fill_manual(values= met.brewer("OKeeffe2",2),
                    labels = c("Hayir", "Evet")) +
  theme_bw()
```
![7](https://user-images.githubusercontent.com/91891099/210836842-865adc5d-54ca-45bc-b2a6-67b5379df5d9.png)

### Enstrüman Çalmanın Obsesif Kompulsif Bozukluğa Etkisi
```
ggplot(yeniveriseti, aes(x = OCD, fill = Instrumentalist )) +
  geom_density(alpha = 0.5) +
  labs(x = "0-10 Arasi Obsesif Kompulsif Bozukluk Seviyeleri",
       y = "Yogunluk",
       title = "Enstrüman Çalmanin Obsesif Kompulsif Bozukluga Etkisi",
       subtitle = "Kernel Yogunluk Tahmini",
       fill= "Enstrüman Çalma Durumu")+
  scale_fill_manual(values= met.brewer("Hokusai2",2),
                    labels = c("Hayir", "Evet")) +
  theme_bw()
```
![8](https://user-images.githubusercontent.com/91891099/210836929-7ff4771f-0b7f-4fd1-ab31-c9add2933b88.png)

## Beste Yapmanın Mental Hastalıklara Etkisi
Beste yapmanın mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi Donut Grafiği ile görselleştirilmiştir. Görselleştirilmeden önce değişkenler üzerinde çeşitli işlemler uygulanmıştır.

```
Burada veri setinin sütun adı türkçeye çevrilmiştir.
colnames(yeniveriseti)[7]  <- "BesteYapmaDurumu"

Burada veri setinin sütun içindeki değişkenleri türkçeye çevrilmiştir.
yeniveriseti$BesteYapmaDurumu<- ifelse(yeniveriseti$BesteYapmaDurumu == "No", "Hayir", "Evet")
```

### Beste Yapmanın Anksiyeteye Etkisi
```
b1 = yeniveriseti %>% group_by(BesteYapmaDurumu, Anxiety) %>% summarise(a = n())

PieDonut(b1, aes(BesteYapmaDurumu, Anxiety, count=a), title = "Beste Yapmanın Anksiyeteye Etkisi
         Donut Grafigi")
```     
![9](https://user-images.githubusercontent.com/91891099/210837001-1b97670d-e53f-4157-adb5-49e0db0b1039.png)

### Beste Yapmanın Depresyona Etkisi
```
b2 = yeniveriseti %>% group_by(BesteYapmaDurumu, Depression) %>% summarise(a = n())

PieDonut(b2, aes(BesteYapmaDurumu, Depression, count=a), title = "Beste Yapmanın Depresyona Etkisi
         Donut Grafigi")
```
![10](https://user-images.githubusercontent.com/91891099/210837060-d575a80c-1045-4d9b-a826-dd09f88d5eea.png)

### Beste Yapmanın Uykusuzluk Hastalığına Etkisi
```
b3 = yeniveriseti %>% group_by(BesteYapmaDurumu, Insomnia) %>% summarise(a = n())

PieDonut(b3, aes(BesteYapmaDurumu, Insomnia, count=a), title = "Beste Yapmanın Uykusuzluk Hastalığına Etkisi
         Donut Grafigi")
```
![11](https://user-images.githubusercontent.com/91891099/210837136-25ead47c-a117-4414-a6f5-2d98d1616492.png)

### Beste Yapmanın Obsesif Kompulsif Bozukluğa Etkisi
```
b4 = yeniveriseti %>% group_by(BesteYapmaDurumu, OCD) %>% summarise(a = n())

PieDonut(b4, aes(BesteYapmaDurumu, OCD, count=a), title = "Beste Yapmanın Obsesif Kompulsif Bozukluğa Etkisi
         Donut Grafigi")
```      
![12](https://user-images.githubusercontent.com/91891099/210837200-9a08f76b-5c8e-40e4-8c62-4298f2f049b6.png)

#### Sonuç
Bu kısımda elde edilen sonuçlara göre, insanların %82’si beste yaparken %18’i beste yapmamaktadır. Beste yapmayan insanların büyük bir çoğunluğunun anksiyete ve depresyon seviyeleri 7-8 civarındayken; uykusuzluk ve OKB seviyeleri ise 0-2 civarındadır. Beste yapan insanların büyük bir çoğunluğunun anksiyete ve depresyon seviyeleri 7-8 civarındayken, uykusuzluk ve OKB seviyeleri 0 ‘dır. Diğer bir deyişle, beste yapmanın mental hastalıklar üzerinde kesin bir etkisi yoktur. Bu seviyeler kişiden kişiye göre değişiklik göstermektedir.

## Günde Dinlenilen Müzik Süresinin(Saat) Mental Hastalıklara Etkisi
Günde dinlenilen müzik süresinin mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi Sacılım Grafigi Dikdörtgen Gruplama ile görselleştirilmiştir.

### Günde Dinlenilen Müzik Süresinin Anksiyete ile İlişkisi
```
ggplot(yeniveriseti, aes(Anxiety, `Hours per day`)) +
  geom_bin2d(bins = 10, color ="white")+
  scale_fill_gradient(low =  "#00AFBB", high = "#FC4E07")+
  theme_minimal()+
  labs(title = "Günde Dinlenilen Müzik Süresinin Anksiyeteye Etkisi",
       subtitle = "Saçılım Grafigi ile Dikdörtgen Gruplama",
       y= "Anksiyete Seviyeleri",
       x= "Günde Dinlenen Müzik Süresi(saat)",
       fill= "Kisi Sayısı")
```


### Günde Dinlenilen Müzik Süresinin Depresyon ile İlişkisi
```
ggplot(yeniveriseti, aes(Depression, `Hours per day`)) +
  geom_bin2d(bins = 10, color ="white")+
  scale_fill_gradient(low =  "#00AFBB", high = "#FC4E07")+
  theme_minimal() +
  labs(title = "Günde Dinlenilen Müzik Süresinin Depresyona Etkisi",
       subtitle = "Sacılım Grafigi ile Dikdörtgen Gruplama",
       y= "Depresyon Seviyeleri",
       x= "Günde Dinlenen Müzik Süresi(saat)",
       fill= "Kisi Sayısı")
 ```      

 
 ### Günde Dinlenilen Müzik Süresinin Uykusuzluk Hastalığı ile İlişkisi
 ```
 ggplot(yeniveriseti, aes(Insomnia, `Hours per day`)) +
  geom_bin2d(bins = 10, color ="white")+
  scale_fill_gradient(low =  "#00AFBB", high = "#FC4E07")+
  theme_minimal()+
  labs(title = "Günde Dinlenilen Müzik Süresinin Uykusuzluk Hastaligina Etkisi",
       subtitle = "Sacılım Grafigi ile Dikdörtgen Gruplama",
       y= "Uykusuzluk Hastaligi Seviyeleri",
       x= "Günde Dinlenen Müzik Süresi(saat)",
       fill= "Kisi Sayısı")
 ```      

 
 ### Günde Dinlenilen Müzik Süresinin Obsesif Kompulsif Bozukluk ile İlişkisi
 ```
 ggplot(yeniveriseti, aes(OCD, `Hours per day`)) +
  geom_bin2d(bins = 10, color ="white")+
  scale_fill_gradient(low =  "#00AFBB", high = "#FC4E07")+
  theme_minimal()+
  labs(title = "Günde Dinlenilen Müzik Süresinin Obsesif Kompulsif Bozukluga Etkisi",
       subtitle = "Sacılım Grafigi ile Dikdörtgen Gruplama",
       y= "Obsesif Kompulsif Bozukluga Seviyeleri",
       x= "Günde Dinlenen Müzik Süresi(saat)",
       fill= "Kisi Sayısı")
  ```
 







 

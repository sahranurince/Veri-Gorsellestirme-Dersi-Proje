# Veri-Gorsellestirme-Dersi-Proje
Eskişehir Teknik Üniversitesi İstatistik Bölümü lisans programında, 2022-2023 Öğretim Yılı - Güz Dönemi'nde yürütülen Veri Görselleştirme dersi projesinin materyallerini içermektedir.

# Gerekli Paketlerin İndirilmesi
#### install.packages("ggplot2")     # Veri görselleştirme aracı olarak kullanılır.
#### library(ggplot2)
#### install.packages("dplyr")     # Veri üzerinde manipülasyon yapmak için kullanılır.
#### library(dplyr)
#### install.packages("tidyr")     # Veri üzerinde düzenlemeler yapmak için kullanılır.
#### library(tidyr)
#### install.packages("webr")     # PieDonut Grafiğini çizmek için kullanılır.
#### library(webr)
#### install.packages("MetBrewer")     # Grafikler üzerinde renklendirme yapmak için kullanılır.
#### library(MetBrewer)

# Veri Setinin Kullanıma Hazırlanması

## Veri Setinin Çağırılması
#### library(readr)
#### mxmh_survey_results <- read_csv("mxmh_survey_results.csv")

## Yeni Veri Setinin Oluşturulması
#### yeniveriseti <- drop_na(mxmh_survey_results)     # drop_na() fonksiyonu veri setindeki eksik gözlemleri çıkarmak için kullanılır.

## Veri Setinin Kontrol Edilmesi
#### head(yeniveriseti)     # head() fonksiyonu veri setinin ilk altı satırını kontrol etmek için kullanılır.
#### tail(yeniveriseti)     # tail() fonksiyonu veri setinin son altı satırını kontrol etmek için kullanılır.
#### summary(yeniveriseti)     # summary() fonksiyonu veri setini özetlemek için kullanılır.(Değişkenlerin minimum, maksimum, ortalama, medyan, 1. çeyrek ve 3. çeyrek değerlerini gösterir.)
#### dim(yeniveriseti)     # dim() fonksiyonu veri setinin satır ve sütun sayılarını gösterir.
#### colnames(yeniveriseti)     #colnames() fonksiyonu veri setinin sütun isimlerini gösterir.

# Grafiklerin Çizilmesi

## Müzik Türlerinin Mental Hastalıklara Etkisi
Müzik türlerinin mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi çoklu çubuk grafiği ile görselleştirilmiştir. Görselleştirilmeden önce değişkenler üzerinde çeşitli işlemler uygulanmıştır.

### Müzik Türlerinin Anksiyeteye Etkisi
İlk olarak veri setini müzik türleri ve anksiyeteye göre gruplanıp, x ismi atanmıştır.
#### x <- yeniveriseti %>%
  group_by(`Fav genre`,Anxiety) %>%
  summarise(n1=n(),n2=n())

Burada gruplanan müzik türleri ve anksiyetenin oranları hesaplanıp, x2 ismi atanmıştır.
x2 <- x %>%
group_by(`Fav genre`) %>%
  summarize(anxietyoran = sum(Anxiety)/616)

Çoklu Çubuk Grafiğinin Çizilmesi
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
  
 ### Müzik Türlerinin Depresyona Etkisi

y <- yeniveriseti %>%
  group_by(`Fav genre`,Depression) %>%
  summarise(n1=n(),n2=n())

y2 <- y %>%
  group_by(`Fav genre`) %>%
  summarize(Depressionoran = sum(Depression)/616)
 
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
  
 ### Müzik Türlerinin Uykusuzluk Hastalığına Etkisi
 
 v <- yeniveriseti %>%
  group_by(`Fav genre`,Insomnia) %>%
  summarise(n1=n(),n2=n())

v2 <- v %>%
  group_by(`Fav genre`) %>%
  summarize(Insomniaoran = sum(Insomnia)/616)

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

 ### Müzik Türlerinin Obsesif Kompulsif Bozukluğa Etkisi
 
t <- yeniveriseti %>%
  group_by(`Fav genre`,OCD) %>%
  summarise(n1=n(),n2=n())

t2 <- t %>%
  group_by(`Fav genre`) %>%
  summarize(OCDoran = sum(OCD)/616)
  
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

## Enstrüman Çalmanın Mental Hastalıklara Etkisi
Enstrüman çalmanın mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi kernel yoğunluk grafiği ile görselleştirilmiştir.

### Enstrüman Çalmanın Anksiyeteye Etkisi

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

### Enstrüman Çalmanın Depresyona Etkisi

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
  
### Enstrüman Çalmanın Uykusuzluk Hastalığına Etkisi

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

### Enstrüman Çalmanın Obsesif Kompulsif Bozukluğa Etkisi

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

## Beste Yapmanın Mental Hastalıklara Etkisi
Beste yapmanın mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi Donut Grafiği ile görselleştirilmiştir. Görselleştirilmeden önce değişkenler üzerinde çeşitli işlemler uygulanmıştır.

Burada veri setinin sütun adı türkçeye çevrilmiştir.
colnames(yeniveriseti)[7]  <- "BesteYapmaDurumu"
Burada veri setinin sütun içindeki değişkenleri türkçeye çevrilmiştir.
yeniveriseti$BesteYapmaDurumu<- ifelse(yeniveriseti$BesteYapmaDurumu == "No", "Hayir", "Evet")

# Grafiklerin Çizilmesi

b1 = yeniveriseti %>% group_by(BesteYapmaDurumu, Anxiety) %>% summarise(a = n())

PieDonut(b1, aes(BesteYapmaDurumu, Anxiety, count=a), title = "Beste Yapmanın Anksiyeteye Etkisi
         Donut Grafigi")
b2 = yeniveriseti %>% group_by(BesteYapmaDurumu, Depression) %>% summarise(a = n())

PieDonut(b2, aes(BesteYapmaDurumu, Depression, count=a), title = "Beste Yapmanın Depresyona Etkisi
         Donut Grafigi")

b3 = yeniveriseti %>% group_by(BesteYapmaDurumu, Insomnia) %>% summarise(a = n())

PieDonut(b3, aes(BesteYapmaDurumu, Insomnia, count=a), title = "Beste Yapmanın Uykusuzluk Hastalığına Etkisi
         Donut Grafigi")

b4 = yeniveriseti %>% group_by(BesteYapmaDurumu, OCD) %>% summarise(a = n())

PieDonut(b4, aes(BesteYapmaDurumu, OCD, count=a), title = "Beste Yapmanın Obsesif Kompulsif Bozukluğa Etkisi
         Donut Grafigi")
 
  
  
  
  






 

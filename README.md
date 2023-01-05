# Veri-Gorsellestirme-Dersi-Proje
Eskişehir Teknik Üniversitesi İstatistik Bölümü lisans programında, 2022-2023 Öğretim Yılı - Güz Dönemi'nde yürütülen Veri Görselleştirme dersi projesinin materyallerini içermektedir.

# Gerekli Paketlerin İndirilmesi
# install.packages("ggplot2")     # Veri görselleştirme aracı olarak kullanılır.
#library(ggplot2)
# install.packages("dplyr")     # Veri üzerinde manipülasyon yapmak için kullanılır.
### library(dplyr)
### install.packages("tidyr")     # Veri üzerinde düzenlemeler yapmak için kullanılır.
### library(tidyr)
### install.packages("webr")     # PieDonut Grafiğini çizmek için kullanılır.
### library(webr)
### install.packages("MetBrewer")     # Grafikler üzerinde renklendirme yapmak için kullanılır.
### library(MetBrewer)

# Veri Setinin Kullanıma Hazırlanması

## Veri Setinin Çağırılması
library(readr)
mxmh_survey_results <- read_csv("mxmh_survey_results.csv")

## Yeni Veri Setinin Oluşturulması
yeniveriseti <- drop_na(mxmh_survey_results)     # drop_na() fonksiyonu veri setindeki eksik gözlemleri çıkarmak için kullanılır.

## Veri Setinin Kontrol Edilmesi
head(yeniveriseti)     # head() fonksiyonu veri setinin ilk altı satırını kontrol etmek için kullanılır.
tail(yeniveriseti)     # tail() fonksiyonu veri setinin son altı satırını kontrol etmek için kullanılır.
summary(yeniveriseti)     # summary() fonksiyonu veri setini özetlemek için kullanılır.(Değişkenlerin minimum, maksimum, ortalama, medyan, 1. çeyrek ve 3. çeyrek değerlerini gösterir.)
dim(yeniveriseti)     # dim() fonksiyonu veri setinin satır ve sütun sayılarını gösterir.
colnames(yeniveriseti)     #colnames() fonksiyonu veri setinin sütun isimlerini gösterir.

# Grafiklerin Çizilmesi

## Müzik Türlerinin Mental Hastalıklara Etkisi
Müzik türlerinin mental haslatıklara(Anksiyete,Depresyon,Uykusuzluk Hastalığı,Obsesif Kompulsif Bozukluk) etkisi çoklu çubuk grafiği ile görselleştirilmiştir. Görselleştirilmeden önce değişkenler üzerinde çeşitli işlemler uygulanmıştır.

İlk olarak veri setini müzik türleri ve anksiyeteye göre gruplanıp, x ismi atanmıştır.
x <- yeniveriseti %>%
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
  
  
  






 

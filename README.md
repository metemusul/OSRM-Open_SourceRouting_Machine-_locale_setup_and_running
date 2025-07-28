# OSRM(Open_SourceRouting_Machine)_locale_setup_and_running

OSRM (Open Source Routing Machine) Lokal Kurulum ve Çalıştırma Süreci
1. Docker Ortamını Hazırlama
Windows üzerinde Docker Desktop kuruldu ve çalıştırıldı.

Docker Engine’in aktif olduğu doğrulandı (docker info ile).

2. OSRM İmajını İndirme
Resmi OSRM backend Docker imajı indirildi:

bash
Kopyala
Düzenle
docker pull osrm/osrm-backend
3. Harita Verisinin İndirilmesi
Türkiye için OSM veri dosyası (.osm.pbf) indirildi:

Tarayıcı veya PowerShell ile Geofabrik’ten:
http://download.geofabrik.de/europe/turkey-latest.osm.pbf

4. Harita Verisinin OSRM Formatına Dönüştürülmesi
osrm-extract: Ham OSM verisi işlendi.

osrm-partition ve osrm-customize: Rota sorguları için hızlandırılmış veri yapıları oluşturuldu.

Komutlar PowerShell’de Windows uyumlu path ile çalıştırıldı:

powershell
Kopyala
Düzenle
docker run -t -v C:/Users/pc/Desktop/api_docker:/data osrm/osrm-backend osrm-extract -p /opt/car.lua /data/turkey-latest.osm.pbf
docker run -t -v C:/Users/pc/Desktop/api_docker:/data osrm/osrm-backend osrm-partition /data/turkey-latest.osrm
docker run -t -v C:/Users/pc/Desktop/api_docker:/data osrm/osrm-backend osrm-customize /data/turkey-latest.osrm
5. OSRM Routing Server’ı Çalıştırma
API servisi başlatıldı:

powershell
Kopyala
Düzenle
docker run -t -i -p 5000:5000 -v C:/Users/pc/Desktop/api_docker:/data osrm/osrm-backend osrm-routed --algorithm mld /data/turkey-latest.osrm
Servis http://localhost:5000 adresinde aktif hale geldi.

6. API’nin Test Edilmesi
Tarayıcı üzerinden rota sorgusu başarıyla yapıldı:

bash
Kopyala
Düzenle
http://localhost:5000/route/v1/driving/29.0,41.0;32.8,39.9?overview=false
JSON formatında mesafe (distance) ve süre (duration) bilgisi alındı.


